<center>Token续签&分布式锁</center>





[toc]







## Token续签&分布式锁

> 如果将 token 的有效期时间设置过短，到期后用户需要重新登录，过于繁琐且体验感差，这里我将采用服务端刷新 token 的方式来处理。先规定一个时间点，比如在过期前的 2 小时内，如果用户访问了接口，就颁发新的 token 给客户端（设置响应头），同时把旧 token 加入黑名单，在上一篇中，设置了一个黑名单宽限时间，目的就是避免并发请求中，刷新了 token ，导致部分请求失败的情况；同时，我们也要避免并发请求导致 token 重复刷新的情况，这时候就需要上锁了，这里使用了 Redis 来实现，考虑到以后项目中可能会频繁使用锁，在篇头将简单做个封装







### 封装分布式锁

> 新建 `utils/str.go` ，编写 `RandString()` 用于生成锁标识，防止任何客户端都能解锁

```go
package utils

import (
    "math/rand"
    "time"
)

func RandString(len int) string {
    r := rand.New(rand.NewSource(time.Now().UnixNano()))
    bytes := make([]byte, len)
    for i := 0; i < len; i++ {
        b := r.Intn(26) + 65
        bytes[i] = byte(b)
    }
    return string(bytes)
}

```

> 新建 `global/lock.go` ，编写

```go
package global

import (
    "context"
    "github.com/go-redis/redis/v8"
    "jassue-gin/utils"
    "time"
)

type Interface interface {
    Get() bool
    Block(seconds int64) bool
    Release() bool
    ForceRelease()
}

type lock struct {
    context context.Context
    name string // 锁名称
    owner string // 锁标识
    seconds int64 // 有效期
}

// 释放锁 Lua 脚本，防止任何客户端都能解锁
const releaseLockLuaScript = `
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
`

// 生成锁
func Lock(name string, seconds int64) Interface {
    return &lock{
        context.Background(),
        name,
        utils.RandString(16),
        seconds,
    }
}

// 获取锁
func (l *lock) Get() bool {
    return App.Redis.SetNX(l.context, l.name, l.owner, time.Duration(l.seconds)*time.Second).Val()
}

// 阻塞一段时间，尝试获取锁
func (l *lock) Block(seconds int64) bool {
    starting := time.Now().Unix()
    for {
        if !l.Get() {
            time.Sleep(time.Duration(1) * time.Second)
            if time.Now().Unix()-seconds >= starting {
                return false
            }
        } else {
            return true
        }
    }
}

// 释放锁
func (l *lock) Release() bool {
    luaScript := redis.NewScript(releaseLockLuaScript)
    result := luaScript.Run(l.context, App.Redis, []string{l.name}, l.owner).Val().(int64)
    return result != 0
}

// 强制释放锁
func (l *lock) ForceRelease() {
    App.Redis.Del(l.context, l.name).Val()
}
```







### 定义配置项

> 在 `config/jwt.go` 中，增加 `RefreshGracePeriod` 属性

```go
package config

type Jwt struct {
    Secret string `mapstructure:"secret" json:"secret" yaml:"secret"`
    JwtTtl int64 `mapstructure:"jwt_ttl" json:"jwt_ttl" yaml:"jwt_ttl"` // token 有效期（秒）
    JwtBlacklistGracePeriod int64 `mapstructure:"jwt_blacklist_grace_period" json:"jwt_blacklist_grace_period" yaml:"jwt_blacklist_grace_period"` // 黑名单宽限时间（秒）
    RefreshGracePeriod int64 `mapstructure:"refresh_grace_period" json:"refresh_grace_period" yaml:"refresh_grace_period"` // token 自动刷新宽限时间（秒）
}
```

> `config.yaml` 添加对应配置

```go
jwt:
  refresh_grace_period: 1800
```





### 在 jwt 中间件中增加续签机制

> 在 `app/services/jwt.go` 中，编写 `GetUserInfo()`， 根据不同客户端 token ，查询不同用户表数据

```go
func (jwtService *jwtService) GetUserInfo(GuardName string, id string) (err error, user JwtUser) {
    switch GuardName {
    case AppGuardName:
        return UserService.GetUserInfo(id)
    default:
        err = errors.New("guard " + GuardName +" does not exist")
    }
    return
}
```

> 在 `app/middleware/jwt.go` 中，编写

```go

func JWTAuth(GuardName string) gin.HandlerFunc {
    return func(c *gin.Context) {
      
        //...
        claims := token.Claims.(*services.CustomClaims)
        if claims.Issuer != GuardName {
            response.TokenFail(c)
            c.Abort()
            return
        }

        // token 续签
        if claims.ExpiresAt-time.Now().Unix() < global.App.Config.Jwt.RefreshGracePeriod {
            lock := global.Lock("refresh_token_lock", global.App.Config.Jwt.JwtBlacklistGracePeriod)
            if lock.Get() {
                err, user := services.JwtService.GetUserInfo(GuardName, claims.Id)
                if err != nil {
                    global.App.Log.Error(err.Error())
                    lock.Release()
                } else {
                    tokenData, _, _ := services.JwtService.CreateToken(GuardName, user)
                    c.Header("new-token", tokenData.AccessToken)
                    c.Header("new-expires-in", strconv.Itoa(tokenData.ExpiresIn))
                    _ = services.JwtService.JoinBlackList(token)
                }
            }
        }

        c.Set("token", token)
        c.Set("id", claims.Id)
    }
}
```

> 修改 `config.yaml` 配置，暂时将 `refresh_grace_period` 设置一个较大的值，确保能满足续签条件

```yaml
jwt:
  secret: 3Bde3BGEbYqtqyEUzW3ry8jKFcaPH17fRmTmqE7MDr05Lwj95uruRKrrkb44TJ4s
  jwt_ttl: 43200
  jwt_blacklist_grace_period: 10
  refresh_grace_period: 43200

```

> 测试：apifox