<center>PHP对接七牛kodo</center>





[toc]







## PHP对接七牛kodo

> 对接七牛。
>
> [官网](https://portal.qiniu.com/kodo/)  [存储区域](https://developer.qiniu.com/kodo/1671/region-endpoint-fq)





### 1. 安装

> 安装依赖

```shell
composer require qiniu/php-sdk 
```







### 2. 代码

```php
# .env
# 七牛云
AK =xxxx
SK = xxxx
AK_BUCKET_NAME = xxx
AK_BUCKET_DOMAIN = xxx
AK_BUCKET_UPFILE = xxx

```

```shell
<?php

namespace app\library\Uploader;

require './vendor/autoload.php';



use Qiniu\Auth;
use Qiniu\Storage\BucketManager;
use Qiniu\Storage\UploadManager;
use Qiniu\Config;
use Qiniu\Processing\PersistentFop;

class QiniuKodo {

    /**
     * @var Auth 鉴权对象
     */
    private $auth;
    /**
     * @var string 存储空间名称
     */
    private $bucketName;
    /**
     * @var string 存储空间域名
     */
    private $bucketDomain;
    /**
     * @var BucketManager 存储空间管理对象
     */
    private $bucketManager;


    private $bucketUpfile;

    /**
     * 构造函数
     * @param string $accessKey Access Key
     * @param string $secretKey Secret Key
     * @param string $bucketName 存储空间名称
     * @param string $bucketDomain 存储空间域名
     */
    public function __construct(string $accessKey, string $secretKey, string $bucketName, string $bucketDomain, string $bucketUpfile)
    {
        // 初始化鉴权对象
        $this->auth = new Auth($accessKey, $secretKey);
        // 设置存储空间名称
        $this->bucketName = $bucketName;
        // 设置存储空间域名
        $this->bucketDomain = $bucketDomain;
        // 初始化存储空间管理对象
        $this->bucketManager = new BucketManager($this->auth);
        // 初始化上传路径
        $this->bucketUpfile = $bucketUpfile;

    }

    /**
     * 上传文件
     * @param string $key 文件在存储空间中的名称
     * @param string $localFilePath 本地文件路径
     * @param bool $overwrite 是否覆盖同名文件
     * @return array 上传结果
     */
    public function uploadFile(string $key, string $localFilePath, bool $overwrite = true): array
    {
        // 设置上传策略
        $policy = [
            'insertOnly' => $overwrite ? 0 : 1, // 是否覆盖同名文件
        ];
        // 生成上传凭证
        $token = $this->auth->uploadToken($this->bucketName, $key, 3600, $policy);
        // 初始化上传管理对象
        $uploadMgr = new UploadManager();
        // 执行上传操作
        list($ret, $err) = $uploadMgr->putFile($token, $key, $localFilePath);
        // 判断上传是否成功
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err->message()];
        }
        // 生成带签名的私有链接
        $baseUrl = "http://{$this->bucketDomain}/{$key}";

        $privateUrl = $this->auth->privateDownloadUrl($baseUrl, 3600); // 有效时间为 3600 秒

        return ['status' => 'success', 'url' => $privateUrl];
    }


    /**
     * 上传大文件
     * @param string $key 文件在存储空间中的名称
     * @param string $localFilePath 本地文件路径
     * @param int $chunkSize 分片大小，默认为 4MB
     * @return array 上传结果
     */
    public function uploadLargeFile(string $key, string $localFilePath, int $chunkSize = 4 * 1024 * 1024): array
    {
        // 生成上传凭证
        $token = $this->auth->uploadToken($this->bucketName);
        // 初始化上传管理对象
        $uploadMgr = new UploadManager();
        // 执行上传操作
        list($ret, $err) = $uploadMgr->putFile($token, $key, $localFilePath, [
            'partSize' => $chunkSize, // 设置分片大小
        ]);
        // 判断上传是否成功
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err->message()];
        }
        // 生成带签名的私有链接
        $baseUrl = "http://{$this->bucketDomain}/{$key}";

        $privateUrl = $this->auth->privateDownloadUrl($baseUrl, 3600); // 有效时间为 3600 秒

        return ['status' => 'success', 'url' => $privateUrl];
    }


    /**
     * 下载文件
     * @param string $key 文件在存储空间中的名称
     * @param string $savePath 本地保存路径
     * @param bool $private 是否为私有文件
     * @return bool 下载是否成功
     */
    public function downloadFile(string $key, string $savePath, bool $private = false): bool
    {
        // 构造文件基础 URL
        $baseUrl = "http://{$this->bucketDomain}/{$key}";
        // 如果是私有文件，生成私有下载链接
        if ($private) {
            $downloadUrl = $this->auth->privateDownloadUrl($baseUrl, 3600);
        } else {
            $downloadUrl = $baseUrl;
        }
        // 获取文件内容
        $content = file_get_contents($downloadUrl);
        // 将文件内容保存到本地
        return file_put_contents($savePath, $content) !== false;
    }


    /**
     * 删除文件
     * @param string $key 文件在存储空间中的名称
     * @return array 删除结果
     */
    public function deleteFile(string $key): array
    {
        // 执行删除操作
        $err = $this->bucketManager->delete($this->bucketName, $key);
        // 判断删除是否成功
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err];
        }
        return ['status' => 'success'];
    }


    /**
     * 重命名文件
     * @param string $oldKey 原文件名称
     * @param string $newKey 新文件名称
     * @return array 重命名结果
     */
    public function renameFile(string $oldKey, string $newKey): array
    {
        // 执行移动操作，实现重命名
        $err = $this->bucketManager->move($this->bucketName, $oldKey, $this->bucketName, $newKey);
        // 判断重命名是否成功
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err];
        }
        return ['status' => 'success'];
    }


    /**
     * 上传文件并显示进度
     * @param string $key 文件在存储空间中的名称
     * @param string $localFilePath 本地文件路径
     * @return array 上传结果
     */
    public function uploadWithProgress(string $key, string $localFilePath): array
    {
        // 生成上传凭证
        $token = $this->auth->uploadToken($this->bucketName);
        // 初始化上传管理对象
        $uploadMgr = new UploadManager();
        // 定义进度回调函数
        $progress = function ($uploadedBytes, $totalBytes) {
            printf("上传进度: %d/%d (%.1f%%)\n", $uploadedBytes, $totalBytes, $uploadedBytes/$totalBytes*100);
        };
        // 执行上传操作
        list($ret, $err) = $uploadMgr->putFile($token, $key, $localFilePath, null, null, 'application/octet-stream', $progress);
        // 判断上传是否成功
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err->message()];
        }
        // 返回上传成功的文件访问 URL
        return ['status' => 'success', 'url' => "http://{$this->bucketDomain}/{$key}"];
    }


    /**
     * 生成预签名
     * @param string $key 文件在存储空间中的名称
     * @param int $expires 有效期，默认为 3600 秒
     * @return string 预签名 token
     */
    public function generatePresignedUrl(string $key, int $expires = 3600): array {
        // 设置上传策略
        $policy = [
            'scope' => "{$this->bucketName}:{$key}", // 指定存储空间和文件名
            'deadline' => time() + $expires, // 设置有效期
            'returnBody' => json_encode([  // 自定义返回体，包含文件的完整访问路径和访问时间限制
                 'key' => '$(key)',  // 路径
                 'url' => $this->auth->privateDownloadUrl("http://{$this->bucketDomain}/{$key}", $expires),  // 路径
                 'deadline' => time() + $expires, // 有效时间
                 // 'bucket' => '$(bucket)',   // bucket name
                 // 'mimeType' => '$(mimeType)',  // 获取文件类型
                 // 'ext'      => '$(ext)',       // 自定义提取后缀参数
                 'fsize' => '$(fsize)',        // 文件大小
                 'hash' => '$(etag)',         //  hash

            ]),
            'insertOnly' => 1, // 不允许覆盖文件
        ];

        // 返回预签名 URL
        $upToken = $this->auth->uploadToken($this->bucketName, $key, $expires, $policy);

        $uploadUrl = "http://{$this->bucketUpfile}";

        return [
            "upToken" => $upToken,
            "uploadUrl" => $uploadUrl,
            'key' => $key
        ];
    }


    /**
     * 设置文件生命周期
     * @param string $key 文件在存储空间中的名称
     * @param int $days 生命周期，单位为天
     * @return array 设置结果
     */
    public function setLifecycle(string $key, int $days): array
    {
        // 初始化持久化操作管理对象
        $pfop = new PersistentFop($this->auth, new Config());
        // 设置操作指令
        $ops = "deleteAfterDays/{$days}";
        // 执行持久化操作
        list($persistentId, $err) = $pfop->execute($this->bucketName, $key, $ops);
        if ($err !== null) {
            return ['status' => 'error', 'message' => $err->message()];
        }
        return ['status' => 'success', 'persistentId' => $persistentId];
    }

    /**
     * 获取有时间限制的路径
     * @param string $key 路径
     * @param int $expires 过期时间
     * @return array 路径
     */
    public function getLifeFile(string $key, int $expires = 3600) : array
    {
        // 生成带签名的私有链接
        $baseUrl = "http://{$this->bucketDomain}/{$key}";

        $privateUrl = $this->auth->privateDownloadUrl($baseUrl, 3600); // 有效时间为 3600 秒

        return ['status' => 'success', 'url' => $privateUrl];
    }


}
```

```shell
# service
<?php

namespace app\service\v1\common;


use app\enum\CommonEnum;
use app\library\Singleton\Singleton;
use app\library\Uploader\QiniuKodo;
use support\exception\BusinessException;
use support\Log;


class KodoService
{

    use Singleton;

    private $AK;

    private $SK;

    private $AK_BUCKET_NAME;

    private $AK_BUCKET_DOMAIN;

    private $AK_BUCKET_UPFILE;

    public function __construct()
    {
        $this->AK = getenv('AK');
        $this->SK = getenv('SK');
        $this->AK_BUCKET_NAME = getenv('AK_BUCKET_NAME');
        $this->AK_BUCKET_DOMAIN = getenv('AK_BUCKET_DOMAIN');
        $this->AK_BUCKET_UPFILE = getenv('AK_BUCKET_UPFILE');
    }

    public function ValiFile($file) : array
    {
        if (!$file) {
            throw new BusinessException('未上传文件', CommonEnum::RETURN_CODE_FAIL);
        }
        // 检查文件是否有效
        if (!$file->isValid()) {
            throw new BusinessException('文件上传无效', CommonEnum::RETURN_CODE_FAIL);
        }

        // 获取文件扩展名
        $originExt = $file->getUploadExtension();

        // 获取文件内容
        $getFile = $file->getRealPath();

        # 文件相关属性
        $fileContent = file_get_contents($getFile);
        $upName = $file->getUploadName();
        $upSize = $file->getSize();
        $upHash = hash_file('sha256', $getFile);

        // 生成目录
        $time = time();
        $y = date('Y', $time);
        $m = date('m', $time);
        $d = date('d', $time);
        $path = 'case/' . $y . '/' . $m . '/' . $d . '/';
        // 生成文件名
        $mTime = mTime();
        // 路径
        $filePath = $path . $mTime . '.' . $originExt;

        return ['getFile' => $getFile, 'filePath' => $filePath];
    }

    public function initKodo()
    {
        $kodo = new QiniuKodo($this->AK, $this->SK, $this->AK_BUCKET_NAME, $this->AK_BUCKET_DOMAIN, $this->AK_BUCKET_UPFILE);

        return $kodo;
    }

    /**
     * 上传小文件
     * @param $file
     * @return array|\Response|string[]|\support\Response
     */
    public function uploadSmallFileService($file)
    {
        $file = $this->valiFile($file);

        try {

            $kodo = $this->initKodo();

            $upRes = $kodo->uploadFile($file['filePath'],$file['getFile']);

            return $upRes;

        }catch (\Exception $e){
            Log::error("上传失败:" . $e->getMessage());
            throw new BusinessException('上传失败', CommonEnum::RETURN_CODE_FAIL);
        }

    }


    /**
     * 上传大文件
     * @param $file
     * @return array
     */
    public function uploadLargeFileService($file)
    {

        $file = $this->valiFile($file);

        try {

            $kodo = $this->initKodo();

            $upRes = $kodo->uploadLargeFile($file['filePath'],$file['getFile']);

            return $upRes;

        }catch (\Exception $e){
            Log::error("上传失败:" . $e->getMessage());
            throw new BusinessException('上传失败', CommonEnum::RETURN_CODE_FAIL);
        }

    }

    /**
     * 获取欲上传信息
     * @param $filename 文件名称
     * @return array|string
     */
    public function generatePresignedUrl($filename) : array
    {

        // 生成目录
        $time = time();
        $y = date('Y', $time);
        $m = date('m', $time);
        $d = date('d', $time);
        $path = 'case/' . $y . '/' . $m . '/' . $d . '/';
        // 生成文件名
        $mTime = mTime();
        // 路径
        $filePath = $path . $mTime . '.' . $filename;

        try {

            $kodo = $this->initKodo();

            $getInfo = $kodo->generatePresignedUrl($filePath);

            return $getInfo;

        }catch (\Exception $e){
            Log::error("获取token失败:" . $e->getMessage());
            throw new BusinessException('获取token失败:', CommonEnum::RETURN_CODE_FAIL);
        }
    }


    /**
     * 获取有效连接
     * @param string $key
     * @return string 获取路径
     */
    public function getLifeFile(string $key) : array
    {
        try {

            $kodo = $this->initKodo();

            $getInfo = $kodo->getLifeFile($key);

            if ($getInfo['status'] != 'success') {
                throw new BusinessException("获取有效连接失败:", CommonEnum::RETURN_CODE_FAIL);
            }

            return $getInfo['url'];

        }catch (\Exception $e){
            Log::error("获取有效连接失败:" . $e->getMessage());
            throw new BusinessException('获取有效连接失败:', CommonEnum::RETURN_CODE_FAIL);
        }
    }



}
```

```shell
# controller
<?php

namespace app\controller\v1\common;

use app\service\v1\common\KodoService;
use support\Request;
use support\Response;

class KodoUpfile
{

    public function uploadSmallFile(Request $request) : Response
    {
        $file = $request->file('file');

        $kodo = new KodoService();
        $upRes = $kodo->uploadSmallFileService($file);

        if ($upRes['status'] != 'success') {
            return fail("上传失败");
        }

        return success(['url' => $upRes['url']],200, "文件上传成功");
    }


    public function uploadLargeFile(Request $request) : Response
    {
        $file = $request->file('file');

        $kodo = new KodoService();
        $upRes = $kodo->uploadSmallFileService($file);

        if ($upRes['status'] != 'success') {
            return fail("上传失败");
        }

        return success(['url' => $upRes['url']],200, "文件上传成功");
    }


    public function generatePresignedUrl(Request $request) : Response
    {
        $filename = $request->input('filename');

        $kodo = new KodoService();

        $upInfo = $kodo->generatePresignedUrl($filename);

        return success(
            ["upToken" => $upInfo['upToken'], "uploadUrl" => $upInfo['uploadUrl'], 'key' => $upInfo['key']], 200, "获取成功"
        );
    }
}
```





> 又很多策略

```php
use Qiniu\Auth;

class QiniuUploader {
    private $auth;
    private $bucketName;

    public function __construct($accessKey, $secretKey, $bucketName) {
        $this->auth = new Auth($accessKey, $secretKey);
        $this->bucketName = $bucketName;
    }

    public function generatePresignedUrl(string $key, int $expires = 3600): array {
        // 设置上传策略
        $policy = [
            'scope' => "{$this->bucketName}:{$key}", // 指定存储空间和文件名
            'deadline' => time() + $expires, // 设置有效期
            'returnBody' => '{"key":"$(key)","hash":"$(etag)","bucket":"$(bucket)","fsize":$(fsize)}', // 自定义返回体
            'insertOnly' => 1, // 不允许覆盖文件
            'fsizeLimit' => 1048576, // 限制文件大小为 1MB
            'persistentOps' => 'imageView2/1/w/200/h/200', // 持久化处理
            'persistentNotifyUrl' => 'http://your-domain.com/notify', // 持久化通知 URL
            'callbackUrl' => 'http://your-domain.com/callback', // 回调 URL
            'callbackBody' => '{"key":"$(key)","hash":"$(etag)","bucket":"$(bucket)","fsize":$(fsize)}', // 回调 Body
            'callbackBodyType' => 'application/json', // 回调 Body 类型
            'vars' => [
                'x:customVar' => 'customValue' // 自定义变量
            ]
        ];

        // 返回预签名 URL
        $upToken = $this->auth->uploadToken($this->bucketName, $key, $expires, $policy);

        $uploadUrl = "http://up-z2.qiniup.com"; // 七牛云上传地址（华南区域）

        return [
            "upToken" => $upToken,
            "uploadUrl" => $uploadUrl,
            'key' => $key
        ];
    }
}
```



> 前端： 生成预签名 

```js
    <input type="file" name="file" id="">

    <script>
        async function uploadFile(file) {
//   // 1. 获取预签名 URL 和 key（假设从后端接口获取）
//     const { token, key, uploadUrl } = await fetch('/api/qiniu-token', {
//         method: 'POST',
//         body: JSON.stringify({ filename: file.name }),
//         headers: { 'Content-Type': 'application/json' }
//     }).then(res => res.json());

    // 2. 构造上传表单数据
    const formData = new FormData();
            
    formData.append('token', 'token');
    formData.append('key', 'case/2025/04/28/1745822760960.a.jpg');     // 指定存储路径
    formData.append('file', file);   // 文件对象

    // 3. 发起上传请求到七牛云
    try {
        const response = await fetch('http://up-z2.qiniup.com', {
            method: 'POST',
            body: formData,
        });
        const result = await response.json();
        console.log('上传成功:', result);
        return `http://svcs4nvvt.hn-bkt.clouddn.com/a.jpg`;
    } catch (error) {
        console.error('上传失败:', error);
    }
    }

    // 使用示例
    const fileInput = document.querySelector('input[type="file"]');
    fileInput.addEventListener('change', async (e) => {
    const file = e.target.files[0];
    const fileUrl = await uploadFile(file);
    // 显示上传后的文件链接...
    });
    </script>
```



