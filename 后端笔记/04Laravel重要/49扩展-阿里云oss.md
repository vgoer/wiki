<center>阿里云 OSS</center>





[toc]





## 阿里云oss

>  文件我们大多使用oss。 注册找到 key id [blog](https://blog.csdn.net/weixin_45606067/article/details/114292972)







###  1. 安装

```php
composer require aliyuncs/oss-sdk-php
```





### 2. 公共控制器

```php
// app/Http/Controller  AliyunOss.php
<?php

namespace App\Http\Controllers;

use AlibabaCloudCredentialsWrapper;
use App\Http\Controllers\Controller;
use OSS\Core\OssException;
use OSS\Core\OssUtil;
use OSS\OssClient;

class AliyunOss extends Controller
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



### 3. 单独的文件上传的控制器

```php
// FileController.php
<?php

namespace App\Http\Controllers\Admin;

use App\Http\Controllers\AliyunOss;
use App\Http\Controllers\Controller;
use App\Http\Models\Admin\Guarantee;
use Illuminate\Http\Request;

class FileController extends Controller
{

    private $accessKeyId = '';
    private $accessKeySecret = '';
    private $bucket = 'baotongdzbh';
    private $endpoint = 'oss-cn-guangzhou.aliyuncs.com';
    
    public function pdf(Request $request)
    {
        $input = $request->all();
        
        $file = $request->file('file');
        $origin_ext = $file->extension();
        if ($origin_ext != 'pdf') return error(560001);
        if (empty($input['case_no']) ) return error(60016);
        
        $types = [
            'guarantee_slip',
        ];

        if (! in_array($input['type'], $types)) return error(500002);
        
        // 上传成功
        switch ($input['type']) {
            // 开工合共
            case 'guarantee_slip' : $type = 20; break;
        }

        // 文件信息
        $file = $request->file('file');
        $origin_name = $file->getClientOriginalName();
        $file_content = file_get_contents($file->getRealPath());

        // 生成目录
        $time = time();
        $y = date('Y', $time);
        $m = date('m', $time);
        $d = date('d', $time);
        $path = 'case/' . $y . '/' . $m . '/' . $d . '/';
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
            // 上传成功
            // $full_path = 'https://' . $this->bucket . '.' . $this->endpoint . '/' . $file_path;
            // 修改过
            $full_path =  $file_path;
            // echo $full_path;exit;
            
            // 写入记录
            $case = Guarantee::query()
                        ->where("case_no",$input['case_no'])
                        ->first();
            $formdata = json_decode($case->formdata,true);
            $formdata['guarantee_slip'] = [
                'url'         =>  $full_path,
                'name'        =>  $origin_name,
                'fileType'    =>  "applicationForm",
                'remak'       =>  '投保单'
            ];
            $formdata_json = json_encode($formdata);
            $result = Guarantee::query()
                        ->where("case_no",$input['case_no'])    
                        ->update([
                            'formdata' => $formdata_json
                        ]);
            if (! $result) {
                return error(510003);
            }
            return success([
                'case_no'     => $input['case_no'],
                'origin_name' => $origin_name, 
                'origin_ext'  => $origin_ext, 
                'file_name'   => $file_name,
                'url'         => $full_path
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

