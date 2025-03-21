<center>PHP对接阿里云OSS</center>





[toc]









## PHP对接阿里云OSS

> PHP对接阿里云的对象存储。oss





### 1. 前期准备

\1. **注册阿里云账号**：如果还没有阿里云账号，请先注册。
\2. **创建OSS存储空间**：登录阿里云控制台，创建一个新的OSS存储空间（Bucket）。
\3. **获取访问密钥**：在阿里云控制台，找到并记录你的AccessKey ID和AccessKey Secret。







### 2.安装sdk和配置

```shell
composer require aliyun/aliyun-oss-php-sdk
```

> `AliyunOss`

```php
<?php
namespace App\Http\Controllers;

use OSS\Core\OssException;
use OSS\OssClient;

class AliyunOss
{
    private $ossClient;
    private $endpoint;
    
    public function __construct($accessKeyId, $accessKeySecret, $endpoint)
    {
        try {
            $this->ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
            $this->endpoint = $endpoint;
            return True;
        } catch (OssException $e) {
            printf(__FUNCTION__ . "creating OssClient instance: FAILED\n");
            printf($e->getMessage() . "\n");
            return False;
        }
    }
    
    // 上传文件
    public function upLoadFile($bucket, $object, $content, $options = NULL)
    {
        try{
            $this->ossClient->putObject($bucket, $object, $content, $options);
            return 'https://' . $bucket . '.' . $this->endpoint . '/' . $object;
        } catch(OssException $e) {
            printf(__FUNCTION__ . ": FAILED\n");
            printf($e->getMessage() . "\n");
            return False;
        }
    }
    
    // 读取文件内容
    public function downLoadFile($bucket, $object)
    {
        try{
            return $this->ossClient->getObject($bucket, $object);
        } catch(OssException $e) {
            printf(__FUNCTION__ . ": FAILED\n");
            printf($e->getMessage() . "\n");
            return;
        }
    }
    
    // 删除文件
    public function removeFile($bucket, $object)
    {
        $object = str_replace('https://' . $bucket . '.' . $this->endpoint . '/', '', $object);
        try{
            $this->ossClient->deleteObject($bucket, $object);
            return True;
        } catch(OssException $e) {
            printf(__FUNCTION__ . ": FAILED\n");
            printf($e->getMessage() . "\n");
            return False;
        }
    }
}


```

> `FileController`

```php
<?php

namespace App\Http\Controllers\Api\V3;

use App\Http\Controllers\AliyunOss;

use Illuminate\Http\Request;

class FileController extends Controller
{
    private $accessKeyId = '';
    private $accessKeySecret = '';
    private $bucket = '';
    private $endpoint = '';
    
    
     public function getFileBase64(Request $request)
    {
        $input = $request->all();
        
        if (empty($input['url'])) return error(500002);
        
        // 'Content-Type' => 'image/jpeg'
        // return response($this->downLoad($input['url']), 200, [
            // 'Content-Type' => 'application/pdf'
        // ]);
        $content = base64_encode($this->downLoad($input['url']));
        return $this->success(['base64' => $content]);
    }
    
    public function license(Request $request)
    {
        $input = $request->all();
        
        $types = [
            'license', 
            'permit',
            'corporate_card',
            'corporate_card_re',
            'handled_card',
            'handled_card_re',
        ];
        
        if (! in_array($input['type'], $types)) return error(500002);
        
        // 上传成功
        switch ($input['type']) {
            // 营业执照
            case 'license': $type = 1; break;
            // 开户许可证
            case 'permit': $type = 2; break;
            // 法人身份证
            case 'corporate_card' : $type = 3; break;
            case 'corporate_card_re' : $type = 4; break;
            // 经办人
            case 'handled_card' : $type = 5; break;
            case 'handled_card_re' : $type = 6; break;
        }
        
        // 文件信息
        $file = $request->file('file');
        
        $origin_name = $file->getClientOriginalName();
        // echo $origin_name;exit;
        
        $origin_ext = $file->extension();
        
        $file_content = file_get_contents($file->getRealPath());
        
        // 生成目录
        $time = time();
        $y = date('Y', $time);
        $m = date('m', $time);
        $d = date('d', $time);
        $path = 'enterprise/' . $y . '/' . $m . '/' . $d . '/';
        // 生成文件名
        $mTime = mTime();
        $file_name = $mTime . '.' . $origin_ext;
        // 路径
        $file_path = $path . $file_name;
        $result = $this->upLoad($file_path, $file_content);
        if (! $result) {
            // 上传失败
            return error(500001);
        } else {
            // $full_path = 'https://' . $this->bucket . '.' . $this->endpoint . '/' . $file_path;
            $full_path = $file_path;
            // echo $full_path;exit;
            // 写入记录
            $upload = new EnterpriseUploadModel;
            $upload->enterprise_id = $input['enterprise_id'];
            $upload->type          = $type;
            $upload->file_path     = $full_path;
            $result = $upload->save();
            if (! $result) {
                return error(500001);
            }
            return success([
                'origin_name' => $origin_name, 
                'origin_ext'  => $origin_ext, 
                'file_name'   => $file_name,
                'url'         => $full_path,
                'type'        => $input['type'],
            ]);
        }
    }
 
    
    
        public function upLoad($file, $content)
    {
        $oss = new AliyunOss($this->accessKeyId, $this->accessKeySecret, $this->endpoint);
        return $oss->upLoadFile($this->bucket, $file, $content);
    }
    
    public function downLoad($path)
    {
        $oss = new AliyunOss($this->accessKeyId, $this->accessKeySecret, $this->endpoint);
        return $oss->downLoadFile($this->bucket, $path);
    }
    
    public function remove($path)
    {
        $oss = new AliyunOss($this->accessKeyId, $this->accessKeySecret, $this->endpoint);
        return $oss->removeFile($this->bucket, $path);
    }
    
    

    
}
```





