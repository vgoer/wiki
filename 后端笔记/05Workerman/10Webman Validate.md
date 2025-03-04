<center>Webman Validate</center>







[toc]







## Webman Validate

> 验证器：
>
> 基于PHP7.4 + 的Validate实现。基于ThinkPHP6修改的可用于 [webman](https://www.workerman.net/doc/webman/) 的通用validate数据验证器。
>
> 站在巨人（laravel）的肩膀上使验证器使用更加*可靠*和*便捷*
>
> 所有方法和配置与 laravel 几乎一模一样，因此使用方式完全参考 [Laravel文档](http://laravel.p2hp.com/cndocs/8.x/validation) 即可







### 1. 安装

```shell
composer require webman-tech/laravel-validation
```









### 2. 使用

````php
$request->validate([
    'title' => 'required|unique:posts|max:255',
    'author.name' => 'required',
    'author.description' => 'required',
]);

# 自定义
use Illuminate\Support\Facades\Validator;
/**
 * 存储一篇博客文章。
 *
 * @param  Request  $request
 * @return Response
 */
public function store(Request $request)
{
    $validator = Validator::make($request->all(), [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    if ($validator->fails()) {
        return redirect('post/create')
                    ->withErrors($validator)
                    ->withInput();
    }

    // 存储博客文章...
}
````

