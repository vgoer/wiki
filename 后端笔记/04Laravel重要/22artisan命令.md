<center>Artisan命令</center>





[toc]





## Artisan命令

> `artisan` 提供了一系列的命令行工具，用于快速进行开发和管理。





### 1. 常用

```shell
php artisan list：列出所有可用的 artisan 命令列表。
php artisan help <command>：显示指定命令的帮助信息。
php artisan serve：启动内置的开发服务器。
php artisan migrate：执行数据库迁移。
php artisan db:seed：填充数据库种子数据。
php artisan make:model <name>：创建一个新的模型类。
php artisan make:controller <name>：创建一个新的控制器类。
php artisan make:migration <name>：生成一个新的数据库迁移文件。
php artisan make:seeder <name>：生成一个新的数据库填充器。
php artisan route:list：显示应用程序定义的所有路由。
php artisan cache:clear：清除缓存。
php artisan optimize：优化框架性能。
```





### 2. 自定义

> 你也可以创建自己的自定义 Artisan 命令来满足特定的需求。

1. 创建

```php
php artisan make:command CustomCommand
```

2. 编辑文件

> 这将在 `app/Console/Commands` 目录下创建一个新的 `CustomCommand` 类文件。

```php
# 定义命令
protected $signature = 'custom:run';
# 定义描述
protected $description = 'Command description';
public function handle()
{   
 		// 写业务逻辑
        mkdir('aaa');
}
```

3. 注册

> 在 `app/Console/Kernel.php` 文件的 `commands` 属性中注册你的自定义命令。在 `commands` 数组中添加你的命令类的全局命名空间。

```php
protected $commands = [
    \App\Console\Commands\CustomCommand::class,
];
```

4. 执行

```php
php artisan custom:run
```





### 3. 示例

> 自定义命令 数据迁移和填充   定时任务    后台处理任务    资源生成

```php
# 1. 用于生成随机用户数据的自定义命令
class GenerateRandomUsers extends Command
{
    protected $signature = 'users:generate {count}';

    protected $description = 'Generate random users';

    public function handle()
    {
        $count = $this->argument('count');

        for ($i = 0; $i < $count; $i++) {
            User::create([
                'name' => 'User ' . ($i + 1),
                'email' => 'user' . ($i + 1) . '@example.com',
                'password' => Hash::make('password'),
            ]);
        }

        $this->info($count . ' random users generated.');
    }
}
```

```php
# 创建一个定时任务脚本的自定义命令，每天执行一次
class DailyTask extends Command
{
    protected $signature = 'task:daily';

    protected $description = 'Perform daily tasks';

    public function handle()
    {
        // 执行你的日常任务逻辑
        $this->info('Daily tasks completed.');
    }
}
```

```php
# 创建一个将邮件队列发送到后台处理的自定义命令
class ProcessEmailQueue extends Command
{
    protected $signature = 'queue:process-emails';

    protected $description = 'Process email queue in the background';

    public function handle()
    {
        while (true) {
            $email = EmailQueue::next();

            if ($email) {
                Mail::send($email);
                $this->info('Email sent to ' . $email->recipient);
            } else {
                break;
            }
        }

        $this->info('Email queue processed.');
    }
}
```

```php
# 创建一个用于自动生成 Markdown 文档的自定义命令：
class GenerateMarkdownDocs extends Command
{
    protected $signature = 'docs:generate-markdown';

    protected $description = 'Generate Markdown documents';

    public function handle()
    {
        $files = File::allFiles(base_path('resources/docs'));

        foreach ($files as $file) {
            $contents = File::get($file);
            // 将文件内容转换为 Markdown 格式
            // ...
			//  File 类的 put() 方法将文件写入到指定位置
            File::put($file.'.md', $markdownContents);
        }

        $this->info('Markdown documents generated.');
    }
}
```

> 我去，666