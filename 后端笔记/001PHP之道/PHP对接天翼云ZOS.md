<center>PHP对接天翼云ZOS</center>



[toc]







## PHP对接天翼云ZOS

> PHP对接天翼云对象存储。
>
> [zos](https://www.ctyun.cn/document/10026735)
>
> [sdk](https://www.ctyun.cn/document/10026735/10110276)







### 1. 前期准备

1. 创建密钥和bucket

![image-20250321173626445](.\assets\image-20250321173626445.png)

2. 获取终端节点

![image-20250321173849276](.\assets\image-20250321173849276.png)







### 2. 配置和使用

> `.env`

```php
# 天翼云zos
CTYUN_ACCESS_KEY_ID = 
CTYUN_SECRET_KEY_SECRET = 
CTYUN_ENDPOINT = 
CTYUN_BASE_URL = 

```

> `ZosUpload`

```php
<?php
namespace utils;

require './vendor/zos/aws-autoloader.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;
use think\Exception;

class ZosUploader
{
    private $s3Client;

    public function __construct($accessKey, $secretKey, $endpoint)
    {
        $this->s3Client = new S3Client([
            'credentials' => [
                'key' => $accessKey,
                'secret' => $secretKey
            ],
            'region' => 'us-east-1',
            'endpoint' => $endpoint,
            'http'  => [
                'verify' => false,
            ]
        ]);
    }

    /** 上传文件
     * @param $bucket 桶名称
     * @param $db_name 数据库类型
     * @param $file 文件
     * @param $acl 权限 默认是private
     * @return array|\Aws\Result
     */
    public function upload($db_name, $file, $bucket = "xdlyxs",$acl="public-read")
    {

        // 获取文件扩展名
        $origin_ext = $file->getUploadExtension();

        // 获取文件内容
        $get_file = $file->getRealPath();
        $file_content = file_get_contents($get_file);

        // 生成目录
        $time = time();
        $y = date('Y', $time);
        $m = date('m', $time);
        $d = date('d', $time);

        $path = $db_name . '/' . $y . '/' . $m . '/' . $d . '/';

        // 生成文件名
        $mTime = mTime();
        $file_name = $mTime . '.' . $origin_ext;
        // 路径
        $file_path = $path . $file_name;

        try {
            $result = $this->s3Client->putObject([
                'Bucket' => $bucket,
                'Key' => $file_path,
                'Body' => $file_content,
                "ACL" => $acl
            ]);

            $result['success'] = true;

            // 组装返回数据
            $result['date'] = [
                'file_name' => $file->getUploadName(),
                'file_path' => $file_path,
                'file_size' => $file->getSize(),
                'file_type' => $origin_ext,
                'file_hash' => hash_file('sha256', $get_file),
            ];
            $result['message'] = "File uploaded successfully.";
        } catch (S3Exception $e) {
            $result['message'] = "S3 error: " . $e->getMessage();
        } catch (AwsException $e) {
            $result['message'] = "AWS error: " . $e->getMessage();
            $result['data'] = [
                'aws_request_id' => $e->getAwsRequestId(),
                'aws_error_type' => $e->getAwsErrorType(),
                'aws_error_code' => $e->getAwsErrorCode(),
                'error_details' => $e->toArray()
            ];
        } catch (\Exception $e) {
            $result['message'] = "General error: " . $e->getMessage();
        }
        return $result;
    }

    /**
     * 文件下载
     * @param $bucket 桶
     * @param $path 路径
     * @return array|\Aws\Result
     */
    public function download($path, $bucket = "xdlyxs")
    {
        try {
            $result = $this->s3Client->getObject([
                'Bucket' => $bucket,
                'Key' => $path ,
            ]);
            // 获取文件流
            $stream = $result['Body'];

            // 设置文件名（可以根据需要动态生成）
            $filename = basename($path);

            // 设置响应头，提示浏览器下载文件
            header('Content-Description: File Transfer');
            header('Content-Type: application/octet-stream');
            header('Content-Disposition: attachment; filename="' . $filename . '"');
            header('Expires: 0');
            header('Cache-Control: must-revalidate');
            header('Pragma: public');
            header('Content-Length: ' . $stream->getSize());

            // 将流内容输出到浏览器
            while (!$stream->eof()) {
                echo $stream->read(1024); // 每次读取 1024 字节
            }

            // 中止脚本，避免输出其他内容
            exit;

        } catch (S3Exception $e) {
            $result['message'] = "S3 error: " . $e->getMessage();
        } catch (AwsException $e) {
            $result['message'] = "AWS error: " . $e->getMessage();
            $result['data'] = [
                'aws_request_id' => $e->getAwsRequestId(),
                'aws_error_type' => $e->getAwsErrorType(),
                'aws_error_code' => $e->getAwsErrorCode(),
                'error_details' => $e->toArray()
            ];
        } catch (\Exception $e) {
            $result['message'] = "General error: " . $e->getMessage();
        }
        return $result;
    }

    /**
     * 删除文件
     * @param $path 删除文件
     * @param $bucket 桶
     * @return array|\Aws\Result
     */
    public function delete($path, $bucket="xdlyxs")
    {
        try {
            $result = $this->s3Client->DeleteObject([
                'Bucket' => $bucket,
                'Key' => $path ,
            ]);

            $result['success'] = true;
            $result['message'] = "File delete successfully.";

        } catch (S3Exception $e) {
            $result['message'] = "S3 error: " . $e->getMessage();
        } catch (AwsException $e) {
            $result['message'] = "AWS error: " . $e->getMessage();
            $result['data'] = [
                'aws_request_id' => $e->getAwsRequestId(),
                'aws_error_type' => $e->getAwsErrorType(),
                'aws_error_code' => $e->getAwsErrorCode(),
                'error_details' => $e->toArray()
            ];
        } catch (\Exception $e) {
            $result['message'] = "General error: " . $e->getMessage();
        }
        return $result;
    }

    /**
     * 分段上传文件
     * @param $localFilePath 本地文件
     * @param $path 远程path
     * @param $bucket 桶
     * @return void
     * @throws Exception
     */
    public function uploadLargeFile($localFilePath, $path, $bucket = "xdlyxs", $acl = "public-read")
    {
        try{
            $initiateResponse = $this->s3Client->createMultipartUpload([
                'Bucket' => $bucket,
                "Key" => $path,
            ]);
            $uploadId = $initiateResponse['UploadId'];

            // 打开本地文件
            $fileHandle = fopen($localFilePath, 'r');
            if (!$fileHandle) {
                throw new Exception("Failed to open file: " . $localFilePath);
            }

            $partNumber = 0;
            $parts = [];

            // 分段读取并上传文件
            while (!feof($fileHandle)) {
                $partNumber++;
                $data = fread($fileHandle, 10 * 1024 * 1024); // 每段10MB
                if ($data === false) {
                    break;
                }
                $partResponse = $this->s3Client->uploadPart([
                    'Bucket' => $bucket,
                    'Key' => $path,
                    'UploadId' => $uploadId,
                    'PartNumber' => $partNumber,
                    'Body' => $data,
                    "ACL" => $acl
                ]);

                $parts[] = [
                    'PartNumber' => $partNumber,
                    'ETag' => $partResponse['ETag']
                ];
            }

            fclose($fileHandle);

            // 完成分段上传
            $result = $this->s3Client->completeMultipartUpload([
                'Bucket' => $bucket,
                'Key' => $path,
                'UploadId' => $uploadId,
                'MultipartUpload' => [
                    'Parts' => $parts
                ]
            ]);

            $result['success'] = true;
            $result['message'] = "File uploaded successfully.";
            $result['object_url'] = $result['Location'];

        } catch (S3Exception $e) {
            $result['success'] = false;
            $result['message'] = "S3 error: " . $e->getMessage();
        } catch (AwsException $e) {
            $result['success'] = false;
            $result['message'] = "AWS error: " . $e->getMessage();
            $result['data'] = [
                'aws_request_id' => $e->getAwsRequestId(),
                'aws_error_type' => $e->getAwsErrorType(),
                'aws_error_code' => $e->getAwsErrorCode(),
                'error_details' => $e->toArray()
            ];
        } catch (\Exception $e) {
            $result['success'] = false;
            $result['message'] = "General error: " . $e->getMessage();
        }

        return $result;
    }

    /** 生成预签名上传链接
     * @param $bucketName
     * @param $objectKey
     * @param $expireMinutes
     * @return string
     */
    function generatePresignedUploadUrl($bucketName, $path, $expireMinutes) {
        $cmd = $this->s3Client->getCommand('PutObject', [
            'Bucket' => $bucketName,
            'Key'    => $path
        ]);
        $request = $this->s3Client->createPresignedRequest($cmd, "+" . $expireMinutes . " minutes");
        $presignedUrl = (string)$request->getUri();
        return $presignedUrl;
    }




}
```

> `FilesUpdateController`

```php
class FilesUpdateController extends BaseController
{
    private $accessKey;
    private $secretKey;
    private $endpoint;

    public function __construct()
    {
        $this->accessKey = config('zos.accessKeyId');
        $this->secretKey = config('zos.accessKeySecret');
        $this->endpoint = config('zos.endpoint');

        parent::__construct();
    }
    
    
    #[
        Apidoc\Title("1.上传文件"),
        Apidoc\Method("POST"),
        Apidoc\Param(name:"type", type: "string", require: true, desc: "数据库type"),
        Apidoc\Returned("id", type: "int", desc: "文件id"),
        Apidoc\Returned("path", type: "string", desc: "上传文件路径"),
    ]
    public function upfileFile(Request $request)
    {
        $file = $request->file('file');
        $type = (int) $request->input('type');

        // 获取到登陆token用户id
        $admin_id = $this->adminId;


        if (!$file) {
            return fail("未上传文件");
        }
        // 检查文件是否有效
        if (!$file->isValid()) {
            return fail("文件上传无效");
        }
        if (!$type) {
            return fail("数据库类型不能为空");
        }
        // 获取到 config file
        $db_type = config("file");
        // 检查配置文件中是否存在对应的 type
        if (!isset($db_type[$type])) {
            return fail("上传的数据库类型不存在于配置文件中");
        }
        // 获取对应的数据库名称和表名
        $db_info = $db_type[$type];
        $db_name = $db_info['table'];

        
        $zosUploader = new ZosUploader($this->accessKey, $this->secretKey, $this->endpoint);
        
        $upload = $zosUploader->upload($db_name, $file);

        if($upload['success']){
            $upload['date']['user_id'] = $userid;
            $upload['date']['system_type'] = $type;
            $upload['date']['create_time'] = date('Y-m-d H:i:s');
            $upload['date']['update_time'] = date('Y-m-d H:i:s');

            $file_add =\think\facade\Db::name($db_name)->insert($upload['date']);
            $file_add_id = \think\facade\Db::name($db_name)->insertGetId($upload['date']);

            if($file_add){
                return success(['id' => $file_add_id, 'system_type' => $upload['date']['system_type'], "path" => $upload['ObjectURL']],200, "文件上传成功");
            }
            return fail("文件写入失败");

        }
        return fail("文件上传失败");
    }
}
```







### 3. 封装zos

> 一步一步封装：

> `ZosUploader`

```php
<?php
require __DIR__.'/aws/aws-autoloader.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;

class ZosUploader {
    private $s3Client;
    private $defaultBucket;
    private $defaultStorageClass;
    private $defaultAcl;

    /**
     * 构造函数，初始化S3客户端
     * @param string $accessKey    访问密钥
     * @param string $secretKey    私有密钥
     * @param string $endpoint     服务端点
     * @param string $region       区域（默认为us-east-1）
     * @param string $defaultBucket 默认存储桶
     * @param string $storageClass 默认存储类型（STANDARD/STANDARD_IA/GLACIER）
     * @param string $acl          默认访问权限（private/public-read等）
     */
    public function __construct(
        string $accessKey,
        string $secretKey,
        string $endpoint,
        string $region = 'us-east-1',
        string $defaultBucket = '',
        string $storageClass = 'STANDARD',
        string $acl = 'private'
    ) {
        $this->s3Client = new S3Client([
            'credentials' => [
                'key'    => $accessKey,
                'secret' => $secretKey,
            ],
            'region'   => $region,
            'version'  => '2006-03-01',
            'endpoint' => $endpoint
        ]);

        $this->defaultBucket = $defaultBucket;
        $this->defaultStorageClass = $storageClass;
        $this->defaultAcl = $acl;
    }

    /**
     * 上传文件到ZOS对象存储
     * @param string $localFilePath 本地文件路径
     * @param string $objectKey     对象键名
     * @param string $bucket        存储桶名（为空使用默认桶）
     * @param array  $metadata      元数据数组
     * @return array 包含上传结果或错误信息
     */
    public function uploadFile(
        string $localFilePath,
        string $objectKey,
        string $bucket = '',
        array $metadata = []
    ): array {
        try {
            // 参数校验
            if (!file_exists($localFilePath)) {
                throw new \InvalidArgumentException("文件不存在: $localFilePath");
            }

            $bucket = $bucket ?: $this->defaultBucket;
            if (empty($bucket)) {
                throw new \RuntimeException("未指定存储桶");
            }

            // 读取文件内容
            $body = file_get_contents($localFilePath);

            // 执行上传
            $result = $this->s3Client->putObject([
                'Bucket'        => $bucket,
                'Key'           => $objectKey,
                'Body'          => $body,
                'StorageClass'  => $this->defaultStorageClass,
                'ACL'           => $this->defaultAcl,
                'Metadata'      => $metadata,
            ]);

            // 返回标准化的结果
            return [
                'status'  => 'success',
                'object_url' => $result['ObjectURL'],
                'etag'     => $result['ETag'],
                'details'  => $result->toArray()
            ];
        } catch (S3Exception $e) {
            return $this->handleException($e);
        } catch (AwsException $e) {
            return $this->handleException($e);
        } catch (\Exception $e) {
            return [
                'status' => 'error',
                'code'   => $e->getCode(),
                'message' => $e->getMessage()
            ];
        }
    }

    /**
     * 统一处理AWS异常
     */
    private function handleException(\Exception $e): array {
        if ($e instanceof AwsException) {
            return [
                'status'  => 'error',
                'code'    => $e->getAwsErrorCode(),
                'message' => $e->getAwsErrorMessage(),
                'request_id' => $e->getAwsRequestId(),
                'details' => $e->toArray()
            ];
        }
        return [
            'status' => 'error',
            'code'   => $e->getCode(),
            'message' => $e->getMessage()
        ];
    }

    /**
     * 设置默认存储桶
     */
    public function setDefaultBucket(string $bucket): void {
        $this->defaultBucket = $bucket;
    }
}

/* 使用示例：
$uploader = new ZosUploader(
    'your_access_key',
    'your_secret_key',
    'your_endpoint',
    'us-east-1',
    'default-bucket'
);

$result = $uploader->uploadFile(
    '/path/to/local/file.txt',
    'target-object-key.txt',
    'specific-bucket', // 可选，不传则用默认桶
    ['author' => 'john'] // 可选元数据
);

if ($result['status'] === 'success') {
    echo "上传成功，对象URL: ".$result['object_url'];
} else {
    echo "上传失败: ".$result['message'];
}
*/
```

> 下载

1. 下载方法

```php
<?php
require __DIR__.'/aws/aws-autoloader.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;

class ZosUploader {
    // ... 保留已有属性和方法 ...

    /**
     * 下载对象到本地文件
     * @param string $objectKey     对象键名
     * @param string $savePath      本地保存路径（包含文件名）
     * @param string $bucket        存储桶名（为空使用默认桶）
     * @param string $versionId     对象版本ID（可选）
     * @return array 包含下载结果或错误信息
     */
    public function downloadFile(
        string $objectKey,
        string $savePath,
        string $bucket = '',
        string $versionId = null
    ): array {
        try {
            // 参数校验
            $bucket = $bucket ?: $this->defaultBucket;
            if (empty($bucket)) {
                throw new \RuntimeException("未指定存储桶");
            }

            if (empty($objectKey)) {
                throw new \InvalidArgumentException("对象键不能为空");
            }

            // 确保目录可写
            $saveDir = dirname($savePath);
            if (!is_dir($saveDir) && !mkdir($saveDir, 0755, true)) {
                throw new \RuntimeException("无法创建目录: $saveDir");
            }

            if (!is_writable($saveDir)) {
                throw new \RuntimeException("目录不可写: $saveDir");
            }

            // 构建请求参数
            $params = [
                'Bucket' => $bucket,
                'Key'    => $objectKey
            ];

            if ($versionId) {
                $params['VersionId'] = $versionId;
            }

            // 执行下载
            $result = $this->s3Client->getObject($params);

            // 写入本地文件
            file_put_contents($savePath, $result['Body']);

            // 返回标准化结果
            return [
                'status'         => 'success',
                'save_path'      => realpath($savePath),
                'content_length' => $result['ContentLength'],
                'last_modified'  => $result['LastModified']->format('c'),
                'etag'           => $result['ETag'],
                'metadata'       => $result['Metadata']->toArray()
            ];
        } catch (S3Exception $e) {
            return $this->handleException($e);
        } catch (AwsException $e) {
            return $this->handleException($e);
        } catch (\Exception $e) {
            return [
                'status'  => 'error',
                'code'    => $e->getCode(),
                'message' => $e->getMessage()
            ];
        }
    }

    // ... 保留已有的 handleException 等方法 ...
}

/* 使用示例：
$uploader = new ZosUploader(
    'your_access_key',
    'your_secret_key',
    'your_endpoint',
    'us-east-1',
    'default-bucket'
);

$result = $uploader->downloadFile(
    'target-object-key.txt',
    '/path/to/save/file.txt',
    'specific-bucket', // 可选，不传则用默认桶
    'version-id'       // 可选版本ID
);

if ($result['status'] === 'success') {
    echo "下载成功，保存路径: ".$result['save_path'];
    echo "文件大小: ".$result['content_length']." bytes";
} else {
    echo "下载失败: ".$result['message'];
}
*/
```

2. 流式下载

```php
// 添加流式下载方法
public function downloadStream(string $objectKey, string $bucket = '') {
    $result = $this->s3Client->getObject([
        'Bucket' => $bucket ?: $this->defaultBucket,
        'Key'    => $objectKey
    ]);
    return $result['Body']->detach();
}

// 使用示例：
$stream = $uploader->downloadStream('file.txt');
while (!feof($stream)) {
    echo fread($stream, 1024);
}
fclose($stream);
```

3. 进度监控

```php
// 添加带进度回调的下载
public function downloadWithProgress(
    string $objectKey,
    string $savePath,
    callable $progress,
    string $bucket = ''
) {
    $this->s3Client->getObject([
        'Bucket' => $bucket ?: $this->defaultBucket,
        'Key'    => $objectKey,
        'SaveAs' => $savePath,
        '@http'  => [
            'progress' => $progress
        ]
    ]);
}

// 使用示例：
$uploader->downloadWithProgress('bigfile.zip', '/data/bigfile.zip', 
    function ($downloadTotal, $downloaded) {
        echo "进度: ".round($downloaded/$downloadTotal*100,2)."%\n";
    }
);
```

4. 断点续传

```php
public function resumeDownload(
    string $objectKey,
    string $savePath,
    string $bucket = ''
) {
    $fileSize = file_exists($savePath) ? filesize($savePath) : 0;
    
    $result = $this->s3Client->getObject([
        'Bucket' => $bucket ?: $this->defaultBucket,
        'Key'    => $objectKey,
        'Range'  => "bytes=$fileSize-"
    ]);

    file_put_contents($savePath, $result['Body'], FILE_APPEND);
    
    return [
        'downloaded' => filesize($savePath),
        'total_size' => $result['ContentLength'] + $fileSize
    ];
}
```

> 删除对象

```php
    // 删除单个文件
    public function deleteFile(
        string $objectKey,
        string $bucket = '',
        string $versionId = null
    ): array {
        try {
            $params = $this->baseParams($bucket) + ['Key' => $objectKey];
            if ($versionId) $params['VersionId'] = $versionId;

            $result = $this->s3Client->deleteObject($params);
            return $this->successResult([
                'delete_marker' => $result['DeleteMarker'],
                'version_id'    => $result['VersionId']
            ]);
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }

	// 私有工具方法
    private function baseParams(string $bucket = ''): array {
        return ['Bucket' => $bucket ?: $this->defaultBucket];
    }

    private function formatSource(string $bucket, string $key, ?string $version): string {
        $source = urlencode("$bucket/$key");
        return $version ? "$source?versionId=$version" : $source;
    }

    private function successResult(array $data = []): array {
        return ['status' => 'success'] + $data;
    }

// 删除文件
$operator->deleteFile('file.txt');
```

> 批量删除

```php
    // 批量删除文件
    public function batchDelete(
        array $objects,
        string $bucket = ''
    ): array {
        try {
            $params = $this->baseParams($bucket) + [
                'Delete' => ['Objects' => $objects]
            ];

            $result = $this->s3Client->deleteObjects($params);
            return $this->successResult([
                'deleted' => $result['Deleted'],
                'errors'  => $result['Errors'] ?? []
            ]);
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }

// 批量删除
$operator->batchDelete([
    ['Key' => 'file1.txt'],
    ['Key' => 'file2.txt', 'VersionId' => 'xxx']
]);
```

> 复制文件

```php
    // 复制文件
    public function copyFile(
        string $sourceKey,
        string $targetKey,
        string $sourceBucket = '',
        string $targetBucket = '',
        string $sourceVersion = null
    ): array {
        try {
            $source = $this->formatSource(
                $sourceBucket ?: $this->defaultBucket,
                $sourceKey,
                $sourceVersion
            );

            $result = $this->s3Client->copyObject([
                'Bucket'     => $targetBucket ?: $this->defaultBucket,
                'Key'        => $targetKey,
                'CopySource' => $source
            ]);

            return $this->successResult([
                'etag'          => $result['ETag'],
                'last_modified' => $result['LastModified']->format('c')
            ]);
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }

// 复制文件
$operator->copyFile('source.txt', 'target.txt');
```

> 设置文件标签

```php
    // 设置文件标签
    public function setObjectTags(
        string $objectKey,
        array $tags,
        string $bucket = '',
        string $versionId = null
    ): array {
        try {
            $params = $this->baseParams($bucket) + [
                'Key'     => $objectKey,
                'Tagging' => ['TagSet' => $tags]
            ];
            if ($versionId) $params['VersionId'] = $versionId;

            $this->s3Client->putObjectTagging($params);
            return $this->successResult();
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }

// 设置标签
$operator->setObjectTags('file.txt', [
    ['Key' => 'Category', 'Value' => 'Docs'],
    ['Key' => 'Priority', 'Value' => 'High']
]);

```

> 生成预签名url

```php
    // 生成预签名URL
    public function presignedUrl(
        string $objectKey,
        string $method = 'GET',
        int $expires = 3600,
        string $bucket = ''
    ): array {
        try {
            $command = $this->s3Client->getCommand(
                strtolower($method) === 'put' ? 'PutObject' : 'GetObject',
                $this->baseParams($bucket) + ['Key' => $objectKey]
            );

            $request = $this->s3Client->createPresignedRequest($command, "+$expires seconds");
            return $this->successResult([
                'url' => (string)$request->getUri()
            ]);
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }

// 生成预签名URL
$url = $operator->presignedUrl('file.txt', 'GET', 3600);
```

> 分片上传

```php
   // 分段上传操作
    public function multipartUpload(
        string $localFile,
        string $objectKey,
        string $bucket = '',
        int $partSize = 10 * 1024 * 1024
    ): array {
        try {
            // 初始化上传
            $initResponse = $this->s3Client->createMultipartUpload(
                $this->baseParams($bucket) + ['Key' => $objectKey]
            );
            $uploadId = $initResponse['UploadId'];

            // 分片上传
            $parts = [];
            $file = fopen($localFile, 'rb');
            $partNumber = 1;
            
            while (!feof($file)) {
                $data = fread($file, $partSize);
                $partResult = $this->s3Client->uploadPart([
                    'Bucket'     => $bucket ?: $this->defaultBucket,
                    'Key'        => $objectKey,
                    'UploadId'   => $uploadId,
                    'PartNumber' => $partNumber,
                    'Body'       => $data
                ]);
                
                $parts[] = [
                    'PartNumber' => $partNumber,
                    'ETag'       => $partResult['ETag']
                ];
                $partNumber++;
            }
            fclose($file);

            // 完成上传
            $completeResult = $this->s3Client->completeMultipartUpload([
                'Bucket'   => $bucket ?: $this->defaultBucket,
                'Key'      => $objectKey,
                'UploadId' => $uploadId,
                'MultipartUpload' => ['Parts' => $parts]
            ]);

            return $this->successResult([
                'etag'    => $completeResult['ETag'],
                'version' => $completeResult['VersionId'] ?? null
            ]);
        } catch (\Exception $e) {
            // 发生错误时终止上传
            if (isset($uploadId)) {
                $this->s3Client->abortMultipartUpload([
                    'Bucket'   => $bucket ?: $this->defaultBucket,
                    'Key'      => $objectKey,
                    'UploadId' => $uploadId
                ]);
            }
            return $this->handleException($e);
        }
    }

// 分段上传大文件
$operator->multipartUpload('/bigfile.zip', 'uploads/bigfile.zip');
```

> 高级扩展：

1. 上传监控

```php
// 添加进度回调
public function multipartUpload(
    string $localFile,
    string $objectKey,
    callable $progress = null,
    // ...其他参数...
) {
    // 在每次分片上传后调用
    if ($progress) {
        $progress(filesize($localFile), array_sum(array_column($parts, 'Size')));
    }
}
```

2. 存储策略管理

```php
// 设置对象存储类型
public function setStorageClass(
    string $objectKey,
    string $storageClass,
    string $bucket = ''
) {
    $this->s3Client->copyObject([
        'Bucket'       => $bucket ?: $this->defaultBucket,
        'Key'          => $objectKey,
        'CopySource'   => $this->formatSource($bucket, $objectKey, null),
        'StorageClass' => $storageClass
    ]);
}
```

3. 生命周期

```php
// 设置对象过期时间
public function setExpiration(
    string $objectKey,
    int $days,
    string $bucket = ''
) {
    $this->s3Client->putObject([
        'Bucket'  => $bucket ?: $this->defaultBucket,
        'Key'     => $objectKey,
        'Expires' => time() + ($days * 86400)
    ]);
}
```

4. 设置跨域共享

```php
// 设置桶CORS策略
public function setBucketCors(
    string $bucket,
    array $allowedOrigins,
    array $allowedMethods = ['GET', 'PUT']
) {
    $this->s3Client->putBucketCors([
        'Bucket' => $bucket,
        'CORSConfiguration' => [
            'CORSRules' => [[
                'AllowedHeaders' => ['*'],
                'AllowedMethods' => $allowedMethods,
                'AllowedOrigins' => $allowedOrigins,
                'ExposeHeaders'  => ['ETag'],
                'MaxAgeSeconds'  => 3000
            ]]
        ]
    ]);
}
```





### 4. 整合

> 整合代码

```php
<?php
require __DIR__.'/aws/aws-autoloader.php';

use Aws\S3\S3Client;
use Aws\Exception\AwsException;
use Aws\S3\Exception\S3Exception;

class ZosClient {
    private $s3Client;
    private $defaultBucket;
    private $defaultOptions;

    /**
     * 初始化客户端
     * @param string $accessKey     访问密钥
     * @param string $secretKey     私有密钥
     * @param string $endpoint      服务端点
     * @param string $region        区域（默认us-east-1）
     * @param string $defaultBucket 默认存储桶
     * @param array  $defaultOptions 默认配置选项：
     *     - storage_class: 存储类型（STANDARD/STANDARD_IA/GLACIER）
     *     - acl: 访问权限（private/public-read等）
     *     - auto_create_bucket: 自动创建桶（默认false）
     */
    public function __construct(
        string $accessKey,
        string $secretKey,
        string $endpoint,
        string $region = 'us-east-1',
        string $defaultBucket = '',
        array $defaultOptions = []
    ) {
        $this->s3Client = new S3Client([
            'credentials' => [
                'key'    => $accessKey,
                'secret' => $secretKey,
            ],
            'region'   => $region,
            'version'  => 'latest',
            'endpoint' => $endpoint,
            'http'    => [
                'connect_timeout' => 10,
                'timeout'        => 60
            ]
        ]);

        $this->defaultBucket = $defaultBucket;
        $this->defaultOptions = array_merge([
            'storage_class'    => 'STANDARD',
            'acl'              => 'private',
            'auto_create_bucket' => false
        ], $defaultOptions);
    }

    /******************* 基础操作 *******************/
    
    /**
     * 上传文件
     * @param string $localFile 本地文件路径
     * @param string $objectKey 对象键
     * @param array  $options   可选参数：
     *     - bucket: 指定存储桶
     *     - metadata: 元数据数组
     *     - tags: 标签数组 [['Key'=>'','Value'=>''], ...]
     * @return array 操作结果
     */
    public function upload(string $localFile, string $objectKey, array $options = []): array {
        try {
            $params = $this->buildParams($options) + [
                'Key'           => $objectKey,
                'Body'          => file_get_contents($localFile),
                'StorageClass'  => $options['storage_class'] ?? $this->defaultOptions['storage_class'],
                'ACL'           => $options['acl'] ?? $this->defaultOptions['acl'],
                'Metadata'      => $options['metadata'] ?? [],
                'Tagging'       => !empty($options['tags']) ? http_build_query(array_combine(
                    array_column($options['tags'], 'Key'),
                    array_column($options['tags'], 'Value')
                )) : null
            ];

            $this->checkBucket($params['Bucket']);
            $result = $this->s3Client->putObject($params);
            return $this->success([
                'object_url' => $result['ObjectURL'],
                'etag'       => $result['ETag'],
                'version_id' => $result['VersionId'] ?? null
            ]);
        } catch (\Exception $e) {
            return $this->error($e);
        }
    }

    /**
     * 下载文件
     * @param string $objectKey 对象键
     * @param string $savePath  保存路径
     * @param array  $options   可选参数：
     *     - bucket: 指定存储桶
     *     - version_id: 版本ID
     * @return array 操作结果
     */
    public function download(string $objectKey, string $savePath, array $options = []): array {
        try {
            $params = $this->buildParams($options) + [
                'Key' => $objectKey,
                'VersionId' => $options['version_id'] ?? null
            ];

            $this->prepareDir($savePath);
            
            $result = $this->s3Client->getObject($params);
            file_put_contents($savePath, $result['Body']);
            
            return $this->success([
                'path'          => realpath($savePath),
                'size'          => $result['ContentLength'],
                'last_modified' => $result['LastModified']->format('c'),
                'metadata'      => $result['Metadata']->toArray()
            ]);
        } catch (\Exception $e) {
            return $this->error($e);
        }
    }

    /******************* 高级操作 *******************/

    /**
     * 分片上传（大文件）
     * @param string $localFile   本地文件路径
     * @param string $objectKey   对象键
     * @param array  $options     可选参数：
     *     - bucket: 指定存储桶
     *     - part_size: 分片大小（字节，默认10MB）
     *     - progress: 进度回调函数
     */
    public function multipartUpload(string $localFile, string $objectKey, array $options = []): array {
        try {
            $params = $this->buildParams($options) + ['Key' => $objectKey];
            $partSize = $options['part_size'] ?? 10 * 1024 * 1024;
            $progress = $options['progress'] ?? null;

            // 初始化上传
            $uploadId = $this->s3Client->createMultipartUpload($params)['UploadId'];
            $parts = [];
            $fileSize = filesize($localFile);
            $bytesUploaded = 0;

            // 执行分片上传
            $fp = fopen($localFile, 'rb');
            for ($i = 1; !feof($fp); $i++) {
                $data = fread($fp, $partSize);
                $result = $this->s3Client->uploadPart([
                    'Bucket'     => $params['Bucket'],
                    'Key'        => $objectKey,
                    'UploadId'   => $uploadId,
                    'PartNumber' => $i,
                    'Body'       => $data
                ]);

                $parts[] = ['PartNumber' => $i, 'ETag' => $result['ETag']];
                $bytesUploaded += strlen($data);

                // 进度回调
                if ($progress) {
                    $progress($bytesUploaded, $fileSize);
                }
            }
            fclose($fp);

            // 完成上传
            $result = $this->s3Client->completeMultipartUpload([
                'Bucket'   => $params['Bucket'],
                'Key'      => $objectKey,
                'UploadId' => $uploadId,
                'MultipartUpload' => ['Parts' => $parts]
            ]);

            return $this->success([
                'etag'       => $result['ETag'],
                'version_id' => $result['VersionId'] ?? null
            ]);
        } catch (\Exception $e) {
            if (isset($uploadId)) {
                $this->s3Client->abortMultipartUpload([
                    'Bucket'   => $params['Bucket'],
                    'Key'      => $objectKey,
                    'UploadId' => $uploadId
                ]);
            }
            return $this->error($e);
        }
    }

    /******************* 管理操作 *******************/

    /**
     * 删除对象
     * @param string|array $objects 对象键或对象数组（批量删除）
     * @param array $options 可选参数：
     *     - bucket: 指定存储桶
     *     - version_id: 版本ID（单对象）
     */
    public function delete($objects, array $options = []): array {
        try {
            $params = $this->buildParams($options);

            // 批量删除
            if (is_array($objects)) {
                $params['Delete'] = ['Objects' => array_map(function($obj) {
                    return is_string($obj) ? ['Key' => $obj] : $obj;
                }, $objects)];

                $result = $this->s3Client->deleteObjects($params);
                return $this->success([
                    'deleted' => $result['Deleted'],
                    'errors'  => $result['Errors'] ?? []
                ]);
            }

            // 单对象删除
            $params['Key'] = $objects;
            if (isset($options['version_id'])) {
                $params['VersionId'] = $options['version_id'];
            }

            $result = $this->s3Client->deleteObject($params);
            return $this->success([
                'delete_marker' => $result['DeleteMarker'],
                'version_id'    => $result['VersionId'] ?? null
            ]);
        } catch (\Exception $e) {
            return $this->error($e);
        }
    }

    /**
     * 生成预签名URL
     * @param string $objectKey 对象键
     * @param string $method    请求方法（GET/PUT）
     * @param int    $expires   有效期（秒）
     * @param array  $options   可选参数：
     *     - bucket: 指定存储桶
     */
    public function presignedUrl(string $objectKey, string $method = 'GET', int $expires = 3600, array $options = []): array {
        try {
            $command = $this->s3Client->getCommand(
                strtoupper($method) === 'PUT' ? 'PutObject' : 'GetObject',
                $this->buildParams($options) + ['Key' => $objectKey]
            );

            $request = $this->s3Client->createPresignedRequest($command, "+$expires seconds");
            return $this->success(['url' => (string)$request->getUri()]);
        } catch (\Exception $e) {
            return $this->error($e);
        }
    }

    /******************* 辅助方法 *******************/

    private function buildParams(array $options): array {
        $params = ['Bucket' => $options['bucket'] ?? $this->defaultBucket];
        if ($this->defaultOptions['auto_create_bucket']) {
            $this->createBucketIfNotExists($params['Bucket']);
        }
        return $params;
    }

    private function checkBucket(string $bucket): void {
        if (!$this->s3Client->doesBucketExist($bucket)) {
            if ($this->defaultOptions['auto_create_bucket']) {
                $this->s3Client->createBucket(['Bucket' => $bucket]);
            } else {
                throw new \RuntimeException("Bucket $bucket 不存在");
            }
        }
    }

    private function prepareDir(string $path): void {
        $dir = dirname($path);
        if (!file_exists($dir) && !mkdir($dir, 0755, true)) {
            throw new \RuntimeException("无法创建目录: $dir");
        }
        if (!is_writable($dir)) {
            throw new \RuntimeException("目录不可写: $dir");
        }
    }

    private function success(array $data = []): array {
        return ['status' => 'success', 'data' => $data];
    }

    private function error(\Throwable $e): array {
        $error = ['status' => 'error', 'message' => $e->getMessage()];
        if ($e instanceof AwsException) {
            $error['code'] = $e->getAwsErrorCode();
            $error['request_id'] = $e->getAwsRequestId();
        }
        return $error;
    }
}

/* 使用示例：
$zos = new ZosClient(
    'your_ak',
    'your_sk',
    'your_endpoint',
    'us-east-1',
    'default-bucket',
    ['auto_create_bucket' => true]
);

// 简单上传
$result = $zos->upload('/data/file.txt', 'docs/file.txt');
if ($result['status'] === 'success') {
    echo "上传成功，ETag: ".$result['data']['etag'];
}

// 分片上传（带进度）
$zos->multipartUpload('/data/bigfile.iso', 'backups/bigfile.iso', [
    'part_size' => 100 * 1024 * 1024, // 100MB分片
    'progress' => function ($uploaded, $total) {
        echo "进度: ".round($uploaded/$total*100, 1)."%\n";
    }
]);

// 生成下载链接
$url = $zos->presignedUrl('files/private.txt', 'GET', 300);
echo "下载链接（5分钟有效）: ".$url['data']['url'];

// 批量删除
$result = $zos->delete([
    'file1.txt',
    ['Key' => 'file2.txt', 'VersionId' => 'xxx']
]);
*/
```

> 高级功能

1. 带标签和元数据上传

```php
$zos->upload('photo.jpg', 'user_uploads/avatar.jpg', [
    'metadata' => ['user_id' => '12345'],
    'tags' => [
        ['Key' => 'Category', 'Value' => 'Avatar'],
        ['Key' => 'Status', 'Value' => 'Unverified']
    ]
]);
```

2. 断点续传分片上传

```php
// 第一次上传（失败）
try {
    $zos->multipartUpload('bigfile.zip', 'backups/bigfile.zip');
} catch (\Exception $e) {
    // 记录UploadId
    $uploadId = $e->getUploadId(); // 需要扩展异常处理
}

// 恢复上传
$zos->resumeMultipartUpload('bigfile.zip', 'backups/bigfile.zip', $uploadId, [
    'part_size' => 50 * 1024 * 1024
]);
```

> 3. 加密上传

```php
$zos->upload('secret.txt', 'encrypted/secret.txt', [
    'server_side_encryption' => 'AES256'
]);
```

4. 预上传后端前端

```php
$policy = $zos->generatePostPolicy('user_uploads/', [
    'expires' => '+1 hour',
    'conditions' => [
        ['content-length-range', 0, 10485760] // 限制10MB
    ]
]);

// 前端表单示例：
// <form action="https://your-endpoint" method="post" enctype="multipart/form-data">
//   <input type="hidden" name="key" value="user_uploads/${filename}">
//   <input type="hidden" name="policy" value="<?= $policy['policy'] ?>">
//   <input type="hidden" name="signature" value="<?= $policy['signature'] ?>">
//   <input type="file" name="file">
//   <button>Upload</button>
// </form>
```

