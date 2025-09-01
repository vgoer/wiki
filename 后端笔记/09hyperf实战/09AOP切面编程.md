<center>AOP切面编程</center>





[toc]









## AOP切面编程

> AOP 为 `Aspect Oriented Programming` 的缩写，意为：`面向切面编程`，通过动态代理等技术实现程序功能的统一维护的一种技术。AOP 是 OOP 的延续，也是 Hyperf 中的一个重要内容，是函数式编程的一种衍生范型。利用 AOP 可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
>
> 用通俗的话来讲，就是在 Hyperf 里可以通过 `切面(Aspect)` 介入到任意类的任意方法的执行流程中去，从而改变或加强原方法的功能，这就是 AOP。
>
> > 注意这里所指的任意类并不是完全意义上的所有类，在 Hyperf 启动初期用于实现 AOP 功能的类自身不能被切入。





## 1. 定义切面(Aspect)

> 每个 `切面(Aspect)` 必须实现 `Hyperf\Di\Aop\AroundInterface` 接口，并提供 `public` 的 `$classes` 和 `$annotations` 属性，为了方便使用，我们可以通过继承 `Hyperf\Di\Aop\AbstractAspect` 来简化定义过程，我们通过代码来描述一下。

```php
<?php
namespace App\Aspect;

use App\Service\SomeClass;
use App\Annotation\SomeAnnotation;
use App\Controller\UserController;
use Hyperf\Di\Annotation\Aspect;
use Hyperf\Di\Aop\AbstractAspect;
use Hyperf\Di\Aop\ProceedingJoinPoint;

#[Aspect]
class FooAspect extends AbstractAspect
{
    // 要切入的类或 Trait，可以多个，亦可通过 :: 标识到具体的某个方法，通过 * 可以模糊匹配
    public array $classes = [

        // 直接切片类 所有类都会执行这个切片
        // UserController::class,

        // 直接切片方法 只有类的这个方法会执行
        'App\Controller\UserController::getId',
        'App\Service\SomeClass::someMethod',
        'App\Service\SomeClass::*Method',
    ];

    // 要切入的注解，具体切入的还是使用了这些注解的类，仅可切入类注解和类方法注解
    public array $annotations = [

    ];

    public function process(ProceedingJoinPoint $proceedingJoinPoint)
    {
        // 切面切入后，执行对应的方法会由此来负责
        // $proceedingJoinPoint 为连接点，通过该类的 process() 方法调用原方法并获得结果
        // 在调用前进行某些处理
        var_dump("aspect_111");
        $result = $proceedingJoinPoint->process();
        var_dump("aspect_2222");
        // 在调用后进行某些处理
        return $result;
    }
}

```

> 控制器

```php
<?php

declare(strict_types=1);

namespace App\Controller;

use App\Exception\FooException;
use Hyperf\HttpMessage\Cookie\Cookie;
use Hyperf\HttpServer\Annotation\AutoController;
use Hyperf\HttpServer\Annotation\Controller;
use Hyperf\HttpServer\Annotation\RequestMapping;
use Hyperf\HttpServer\Contract\RequestInterface;
use Hyperf\HttpServer\Contract\ResponseInterface;
use Psr\Http\Message\ResponseInterface as Psr7ResponseInterface;

#[AutoController]
class UserController extends AbstractController
{
    public function getId(RequestInterface $request, ResponseInterface $response) : Psr7ResponseInterface
    {
        
        var_dump("fun 111");
        return $response->json(["code" => 200, "message" =>  "yes"]);
    }

    public function getA(RequestInterface $request, ResponseInterface $response) : Psr7ResponseInterface
    {
        
        var_dump("fun 222");
        return $response->json(["code" => 200, "message" =>  "getA"]);
    }
}

```

