<center>Http客户端</center>





[toc]







## Http客户端

> 让你可用快速地使用 HTTP 请求与其他 Web 应用进行通信。



### 1. 原生的

```php
# 自己写的原生的发送post请求的
 	/**
     * 初始化 发送data
     *
     * @param String $url 发送的url
     * @param Array $data  发送的data 
     * @return Object 
     */
public function initCurl($url,$data)
{
    $options = array(
        'http' => array(
            'method'  => 'POST',
            'header'  => 'Content-type:text/plain',
            'content' => $data
        )
    );
    $context = stream_context_create($options);
    $resopse = file_get_contents($url,false,$context);

    if($resopse === false) return false;

    return json_decode($resopse);
}
```





### 2. laravel

> http客户端



#### a. 安装

```shell
composer require guzzlehttp/guzzle
```



#### b. 创建

```php
use Illuminate\Support\Facades\Http;

$response = Http::get('http://example.com');

if ($response->successful()) {
    // 请求成功，获取响应数据
    $data = $response->json();
} else {
    // 请求失败，获取错误信息
    $errorMessage = $response->body();
}
```

#### c.post

```php

$response = Http::post('http://example.com/users', [
    'name' => 'Steve',
    'role' => 'Network Administrator',
]);

# 1. 发送 URL 编码请求
$response = Http::asForm()->post('http://example.com/users', [
    'name' => 'Sara',
    'role' => 'Privacy Consultant',
]);


# 2. 发送原始数据（Raw）请求#
$response = Http::withBody(
    base64_encode($photo), 'image/jpeg'
)->post('http://example.com/photo');

# 3. 请求头
$response = Http::withHeaders([
    'X-First' => 'foo',
    'X-Second' => 'bar'
])->post('http://example.com/users', [
    'name' => 'Taylor',
]);

# 4. Multi-Part 请求
$photo = fopen('photo.jpg', 'r');

$response = Http::attach(
    'attachment', $photo, 'photo.jpg'
)->post('http://example.com/attachments');

# 5. Bearer 令牌
$response = Http::withToken('token')->post(/* ... */);

# 6. 重试
$response = Http::retry(3, 100)->post(/* ... */);
```

