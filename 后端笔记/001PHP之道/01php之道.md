---
title: 01php之道
description: 
published: 1
date: 2023-08-30T10:37:17.131Z
tags: 
editor: markdown
dateCreated: 2023-08-30T10:37:15.571Z
---

<center>php之道</center>







[toc]





## php 之道

> 如果你刚开始学习 PHP，请使用最新的稳定版本 [PHP 8.0](http://php.net/downloads.php)。
>
> [doc](https://learnku.com/docs/php-the-right-way/PHP8.0/welcome/11458)
>
> 快速安装PHP+Composer: [doc](https://www.workerman.net/download)
>
> 携程：[doc](https://www.workerman.net/u/chaz6chez)







### 1. 内置web服务器

> PHP 5.4 之后，你可以不用安装和配置功能齐全的 Web 服务器，就可以开始学习 PHP。

```php
php -S localhost:8000
```
> goer curd







### 2. jit

> jit编译器： IT通过在运行时将PHP代码编译为机器码，绕过了传统的解释执行过程，从而显著提高了计算密集型任务的执行速度。

```php
// 启用JIT的配置（php.ini）
opcache.enable=1
opcache.jit_buffer_size=100M
opcache.jit=tracing
```

JIT特别适合以下场景：

- 数学计算和算法密集型操作
- 大型数据处理
- 图像处理和操作
- 加密/解密操作





### 3. 命名参数

> 命名参数允许开发者通过参数名称而非位置来传递参数，大大提高了代码的可读性和灵活性。

```php
function createUser(string $name, string $email, int $age = 18, bool $isAdmin = false) {
    // 用户创建逻辑
}

// PHP 8.0+ 命名参数调用
createUser(
    name: '张三',
    email: 'zhangsan@example.com',
    isAdmin: true
);
```





### 4. 联合类型和交集类型

> PHP 8.0引入了联合类型，允许一个参数或返回值声明多种可能的类型。PHP 8.1则进一步增加了交集类型，要求值必须满足所有指定类型。

```php
// PHP 8.0 联合类型
function formatInput(int|string $input): string {
    return (string)$input;
}

// PHP 8.1 交集类型
function generateSlug(HasTitle&HasId $post) {
    return strtolower($post->getTitle()) . $post->getId();
}
```





### 5. match表达式

> Match表达式是switch语句的更强大替代品，它提供严格的比较、返回值能力，并且没有穿透(break)问题。

```php
$statusCode = 404;

// PHP 8.0+ match表达式
$message = match ($statusCode) {
    200 => 'OK',
    301, 302 => 'Redirect',
    404 => 'Not Found',
    500 => 'Server Error',
    default => 'Unknown Status Code'
};

echo $message; // 输出"Not Found"
```





### 6. 构造器属性升级

> 这个特性减少了类定义中的样板代码，允许直接在构造参数中声明和初始化属性。
>
> 直接在参数前添加可见性修饰符（public/protected/private）
>
> 自动将参数赋值给同名属性

```php
// PHP 8.0+ 构造器属性提升
class User
{
    public function __construct
    (
        public string $name,
        protected int $age,
        private bool $isAdmin = false
    )
    {
    }
}

// 等效于传统写法：
class User1
{
    public string $name;
    protected int $age;
    private bool $isAdmin = false;

    public function __construct(string $name, int $age, bool $isAdmin)
    {
        $this->name = $name;
        $this->age = $age;
        $this->isAdmin = $isAdmin;
    }
}
```







### 7. 枚举

> 枚举是一种特殊的类，表示一组固定的命名值，为PHP带来了类型安全的枚举支持。

```php
// PHP 8.1+ 枚举
enum Status: string {
    case Pending = 'pending';
    case Active = 'active';
    case Archived = 'archived';
}

class Post {
    public function __construct(
        public Status $status = Status::Pending
    ) {}
}

$post = new Post();
$post->status = Status::Active;

```

```php
enum Week: string {
    case Monday = 'monday';
    case Tuesday = 'tuesday';
}

//获取枚举类型元素的值
echo Week::Monday->value;

//获取枚举类型元素的key
echo Week::Monday->name;
```

