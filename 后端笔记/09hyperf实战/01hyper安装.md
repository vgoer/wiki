<center>hyperf安装</center>





[toc]







## hyperf

> Hyperf [hyperf](https://hyperf.wiki/) 是一个高性能、高灵活性的渐进式 PHP 协程框架，内置协程服务器及大量常用的组件，性能较传统基于 `PHP-FPM` 的框架有质的提升，提供超高性能的同时，也保持着极其灵活的可扩展性，标准组件均基于 [PSR 标准](https://www.php-fig.org/psr) 实现，基于强大的依赖注入设计，保证了绝大部分组件或类都是 `可替换` 与 `可复用` 的。









### 1. 安装

> Hyperf 对系统环境有一些要求，当您使用 Swoole 网络引擎驱动时.
>
> windows不能直接安装，需要使用`wsl`

```shell
# docker
docker run --name hyperf \
-v /workspace/skeleton:/data/project \
-p 9501:9501 -it \
--privileged -u root \
--entrypoint /bin/sh \
hyperf/hyperf:8.1-alpine-v3.18-swoole
```

> 进入目录，启动服务

```shell
cd /data/project
composer create-project hyperf/hyperf-skeleton

cd hyperf-skeleton
php bin/hyperf.php start
```

> 可以使用的命令 和 `artisan`类似

```shell
php bin/hyperf.php start
```

> 启动： `ip:9501` 我去，看到了。









### 2. 路由

> `config/routes.php`下面是一些常用的用法示例。

```php
use Hyperf\HttpServer\Router\Router;
// 此处代码示例为每个示例都提供了三种不同的绑定定义方式，实际配置时仅可采用一种且仅定义一次相同的路由

// 设置一个 GET 请求的路由，绑定访问地址 '/get' 到 App\Controller\IndexController 的 get 方法
Router::get('/get', 'App\Controller\IndexController::get');
Router::get('/get', 'App\Controller\IndexController@get');
Router::get('/get', [\App\Controller\IndexController::class, 'get']);

// 设置一个 POST 请求的路由，绑定访问地址 '/post' 到 App\Controller\IndexController 的 post 方法
Router::post('/post', 'App\Controller\IndexController::post');
Router::post('/post', 'App\Controller\IndexController@post');
Router::post('/post', [\App\Controller\IndexController::class, 'post']);

// 设置一个允许 GET、POST 和 HEAD 请求的路由，绑定访问地址 '/multi' 到 App\Controller\IndexController 的 multi 方法
Router::addRoute(['GET', 'POST', 'HEAD'], '/multi', 'App\Controller\IndexController::multi');
Router::addRoute(['GET', 'POST', 'HEAD'], '/multi', 'App\Controller\IndexController@multi');
Router::addRoute(['GET', 'POST', 'HEAD'], '/multi', [\App\Controller\IndexController::class, 'multi']);
```



> 路由注解

> 1. ``#[AutoController]`

```php
use Hyperf\HttpServer\Annotation\AutoController;
use Hyperf\HttpServer\Contract\RequestInterface;

#[AutoController]
class IndexController extends AbstractController
{
    // Hyperf 会自动为此方法生成一个 /index/index 的路由，允许通过 GET 或 POST 方式请求
    public function index(RequestInterface $request)
    {
        // 从请求中获得 id 参数
        $id = $request->input('id', 1);
        return (string)$id;
    }
}
```

> 2. `#[Controller]`

```php
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;

#[Controller]
class IndexController
{
    // Hyperf 会自动为此方法生成一个 /index/index 的路由，允许通过 GET 或 POST 方式请求
    #[RequestMapping(path: "index", methods: "get,post")]
    public function index(RequestInterface $request)
    {
        // 从请求中获得 id 参数
        $id = $request->input('id', 1);
        return (string)$id;
    }
}
```









### 2. 模板语法

> 使用larvael blade语法 一般少用，用来写接口就行。

```bash
composer require hyperf/view
composer require duncan3dc/blade
composer require hyperf/task
```

然后生成一个 view 相关的配置。

```bash
php bin/hyperf.php vendor:publish hyperf/view
```

这时，会在 config/autoload 目录下多出一个 view.php 的配置文件，接着去配置它吧。

```ruby
return [
    'engine' => \Hyperf\View\Engine\BladeEngine::class,  // 使用和 Laravel 一样的 Blade 模板引擎
    'mode' => Mode::TASK, // 使用 Task 模式，还需要单独去配置 Task 相关的配置
    'config' => [
        'view_path' => BASE_PATH . '/storage/view/', // 模板文件路径，不存在自己创建下
        'cache_path' => BASE_PATH . '/runtime/view/', // 模板缓存文件路径，不存在自己创建下
    ],
];
```

其中，mode 可以选择 Mode::SYNC 和 Mode::TASK 两种模式，SYNC 是同步模式，要使用协程安全的模板引擎，所以官方更推荐使用 TASK 模式，但开启 TASK 模式又需要一些别的配置，主要就是配置一下 config/autoload/server.php 这个配置文件。

```javascript
// 在 setting 中添加
'setting' =>[
    // ...
    // Task Worker 数量，根据您的服务器配置而配置适当的数量
    'task_worker_num' => 8,
    // 因为 `Task` 主要处理无法协程化的方法，所以这里推荐设为 `false`，避免协程下出现数据混淆的情况
    'task_enable_coroutine' => false,
],
// 在 callbacks 中添加
'callbacks' => [
    // Task callbacks
    Event::ON_TASK => [Hyperf\Framework\Bootstrap\TaskCallback::class, 'onTask'],
    Event::ON_FINISH => [Hyperf\Framework\Bootstrap\FinishCallback::class, 'onFinish'],
]
```

准备工作好了之后，我们去 storage/view 目录下面新建一个 hello.blade.php 吧。

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
hello {{$name}} !
</body>
</html>
```

然后定义一个控制器方法去使用这个模板文件并传值。

```php
public function view(RenderInterface $render){
    return $render->render('hello', ['name' => 'Zyblog']);
}
```

RenderInterface 对象是通过依赖注入进来的一个模板渲染对象，直接调用它的 render() 方法就可以指定模板和传递参数了，其实和 Laravel 也很像，只是我们要做的准备工作更多一些。







### 3. 命令行脚本

> 我们再来看一下在 Hyperf 中如何定义运行一个 Command 脚本。这东西在 Laravel 中也很常用



```shell
php bin/hyperf.php gen:command TestCommand
```

```php
#[Command]
class TestCommand extends HyperfCommand
{
    public function __construct(protected ContainerInterface $container)
    {   
        // 命令
        parent::__construct('test:show');
    }

    public function configure()
    {
        parent::configure();
        // 介绍
        $this->setDescription('Hyperf Demo Command');
    }

    public function handle()
    {
        // 执行
        $this->line('Hello Hyperf!', 'info');
    }
}
```

> 执行

```shell
php bin/hyperf.php test:show
```











