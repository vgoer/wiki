<center> laravel-jwt</center>







[toc]







## laravel-jwt

> Laravel 和 Lumen 的 JSON Web 令牌身份验证。 [jwt](https://jwt-auth.readthedocs.io/en/develop/)





### 1. 安装配置

> 1. composer安装

```shell
composer require tymon/jwt-auth
```

> 2. 提供服务商
>
> 将服务提供者添加到配置文件`providers`中的数组中，`config/app.php`如下所示：

```shell
'providers' => [

    ...

    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
]
```

> 3. 发布配置

```shell
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

> 4. 生成密钥

```shell
php artisan jwt:secret
```







### 2. 快速开始

> 首先，您需要在`User model`上实现`Tymon\JWTAuth\Contracts\JWTSubject`合同，这要求您实现2种方法`getJWTIdentifier()`和`getJWTCustomClaims()`

```php
<?php

namespace App;

use Tymon\JWTAuth\Contracts\JWTSubject;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable implements JWTSubject
{
    use Notifiable;

    /**
     * Get the identifier that will be stored in the subject claim of the JWT.
     *
     * @return mixed
     */
    public function getJWTIdentifier()
    {
        return $this->getKey();//默认取的是主键id：$this->id
    }

    /**
     * Return a key value array, containing any custom claims to be added to the JWT.
     *
     * @return array
     */
    public function getJWTCustomClaims()
    {
        //一些附加信息，不要太敏感，因为这些信息是可以从token里解析出来的，即使不知道签名
        return ['name'=>$this->name, 'email'=>$this->email];
    }
}
```

> 配置看守器: 在`config/auth.php`文件中。

```php
'defaults' => [
    'guard' => 'api',
    'passwords' => 'users',
],

...

'guards' => [
    'api' => [
        'driver' => 'jwt',
        'provider' => 'users',
    ],
],
```

> 添加路由：身份认证的

```php
Route::group(['middleware' => 'api', 'prefix' => 'auth/jwt'], function () {
    Route::post('login', [AuthJWTController::class, 'login']);
    Route::post('logout', [AuthJWTController::class, 'logout']);
    Route::post('refresh', [AuthJWTController::class, 'refresh']);
    Route::get('me', [AuthJWTController::class, 'me']);
});
```

> 控制器： 

```shell
php artisan make:controller AuthJWTController
```

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\Auth;
use App\Http\Controllers\Controller;

class AuthController extends Controller
{
    /**
     * Create a new AuthController instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth:api', ['except' => ['login']]);
    }

    /**
     * Get a JWT via given credentials.
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
     * Get the authenticated User.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function me()
    {
        return response()->json(auth()->user());
    }

    /**
     * Log the user out (Invalidate the token).
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function logout()
    {
        auth()->logout();

        return response()->json(['message' => 'Successfully logged out']);
    }

    /**
     * Refresh a token.
     *
     * @return \Illuminate\Http\JsonResponse
     */
    public function refresh()
    {
        return $this->respondWithToken(auth()->refresh());
    }

    /**
     * Get the token array structure.
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

> 这个文章： [jwt](https://blog.csdn.net/weixin_45310179/article/details/120989859)