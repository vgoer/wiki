<center> laravel-jwt</center>







[toc]







## laravel-jwt

> Laravel 和 Lumen 的 JSON Web 令牌身份验证。 [jwt](https://jwt-auth.readthedocs.io/en/develop/)
>
> 这个文章： [jwt](https://blog.csdn.net/weixin_45310179/article/details/120989859)





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
Route::group([
    'prefix' => 'auth'
], function ($router) {
    Route::post('login', [App\Http\Controllers\AuthJWTController::class, 'login']);
    
    Route::group(['middleware'=>'refresh.token'], function(){
        Route::post('logout', [App\Http\Controllers\AuthJWTController::class, 'logout']);
        Route::post('refresh', [App\Http\Controllers\AuthJWTController::class, 'refresh']);
        Route::get('me', [App\Http\Controllers\AuthJWTController::class, 'me']);
    });

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
     * Get a JWT via given credentials.
     *
     * @return \Illuminate\Http\JsonResponse
     */
/**
     * @param Request $request
     * @return \Illuminate\Http\JsonResponse
     */
    public function login(Request $request)
    {
        $data = $request->input();
        
        $phone = $data['email'];
		//以手机号登录测试，具体根据自己的业务逻辑
        $user = User::where(['email' => $phone])->first();

        if(!$user){
            $user = new User();
            $user->phone = $phone;
            $user->save();
        }

        //方式一
        // $token = JWTAuth::fromUser($user);
        //方式二
        $token = auth('api')->login($user);

        //方式三、下面这种方式必须使用密码登录
        // $token = auth('api')->attempt($user->toArray());

        if(!$token){
            return response()->json(['error' => 'Unauthorized'],401);
        }

        return $this->respondWithToken($token,$user);

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
            'data' => $data,
            'access_token' =>'bearer '.$token,
            'token_type' => 'bearer'
        ]);
    }
}
```

> `http://127.0.0.1:777/api/auth/login`: 访问带上email字段

```shell
{
    "data": {
        "id": 1,
        "name": "goer",
        "email": "goer@qq.com",
        "email_verified_at": null,
        "created_at": null,
        "updated_at": null
    },
    "access_token": "bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vMTI3LjAuMC4xOjc3Ny9hcGkvYXV0aC9sb2dpbiIsImlhdCI6MTY5OTYxMDM4NSwiZXhwIjoxNjk5NjEzOTg1LCJuYmYiOjE2OTk2MTAzODUsImp0aSI6ImJTUVRVOHR6VFg4dGNvaDgiLCJzdWIiOiIxIiwicHJ2IjoiMjNiZDVjODk0OWY2MDBhZGIzOWU3MDFjNDAwODcyZGI3YTU5NzZmNyIsIm5hbWUiOiJnb2VyIiwiZW1haWwiOiJnb2VyQHFxLmNvbSJ9.zyX5DRbwDbRKJ7ZnM01N8G--BGt-PkzurznUUSUOY7Y",
    "token_type": "bearer"
}
```

> 登入逻辑修改

```php
/**
     * 管理员登录接口
     *
     * @return void
     */
    public function login(Request $request)
    {
        $params = $request->all();

        if (empty($params['account'])) {
            return error(500100);
        }
        if (empty($params['password'])) {
            return error(500101);
        }

        $count = Admin::query()
                ->where('account', $params['account'])
                ->count();
        if ($count != 1) {
            return error(500102);
        }
        $admin = Admin::query()
                ->where('account', $params['account'])
                ->first();

        // 验证密码是否正确
        if ((! Hash::check($params['password'].$admin->salt, $admin->password))) {
            return error(500103);
        }

        $admin->token = encrypt(mTime());
        $res = $admin->save();
        if ($res) {
            return success(['token' => $admin->token]);
        }

        return error();
    }
```



> 生成中间件： 

```shell
php artisan make:middleware RefreshToken
```

> 中间件逻辑

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Symfony\Component\HttpKernel\Exception\UnauthorizedHttpException;
use Tymon\JWTAuth\Exceptions\JWTException;
use Tymon\JWTAuth\Exceptions\TokenExpiredException;
use Tymon\JWTAuth\Http\Middleware\BaseMiddleware;

class RefreshToken extends BaseMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure(\Illuminate\Http\Request): (\Illuminate\Http\Response|\Illuminate\Http\RedirectResponse)  $next
     * @return \Illuminate\Http\Response|\Illuminate\Http\RedirectResponse
     */
    public function handle(Request $request, Closure $next)
    {
        //检查此次请求中，是否携带token，如果没有则抛出异常
        $this->checkForToken($request);

        try{
            //检测用户登录状态，如果正常，则通过
            if($this->auth->parseToken()->authenticate()){
                return $next($request);
            }

            throw new UnauthorizedHttpException('jwt-auth',json_encode(['status' => 401,'msg' => '未登录']));

        }catch (TokenExpiredException $exception){
            
            // 此处捕获到了 token 过期所抛出的 TokenExpiredException 异常，我们在这里需要做的是刷新该用户的 token 并将它添加到响应头中
            try{
                //刷新用户token，并放到头部
                $token = $this->auth->refresh();

                //使用一次性登录，保证请求成功
                Auth::guard('api')->onceUsingId($this->auth->manager()->getPayloadFactory()->buildClaimsCollection()->toPlainArray()['sub']);

            }catch (JWTException $exception){
                //如果走到这里，说明refresh也过期了，需要重新登录

                throw new UnauthorizedHttpException('jwt-auth',json_encode(['status' => 401 , 'msg' => '未登录']));
            }
        }

        //在响应头中返回新的token

        return $this->setAuthenticationHeader($next($request),$token);

        return $next($request);
    }
}
```

> 注册新增的中间件到Kernel.php文件中的$routeMiddleware数组中

```php
'refresh.token' => \App\Http\Middleware\RefreshToken::class,
```

> 修改路由文件，写入中间件

> 测试： 打开apifox

```shell
Authorization   =>  bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vMTI3LjAuMC4xOjc3Ny9hcGkvYXV0aC9sb2dpbiIsImlhdCI6MTY5OTYxNzM4OCwiZXhwIjoxNjk5NjIwOTg4LCJuYmYiOjE2OTk2MTczODgsImp0aSI6IlVqUnpqN1dhWmQ3Q1BTbHciLCJzdWIiOiIxIiwicHJ2IjoiMjNiZDVjODk0OWY2MDBhZGIzOWU3MDFjNDAwODcyZGI3YTU5NzZmNyIsIm5hbWUiOiJnb2VyIiwiZW1haWwiOiJnb2VyQHFxLmNvbSJ9.1MnKgyVxym0M7QWwosI6Srmq6PlWGqP2PkXMmQsrJrk
```



> 我去，牛皮牛皮。
