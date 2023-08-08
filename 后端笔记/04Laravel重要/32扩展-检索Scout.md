<center>Scout</center>





[toc]





## Scout

> Scout 实现全文检索. [blog ](https://blog.csdn.net/hedeqiang9436/article/details/106498678)[doc](https://learnku.com/docs/laravel/10.x/scout/14915#848bb1)
>
> `Laravel Scout` 为 [Eloquent 模型](https://laravel-china.org/docs/laravel/5.7/eloquent/2294) 的全文搜索提供了基于驱动的简单的解决方案。







### 1. 准备

> 注册free版本 [algolia](https://www.algolia.com/)

```php
# 安装和生成配置
composer require laravel/scout
php artisan vendor:publish --provider="Laravel\Scout\ScoutServiceProvider"

# 安装algolia
composer require algolia/algoliasearch-client-php
```

> 其他步骤可以按照blog中来