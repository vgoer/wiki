<center>socialite社会化登录</center>









## socialite社会化登录

>  Socialite 目前支持 Facebook，Twitter，LinkedIn，Google，GitHub，GitLab 和 Bitbucket 的身份验证。
>
> 你可以轻松地集成各种社交平台（如Facebook、Twitter、GitHub等）的登录功能。







### 1. 安装

> 依赖

```shell
composer require laravel/socialite
```





### 2. 配置

> `.env` 添加依赖

```shell
# github
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
GITHUB_REDIRECT_URI=

# facebook
FACEBOOK_CLIENT_ID=your-facebook-client-id
FACEBOOK_CLIENT_SECRET=your-facebook-client-secret
FACEBOOK_REDIRECT_URI=your-callback-url
```

> 这些凭证应该放在你的 `config/services.php`

```php
'github' => [
    'client_id' => env('GITHUB_CLIENT_ID'),
    'client_secret' => env('GITHUB_CLIENT_SECRET'),
    'redirect' => env("GITHUB_REDIRECT_URI"),
],

```









### 3. 认证

> 路由

```php
use Laravel\Socialite\Facades\Socialite;

Route::get('/auth/redirect', function () {
    return Socialite::driver('github')->redirect();
});

Route::get('/auth/callback', function () {
    $user = Socialite::driver('github')->user();

    // $user->token
});
```

> ### 身份验证和存储

```php
use App\Models\User;
use Illuminate\Support\Facades\Auth;
use Laravel\Socialite\Facades\Socialite;

Route::get('/auth/callback', function () {
    $githubUser = Socialite::driver('github')->user();

    $user = User::updateOrCreate([
        'github_id' => $githubUser->id,
    ], [
        'name' => $githubUser->name,
        'email' => $githubUser->email,
        'github_token' => $githubUser->token,
        'github_refresh_token' => $githubUser->refreshToken,
    ]);

    Auth::login($user);

    return redirect('/dashboard');
});
```

