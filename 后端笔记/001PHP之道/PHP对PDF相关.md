<center>PHP对PDF相关</center>





[toc]









## PHP对PDF相关

> 比较好用的pdf生成操作库
>
> [github](https://github.com/mpdf/mpdf)  [doc](https://mpdf.github.io/about-mpdf/)









### 1. 安装

> 最新版本

```shell
composer require mpdf/mpdf
```

> 使用

```php
<?php

require_once __DIR__ . '/vendor/autoload.php';

$mpdf = new \Mpdf\Mpdf();
$mpdf->WriteHTML('<h1>Hello world!</h1>');
$mpdf->Output();
```





### 2. 支持中文

```php
// 2. 配置中文字体（以宋体 SimSun 为例）
$config = [
    'mode' => 'utf-8',
    'format' => 'A4',
    'default_font' => 'simsun', // 默认使用宋体
    'autoScriptToLang' => true, // 自动检测语言
    'autoLangToFont' => true, // 自动应用字体
];

$mpdf = new \Mpdf\Mpdf($config);
// 添加中文字体（示例：宋体）
$mpdf->autoScriptToLang = true;
$mpdf->autoLangToFont = true;
$mpdf->WriteHTML($data['htmlstr']);
```







### 3. pdf文件属性

> 设置pdf属性和密码保护 [doc](https://mpdf.github.io/setting-pdf-file-properties/password-protection.html)

```shell
// pdf元数据
    $mpdf->SetTitle('Document Title');
    $mpdf->SetAuthor('Author Name');
    $mpdf->SetCreator('My Creator');
    $mpdf->SetKeywords('My Keywords, More Keywords');

    // 设置PDF密码和权限
    $permissions = array('print'); // 允许用户复制和打印
    $userPassword = 'user123'; // 用户密码，用于打开PDF
    $ownerPassword = 'owner123'; // 所有者密码，用于完全访问
    // 设置保护
    $mpdf->SetProtection(
    $permissions,
    $userPassword,
    $ownerPassword
	);
```







### 4. 完整代码

> 代码

```php
// 2. 配置中文字体（以宋体 SimSun 为例）
        $config = [
            'mode' => 'utf-8',
            'format' => 'A4',
            'default_font' => 'simsun', // 默认使用宋体
            'autoScriptToLang' => true, // 自动检测语言
            'autoLangToFont' => true, // 自动应用字体
        ];

        $mpdf = new \Mpdf\Mpdf($config);
        // 添加中文字体（示例：宋体）
        $mpdf->autoScriptToLang = true;
        $mpdf->autoLangToFont = true;
        $mpdf->WriteHTML($data['htmlstr']);

        // pdf元数据
        $mpdf->SetTitle('Document Title');
        $mpdf->SetAuthor('Author Name');
        $mpdf->SetCreator('My Creator');
        $mpdf->SetKeywords('My Keywords, More Keywords');

        // 设置PDF密码和权限
        $permissions = array('print'); // 允许用户复制和打印
//        $userPassword = 'user123'; // 用户密码，用于打开PDF
//        $ownerPassword = 'owner123'; // 所有者密码，用于完全访问
        // 设置保护
        $mpdf->SetProtection(
            $permissions,
//            $userPassword,
//            $ownerPassword
        );

        // 获取 PDF 文件流
        $pdfContent = $mpdf->Output('', \Mpdf\Output\Destination::STRING_RETURN, 'GB2312');
        //先给流数据文件名称（定一个规范- 甲-乙的基本信息使用对称加密做文件名称。） 把流数据扔给zos上传

        // 创建临时文件路径
        $tempDir = public_path();
        $filename = $tempDir . '/pdf_' . uniqid() . '.pdf';

        // 将PDF内容写入临时文件
        if (file_put_contents($filename, $pdfContent) === false) {
            throw new \Exception("无法将PDF内容写入临时文件");
        }

        // 获取文件的 MIME 类型
        $finfo = new \finfo(FILEINFO_MIME_TYPE);
        $mimeType = $finfo->file($filename);

        // 创建 UploadFile 对象
        $uploadFile = new UploadFile(
            $filename, // 文件路径
            basename($filename), // 上传文件名
            $mimeType, // MIME 类型
            UPLOAD_ERR_OK // 错误码
        );
```

