<center>yakpro-po</center>





[toc]









## 混淆加密

> ### 背景
>
> 防止商用软件实现逻辑泄露。 [blog](https://learnku.com/articles/65068)
>
> ### 工具介绍
>
> [yakpro-po](https://github.com/pk-fr/yakpro-po) 由原生 PHP 开发，所以安装的时候，保证环境能执行 PHP 命令。







### 1. 安装

> 1. 这里按项目推荐目录为准:/user/local

```shell
cd /usr/local
```

> 2. 通过 git 拉取 yakpro-po 代码

```php
git clone https://github.com/pk-fr/yakpro-po.git
```

> 3. 到 yakpro-po 目录，还需要安装 PHP-Parser

```php
cd yakpro-po && git clone https://github.com/nikic/PHP-Parser.git
```

> 4. 保证 yakpro-po 有可执行权限

```php
chmod a+x yakpro-po.php
```

> 5. 为方便使用，建立软连接到 bin 目录

```php
cd /usr/local/bin
ln -s /usr/local/yakpro-po/yakpro-po.php yakpro-po
```

> 6. 安装完毕，查看下是否正常 `yakpro-po –help`

```php
yakpro-po –help
Info:   Using [/usr/local/yakpro-po/yakpro-po.cnf] Config File...
Info:   yakpro-po version = 2.0.13
```



### 2. 使用

> 提供了多种方式混淆代码，因为本次是针对 Laravel 项目代码混淆，所以采用

```php
yakpro-po source_filename -o target_filename
```









### 3. 实践

> Laravel 项目直接上面的命令，不带任何选项，混淆后的代码根本跑不起来，因为默认混淆了类名、命令空间、变量等等，所以要手动添加选项来指定混淆选项。

```php
# 建议使用的选选项
yakpro-po my_app -o obfuscate_my_app --no-obfuscate-function-name --no-obfuscate-class_constant-name --no-obfuscate-class-name --no-obfuscate-interface-name --no-obfuscate-trait-name --no-obfuscate-property-name --no-obfuscate-method-name --no-obfuscate-namespace-name --no-obfuscate-label-name
```

```php
# 混淆了
淆变量名
混淆常量名
混淆 if 语句
淆循环语句
淆字符串文字
随机播放语句
单行输出
```

### 需要注意的点：不要混淆 模板代码

- 先把模板目录拷贝出来，混淆之后，再还原回去。



>  其它 PHP 框架混淆
> 理论上 yakpro 适用所有 PHP 项目，多数项目是在成熟的 PHP 框架下开发，不同框架，有不同的约束，比如 Laravel 会有一些约定的配置常量，就不能混淆常量，不然代码不能正常运行。
> 所以当混淆后，代码跑不起来时，就需要根据实际情况，调整混淆选项了。

附录：可用混淆选项

```php
--no-strip-indentation 多行输出
--strip-indentation 单行输出

--no-shuffle-statements 不打乱语句
--shuffle-statements 随机播放语句

--no-obfuscate-string-literal 不混淆字符串文字
--obfuscate-string-literal 混淆字符串文字

--no-obfuscate-loop-statement 不混淆循环语句
--obfuscate-loop-statement 混淆循环语句

--no-obfuscate-if-statement 不混淆 if 语句
--obfuscate-if-statement 混淆 if 语句

--no-obfuscate-constant-name 不混淆常量名
--obfuscate-constant-name 混淆常量名

--no-obfuscate-variable-name 不混淆变量名
--obfuscate-variable-name 混淆变量名

--no-obfuscate-function-name 不混淆函数名
--obfuscate-function-name 混淆函数名

--no-obfuscate-class_constant-name 不混淆类常量名
--obfuscate-class_constant-name 混淆类常量名

--no-obfuscate-class-name 不混淆类名
--obfuscate-class-name 混淆类名

--no-obfuscate-interface-name 不混淆接口名称
--obfuscate-interface-name 混淆接口名称

--no-obfuscate-trait-name 不混淆特征名称
--obfuscate-trait-name 混淆特征名称

--no-obfuscate-property-name 不混淆属性名称
--obfuscate-property-name 混淆属性名称

--no-obfuscate-method-name 不混淆方法名
--obfuscate-method-name 混淆方法名称

--no-obfuscate-namespace-name 不混淆命名空间名称
--obfuscate-namespace-name 混淆命名空间名称

--no-obfuscate-label-name 不混淆标签名称
--obfuscate-label-name 混淆标签名称
```





