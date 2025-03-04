<center>Webman jwt</center>





[toc]







## Webman jwt

> jwt认证：
>
> [Json web token](https://jwt.io/) 是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（(RFC 7519)。该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。







### 1. 安装

> [blog](https://www.workerman.net/plugin/10)

```shell
composer require tinywan/jwt
```







### 2. 配置文件

> `config/tinywan/jwt/app.php`

```php
<?php

return [
    'enable' => true,
    'jwt' => [
        /** 算法类型 HS256、HS384、HS512、RS256、RS384、RS512、ES256、ES384、Ed25519 */
        'algorithms' => 'HS256',

        /** access令牌秘钥 */
        'access_secret_key' => '2022d3d3LmJq',

        /** access令牌过期时间，单位：秒。默认 2 小时 */
        'access_exp' => 7200,

        /** refresh令牌秘钥 */
        'refresh_secret_key' => '2022KTxigxc9o50c',

        /** refresh令牌过期时间，单位：秒。默认 7 天 */
        'refresh_exp' => 604800,

        /** refresh 令牌是否禁用，默认不禁用 false */
        'refresh_disable' => false,

        /** 令牌签发者 */
        'iss' => 'webman.tinywan.cn',

        /** 某个时间点后才能访问，单位秒。（如：30 表示当前时间30秒后才能使用） */
        'nbf' => 0,

        /** 时钟偏差冗余时间，单位秒。建议这个余地应该不大于几分钟 */
        'leeway' => 60,

        /** 是否允许单设备登录，默认不允许 false */
        'is_single_device' => false,

        /** 缓存令牌时间，单位：秒。默认 7 天 */
        'cache_token_ttl' => 604800,

        /** 缓存令牌前缀，默认 JWT:TOKEN: */
        'cache_token_pre' => 'JWT:TOKEN:',

        /** 缓存刷新令牌前缀，默认 JWT:REFRESH_TOKEN: */
        'cache_refresh_token_pre' => 'JWT:REFRESH_TOKEN:',

        /** 用户信息模型 */
        'user_model' => function ($uid) {
            return [];
        },

        /** 是否支持 get 请求获取令牌 */
        'is_support_get_token' => false,
        /** GET 请求获取令牌请求key */
        'is_support_get_token_key' => 'authorization',

        /** access令牌私钥 */
        'access_private_key' => <<<EOD
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
EOD,

        /** access令牌公钥 */
        'access_public_key' => <<<EOD
-----BEGIN PUBLIC KEY-----
...
-----END PUBLIC KEY-----
EOD,

        /** refresh令牌私钥 */
        'refresh_private_key' => <<<EOD
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
EOD,

        /** refresh令牌公钥 */
        'refresh_public_key' => <<<EOD
-----BEGIN PUBLIC KEY-----
...
-----END PUBLIC KEY-----
EOD,
    ],
];

```

> 生成控制器

```php
php webman make:controller AuthJwtController
```

```php
<?php

namespace app\controller;

use app\model\Admin;
use support\Request;
use Tinywan\Jwt\JwtToken;

class AuthJwtController
{
    public function index(Request $request)
    {
        return response(__CLASS__);
    }



    public function authLogin(Request $request)
    {
        $params = $request->all();
        
        // 数据处理登陆逻辑
        $userInfo = Admin::query()
            ->where("account", $params["account"])
            ->where("password", $params["password"])
            ->first();

        // jwt
        // $res = JwtToken::generateToken($userInfo->toArray());

        // 数据
        $res = array_merge(JwtToken::generateToken($userInfo->toArray()), $userInfo->toArray());

        return json_encode([
            "code" => 200,
            "msg"  => "获取成功",
            "data" => $res
        ]);
    }


    public function getInfo(Request $request)
    {
        // 接口必须携带jwt token
        $uid = JwtToken::getCurrentId();

        // 获取其他值
        $account = JwtToken::getExtendVal("account");
        return json_encode([
            "code"  => 200,
            "msg"   => "获取成功",
            "uid"  => $uid,
            "account" => $account,
        ]);
    }

    // 过期后需要重新请求
    public function refresh(Request $request)
    {
        $data = JwtToken::refreshToken();
        return json_encode([
            "code"  => 200,
            "msg"   => "获取成功",
            "data" => $data,
        ]);
    }

}


```

> `apifox`添加请求头

```shell
Authorization   =  Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJ3ZWJtYW4udGlueXdhbi5jbiIsImF1ZCI6IndlYm1hbi50aW55d2FuLmNuIiwiaWF0IjoxNzQxMDE5MjQxLCJuYmYiOjE3NDEwMTkyNDEsImV4cCI6MTc0MTAyNjQ0MSwiZXh0ZW5kIjp7ImlkIjoxLCJhY2NvdW50IjoiYWRtaW4iLCJlbWFpbCI6bnVsbCwidHJ1ZW5hbWUiOm51bGwsImF2YXRhciI6bnVsbCwibW9iaWxlIjpudWxsLCJwYXNzd29yZCI6IjEyMzQ1NiIsInNhbHQiOm51bGwsInRva2VuIjpudWxsLCJyb2xlcyI6bnVsbCwic3RhdHVzIjowLCJjcmVhdGVkX2F0IjpudWxsLCJ1cGRhdGVkX2F0IjpudWxsfX0.NI72PN12o11_3xdJLOmLky0hmFJnH6idubl44XyA_94
```

> 路由文件： `router.php`

```php
Route::any("/authLogin", [app\controller\AuthJwtController::class, "authLogin"]);
// 接口必须携带jwt token
Route::any("/getInfo", [app\controller\AuthJwtController::class, "getInfo"]);
// 需要携带jwt 刷新 token
Route::any("/refresh", [app\controller\AuthJwtController::class, "refresh"]);
```





### 2. 算法

> 其他算法
>
> RS256算法。

```php
    'enable' => true,
    'jwt' => [
        /** 算法类型 HS256、HS384、HS512、RS256、RS384、RS512、ES256、ES384、Ed25519 */
        'algorithms' => 'RS256',
```

```shell
# 生成密钥对
ssh-keygen -t rsa -b 4096 -E SHA256 -m PEM -P "" -f RS256.key
openssl rsa -in RS256.key -pubout -outform PEM -out RS256.key.pub
```

> 生成了私钥和公钥，替换到配置文件中

```php
        /** access令牌私钥 */
        'access_private_key' => <<<EOD
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
EOD,

        /** access令牌公钥 */
        'access_public_key' => <<<EOD
-----BEGIN PUBLIC KEY-----
...
-----END PUBLIC KEY-----
EOD,

        /** refresh令牌私钥 */
        'refresh_private_key' => <<<EOD
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
EOD,

        /** refresh令牌公钥 */
        'refresh_public_key' => <<<EOD
-----BEGIN PUBLIC KEY-----
...
-----END PUBLIC KEY-----
EOD,
```

> 执行接口，`就生成了RS256加密的jwt了`