### 3. 客户端直传

> 前端直接上传到对象存储里面
>
> [help](https://help.aliyun.com/zh/oss/use-cases/uploading-objects-to-oss-directly-from-clients/#section-wqp-pzp-fhb)
>
> 客户端直传是指客户端直接上传文件到对象存储OSS。相对于服务端代理上传，客户端直传避免了业务服务器中转文件，提高了上传速度，节省了服务器资源。本文介绍客户端直传的方案优势、安全实现和实践参考。

![image-20250321172833402](.\assets\image-20250321172833402.png)

1. 跨域设置

> 如果您的客户端是Web端或小程序，您需要解决跨域访问被限制的问题。浏览器以及小程序容器出于安全考虑，通常都会限制跨域访问，这一限制也会限制您的客户端代码直连OSS。您可以通过配置OSS Bucket的跨域访问规则，来允许指定域名下的Web应用或小程序直接访问OSS。更多信息，请参见[跨域设置](https://help.aliyun.com/zh/oss/user-guide/cors-12/?spm=a2c4g.11186623.0.0.37432235IqEWCy)。https://help.aliyun.com/zh/oss/user-guide/cors-12/?spm=a2c4g.11186623.0.0.37432235IqEWCy)

2. 服务端代码

```php
<?php
require_once __DIR__ . '/vendor/autoload.php';
use OSS\OssClient;
use OSS\Core\OssException;
use OSS\Http\RequestCore;
use OSS\Http\ResponseCore;
// 运行本示例代码之前，请确保已设置环境变量ALIBABA_CLOUD_ACCESS_KEY_ID和ALIBABA_CLOUD_ACCESS_KEY_SECRET。
$accessKeyId = getenv("ALIBABA_CLOUD_ACCESS_KEY_ID");
$accessKeySecret = getenv("ALIBABA_CLOUD_ACCESS_KEY_SECRET");
// yourEndpoint填写Bucket所在地域对应的Endpoint。以华东1（杭州）为例，Endpoint填写为https://oss-cn-hangzhou.aliyuncs.com。
$endpoint = "<YOUR-ENDPOINT>";
// 填写Bucket名称。
$bucket= "<YOUR-BUCKET>";
// 填写不包含Bucket名称在内的Object完整路径。
$object = "test.png";
// 设置签名URL的有效时长为3600秒。
$timeout = 3600;
try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
    // 生成签名URL。
    $signedUrl = $ossClient->signUrl($bucket, $object, $timeout, "PUT", array('Content-Type' => 'image/png'));
    // 打印返回的数据
    echo $signedUrl;
} catch (OssException $e) {
    printf($e->getMessage() . "\n");
    return;
}
```

3. 客户端代码

```js
const form = document.querySelector("form");
form.addEventListener("submit", (event) => {
  event.preventDefault();
  const fileInput = document.querySelector("#file");
  const file = fileInput.files[0];
  fetch("/get_presigned_url_for_oss_upload", { method: "GET" })
    .then((response) => {
      if (!response.ok) {
        throw new Error("获取预签名URL失败");
      }
      return response.text();
    })
    .then((url) => {
      fetch(url, {
        method: "PUT",
        headers: new Headers({
          "Content-Type": "image/png",
        }),
        body: file,
      }).then((response) => {
        if (!response.ok) {
          throw new Error("文件上传到OSS失败");
        }
        console.log(response);
        alert("文件已上传");
      });
    })
    .catch((error) => {
      console.error("发生错误:", error);
      alert(error.message);
    });
});
```

