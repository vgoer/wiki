<center>elasticsearch</center>





[toc]







## elasticsearch

> Elasticsearch 是一种分布式 RESTful 搜索引擎，针对生产规模工作负载的速度和相关性进行了优化。
>
> [elasticsearch](https://github.com/elastic/elasticsearch) 下载：[elastic](https://www.elastic.co/cn/downloads/elasticsearch)







### 1. 启动

> windows： `/bin/elasticsearch.bat`











### 2. php中使用

> 安装

```shell
composer require elasticsearch/elasticsearch
```

> 1. 创建索引 `index.php`

```shell
<?php
require "vendor/autoload.php";

use Elastic\Elasticsearch\ClientBuilder;


// 创建索引
$hosts = [
    "127.0.0.1:9200",

];


$client = ClientBuilder::create()->setHosts($hosts)->build();


$prams = [
    'index'   => "my_index",
    "body"    => [
        "mapings"  => [
            "properties" => [
                "first_name"  => [
                    "type" => "text",
                    // 使用es插件
                    "analyzer"  => "ik_max_word"
                ],
                "age"  => [
                    "type"  => "integer"
                ]
            ]
        ]
    ]
];

$response = $client->index($prams);

print_r($response);
```

> 2. 插入数据： `insert.php`

```php
<?php
require "vendor/autoload.php";

use Elastic\Elasticsearch\ClientBuilder;


# 插入索引
$client = ClientBuilder::create()->build();


$params = [
    "index"  => "my_index",
    "id"     => 1,
    "body"   => [
        "first_name"  => "我的名字叫小白",
        "age"         => 20,
    ]
];


$response = $client->index($params);

print_r($response);
```

> 3. 查询数据： `search.php`

```php
<?php
require "vendor/autoload.php";

use Elastic\Elasticsearch\ClientBuilder;


$client = ClientBuilder::create()->build();

// 搜索索引

$params = [
    "index"  => "my_index",
    "_source" => ["first_name", "age"], // 指定字段
    "body"   => [
        "query" => [
            "match" => [
                "first_name"  => "小白"
            ]
        ]
    ]
];


$response = $client->search($params);
print_r($response);
```

