<center>php格式化</center>



[toc]







## php格式化

> PHP 编码标准修复器 (PHP CS Fixer) 工具可以修复您的代码以符合标准；无论您是想遵循 PSR-1、PSR-2 等定义的 PHP 编码标准，还是像 Symfony 这样的社区驱动的标准。您**还**可以通过配置定义您（团队）的编码风格。
>
> [github](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer)
>
> [blog](https://cs.symfony.com/) 







### 1. 安装使用

```shell
# 在项目中安装
composer require friendsofphp/php-cs-fixer

# 使用
vendor/bin/php-cs-fixer fix src
```

> 假设您按照上述说明安装了 PHP CS Fixer，则可以运行以下命令来修复目录中的 PHP 文件`src`：

> 更多配置在文档。