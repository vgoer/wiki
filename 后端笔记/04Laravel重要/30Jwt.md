<center>Jwt认证</center>



[toc]





## JWT

> 要在 Laravel 中使用 JWT（JSON Web Token）进行身份验证  [blog](https://blog.csdn.net/m0_50593634/article/details/118001170) [doc](https://learnku.com/docs/jwt-auth) [learnku](https://learnku.com/articles/70414)





### 1. 安装

```php
composer require tymon/jwt-auth
    
    
//这条命令会在 config 下增加一个 jwt.php 的配置文件
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"

# 生成密钥 .env
php artisan jwt:secret
```

> 打开 `config/auth.php` 文件，在 `guards` 数组中添加 `'api'` guard，并确保将其驱动程序设置为 `'jwt'`：

```php
'guards' => [
    // 其他 guards...
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
```





### 2. 使用

> 更新模型，修改模型代码
>
> 首先，您需要在 User 模型上实现 Tymon\JWTAuth\Contracts\JWTSubject 契约，这需要您实现 getJWTIdentifier() 和 getJWTCustomClaims() 两个方法。
>

```php
# user模型
<?php

namespace App;

use Tymon\JWTAuth\Contracts\JWTSubject;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable implements JWTSubject
{
    use Notifiable;

    // 为了简洁省略其余部分

    /**
     * 获取将存储在 JWT 主题声明中的标识符。
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();
    }

    /**
     * 返回一个键值数组，其中包含要添加到 JWT 的任何自定义声明。
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        return [];
    }
}
```

> 添加认证路由

```php
Route::group([

    'middleware' => 'api',
    'prefix' => 'auth'

], function ($router) {

    Route::post('login', 'AuthController@login');
    Route::post('logout', 'AuthController@logout');
    Route::post('refresh', 'AuthController@refresh');
    Route::post('me', 'AuthController@me');

});
```

> 添加认证控制器

```php
php artisan make:controller AuthController
```

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Auth;
use App\Http\Controllers\Controller;

class AuthController extends Controller
{
    /**
     * 创建一个新的 AuthController 实例。
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth:api', ['except' => ['login']]);
    }

    /**
     * 通过给定的凭证获取 JWT。
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function login()
    {
        $credentials = request(['email', 'password']);

        if (! $token = auth()->attempt($credentials)) {
            return response()->json(['error' => 'Unauthorized'], 401);
        }

        return $this->respondWithToken($token);
    }

    /**
     * 获取已认证的用户。
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function me()
    {
        return response()->json(auth()->user());
    }

    /**
     * 注销用户（作废 token）。
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function logout()
    {
        auth()->logout();

        return response()->json(['message' => 'Successfully logged out']);
    }

    /**
     * 刷新 token。
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function refresh()
    {
        return $this->respondWithToken(auth()->refresh());
    }

    /**
     * 获取 token 数组结构。
     *
     * @param  string $token
     *
     * @return \Illuminate\Http\JsonResponse
     */
    protected function respondWithToken($token)
    {
        return response()->json([
            'access_token' => $token,
            'token_type' => 'bearer',
            'expires_in' => auth()->factory()->getTTL() * 60
        ]);
    }
}
```



> by: [goer](https://vgoer.github.io/zh/notes/end/study/jwt/#jwt)

