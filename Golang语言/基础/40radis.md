<center>Radis</center>





[toc]





## Radis

> Redis是一个开源的内存数据库，Redis提供了多种不同类型的数据结构，很多业务场景下的问题都可以很自然地映射到这些数据结构上。





### 1. 安装

> 下载安装radis. [官方](https://redis.io/download/)

> 安装库

```go
go get github.com/go-redis/redis/v8
```







### 2. 使用

> 使用

```go
package main

import (
	"context"
	"fmt"
	"time"

	"github.com/go-redis/redis/v8"
)

func main() {

	rdb := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // 密码
		DB:       0,  // 数据库
		PoolSize: 20, // 连接池大小
	})

	// 设置一个值
	err := rdb.Set(context.Background(), "key", "val", 0).Err()

	if err != nil {
		panic(err)
	}

	// 获取一个值
	val, err := rdb.Get(context.Background(), "key").Result()
	if err != nil {
		panic(err)
	}

	fmt.Printf("val: %v\n", val)

	// 设置一个带过期时间的值（10 秒钟后过期）
	err = rdb.Set(context.Background(), "key2", "value2", 10*time.Second).Err()
	if err != nil {
		panic(err)
	}

	time.Sleep(2 * time.Second)

	// 获取一个已经过期的值，返回 redis.Nil 错误
	val2, err := rdb.Get(context.Background(), "key2").Result()
	if err == redis.Nil {
		fmt.Println("key2 does not exist")
	} else if err != nil {
		panic(err)
	} else {
		fmt.Println("val2", val2)
	}
}

```

1. 删除

```go
// 删除一个或多个键
deletedKeys, err := rdb.Del(context.Background(), "key1", "key2").Result()
if err != nil {
    panic(err)
}
fmt.Println("Deleted keys:", deletedKeys)
```

2. 列表操作

```go
// 将一个或多个元素从列表左侧插入
pushedLeftCount, err := rdb.LPush(context.Background(), "list", "element1", "element2", "goer", "sex").Result()
if err != nil {
    panic(err)
}
fmt.Println("Pushed left count:", pushedLeftCount)

// 获取列表的长度
listLength, err := rdb.LLen(context.Background(), "list").Result()
if err != nil {
    panic(err)
}
fmt.Println("List length:", listLength)

// 获取列表中的元素范围
elements, err := rdb.LRange(context.Background(), "list", 0, -1).Result()
if err != nil {
    panic(err)
}
fmt.Println("List elements:", elements)
```

3. 哈希操作

```go
// 设置哈希字段值
err := rdb.HSet(context.Background(), "hash", "goer", "goer1233455.").Err()
if err != nil {
    panic(err)
}

// 获取哈希字段值
val, err := rdb.HGet(context.Background(), "hash", "goer").Result()
if err == redis.Nil {
    fmt.Println("Field does not exist")
} else if err != nil {
    panic(err)
} else {
    fmt.Println("Field value:", val)
}

// 批量设置哈希字段值
err = rdb.HMSet(context.Background(), "hash", map[string]interface{}{
    "py": "python",
    "go": "golang",
}).Err()
if err != nil {
    panic(err)
}

// 批量获取哈希字段值
vals, err := rdb.HMGet(context.Background(), "hash", "py", "go").Result()
if err != nil {
    panic(err)
}
for i, val := range vals {
    fmt.Printf("Field%d value: %v\n", i+1, val)
}

// 获取哈希所有字段及值
valss, err := rdb.HGetAll(context.Background(), "hash").Result()
if err != nil {
    panic(err)
}
for field, val := range valss {
    fmt.Printf("Field: %s, Value: %s\n", field, val)
}

// 获取哈希长度（字段数量）
length, err := rdb.HLen(context.Background(), "hash").Result()
if err != nil {
    panic(err)
}
fmt.Println("Hash length:", length)

// 删除一个或多个哈希字段
deletedCount, err := rdb.HDel(context.Background(), "hash", "field1", "field2").Result()
if err != nil {
    panic(err)
}
fmt.Println("Deleted fields:", deletedCount)
```

4. 集合操作

```go
ctx := context.Background()
// 添加单个元素到集合
err := rdb.SAdd(ctx, "set", "element1", "element2").Err()
if err != nil {
    panic(err)
}

// 获取集合中的所有元素
elements, err := rdb.SMembers(ctx, "set").Result()
if err != nil {
    panic(err)
}
fmt.Println("Set elements:", elements)

// 检查元素是否存在于集合中
exists, err := rdb.SIsMember(ctx, "set", "element1").Result()
if err != nil {
    panic(err)
}
if exists {
    fmt.Println("Element exists")
} else {
    fmt.Println("Element does not exist")
}

// 获取集合的元素数量
count, err := rdb.SCard(ctx, "set").Result()
if err != nil {
    panic(err)
}
fmt.Println("Set size:", count)

// 从集合中移除单个元素
err = rdb.SRem(ctx, "set", "element1", "element2").Err()
if err != nil {
    panic(err)
}
```

5. 有序集合

```go
ctx := context.Background()
// 添加单个元素到有序集合
err := rdb.ZAdd(ctx, "sortedSet", &redis.Z{
    Score:  1.0,
    Member: "element1",
}).Err()
if err != nil {
    panic(err)
}

// 添加多个元素到有序集合
elements := []*redis.Z{
    {Score: 2.0, Member: "element2"},
    {Score: 3.0, Member: "element3"},
}
err = rdb.ZAdd(ctx, "sortedSet", elements...).Err()
if err != nil {
    panic(err)
}

// 获取有序集合中的所有元素
elementss, err := rdb.ZRange(ctx, "sortedSet", 0, -1).Result()
if err != nil {
    panic(err)
}
fmt.Println("Sorted set elements:", elementss)

// 获取有序集合中元素的排名
rank, err := rdb.ZRank(ctx, "sortedSet", "element3").Result()
if err != nil {
    panic(err)
}
fmt.Println("Element rank:", rank)

// 获取有序集合中元素的分值
score, err := rdb.ZScore(ctx, "sortedSet", "element1").Result()
if err != nil {
    panic(err)
}
fmt.Println("Element score:", score)

// 从有序集合中移除单个元素
err = rdb.ZRem(ctx, "sortedSet", "element1").Err()
if err != nil {
    panic(err)
}

// 从有序集合中移除多个元素
elementsq := []string{"element2", "element3"}
err = rdb.ZRem(ctx, "sortedSet", elementsq).Err()
if err != nil {
    panic(err)
}
```







