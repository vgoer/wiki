<center>Webman Translation</center>





[toc]









## Webman Translation

> 站在巨人（laravel）的肩膀上使本地化使用更加*可靠*和*便捷*
>
> 所有方法和配置与 laravel 几乎一模一样，因此使用方式完全参考 [Laravel文档](http://laravel.p2hp.com/cndocs/8.x/translation) 即可
>
> [blog](https://www.workerman.net/plugin/90)









### 1. 安装

```shell
composer require webman-tech/laravel-translation
```







### 2. 使用

>  修改配置文件 `conig/plugin/webman-tech/laravel-translation/app.php`

```php
<?php
return [
    'enable' => true,
    
    // 默认语言
    'locale' => 'zh_CN',
    
    // 备用语言
    'fallback_locale' => 'en',
    
    // 语言文件存放路径
    'path' => base_path() . '/resource/translations',
    
    // 是否缓存语言文件
    'cache' => true,
    
    // 缓存时间(秒)
    'cache_time' => 3600,
];
```

> 文件目录

```shell
resource/translations/
    ├── en/
    │   ├── messages.php
    │   ├── validation.php
    │   └── auth.php
    └── zh_CN/
        ├── messages.php
        ├── validation.php
        └── auth.php
```

```php
# zh_CN/messages.php
<?php
return [
    'welcome' => '欢迎访问我们的网站',
    'hello' => '你好 :name',
    'nested' => [
        'message' => '这是一个嵌套消息'
    ]
];
```

```php
# en/messages.php
return [
    'welcome' => 'Welcome to our website',
    'hello' => 'Hello :name',
    'nested' => [
        'message' => 'This is a nested message'
    ]
];

```

> 使用，在项目中

```php
    public function index(Request $request)
    {
        // 设置当前语言
        Translation::setLocale('zh_CN');

        // 获取当前语言
        $currentLocale = Translation::getLocale();

        // echo $currentLocale;

        $message1 = transL('messages.welcome');
        $message2 = trans_choice('messages.hello', 2);
        $message3 = __('messages.nested');

        return json([
            $message1, $message2, $message3
        ]);
    }
```

> 推荐使用函数`__`获取翻译。

> 手动切换`locale`: 
>
> 因此本扩展基于此原因，已经做到了根据 `locale()` 自动切换 `transL()` `trans_choice()` `__()` 下使用的语言包，无需开发手动设置