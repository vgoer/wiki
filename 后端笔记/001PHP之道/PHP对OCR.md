<center>PHP对OCR</center>





[toc]











## PHP对OCR

> 与 PHP 内部的 Tesseract OCR 协同工作的包装器。 [github](https://github.com/thiagoalessio/tesseract-ocr-for-php)





### 1. 安装使用

```shell
composer require thiagoalessio/tesseract_ocr
```

>  **该库依赖于[Tesseract OCR](https://github.com/tesseract-ocr/tesseract)，版本\*3.02\*或更高版本。**



```php
# 使用
use thiagoalessio\TesseractOCR\TesseractOCR;
echo (new TesseractOCR('text.png'))
    ->run();
```



























### 2. 第三方ocr

> 对接阿里云的ocr。[doc ](https://api.aliyun.com/api/ocr-api/2021-07-07)  [api](https://api.aliyun.com/api/ocr-api/2021-07-07/RecognizeAllText)

```shell
composer require alibabacloud/ocr-api-20210707
```

> `env`文件记得写入

```shell
# 阿里云sms
ALIYUN_KEY = 
ALIYUN_SECRET = 
# ocr Endpoint
ALIYUN_KEY_OCR = ocr-api.cn-hangzhou.aliyuncs.com

```



```php
<?php

namespace app\library\Ocr;

use Darabonba\OpenApi\OpenApiClient;
use AlibabaCloud\Credentials\Credential;
use AlibabaCloud\OpenApiUtil\OpenApiUtilClient;
use Darabonba\OpenApi\Models\Config;
use Darabonba\OpenApi\Models\Params;
use AlibabaCloud\Tea\Utils\Utils\RuntimeOptions;
use Darabonba\OpenApi\Models\OpenApiRequest;

class AliyunOcrSample
{
    /**
     * 使用凭据初始化账号Client
     * @return OpenApiClient Client
     */
    public static function createClient(){
        // 工程代码建议使用更安全的无AK方式，凭据配置方式请参见：https://help.aliyun.com/document_detail/311677.html。
        $credConfig = new Config([
            'type' => 'access_key',
            'accessKeyId' => getenv('ALIYUN_KEY'),
            'accessKeySecret' => getenv('ALIYUN_SECRET'),
        ]);

        $credClient = new Credential($credConfig);

        // Endpoint 请参考 https://api.aliyun.com/product/ocr-api
        $credConfig->endpoint = getenv('ALIYUN_KEY_OCR');
        return new OpenApiClient($credConfig);
    }

    /**
     * API 相关
     * @return Params OpenApi.Params
     */
    public static function createApiInfo(){
        $params = new Params([
            // 接口名称
            "action" => "RecognizeAllText",
            // 接口版本
            "version" => "2021-07-07",
            // 接口协议
            "protocol" => "HTTPS",
            // 接口 HTTP 方法
            "method" => "POST",
            "authType" => "AK",
            "style" => "V3",
            // 接口 PATH
            "pathname" => "/",
            // 接口请求体内容格式
            "reqBodyType" => "json",
            // 接口响应体内容格式
            "bodyType" => "json"
        ]);
        return $params;
    }

    /**
     * 获取ocr信息。
     * @param string $url 图片
     * @param string $type 类型
     * @return array 返回数据
     */
    public static function getOcrInfo(string $url ,string $type = 'Advanced'){
        $client = self::createClient();
        $params = self::createApiInfo();
        // query params
        $queries = [];
        $queries["Url"] = $url;
        $queries["Type"] = $type;
        // runtime options
        $runtime = new RuntimeOptions([]);
        $request = new OpenApiRequest([
            "query" => OpenApiUtilClient::query($queries)
        ]);
        // 复制代码运行请自行打印 API 的返回值
        // 返回值实际为 Map 类型，可从 Map 中获得三类数据：响应体 body、响应头 headers、HTTP 返回的状态码 statusCode。
        return $client->callApi($params, $request, $runtime);
    }

}
```

```php
class OcrService
{
    use Singleton;

    public function getOcrInfo(string $url)
    {
        $result = AliyunOcrSample::getOcrInfo($url);

        return $result;
    }
}
```

```shell
# 方法
    public function getOrcInfo(Request $request)
    {

        $url = 'http://s3.xchuxing.com/xchuxing/article/2022/12/02/c7cc7202212022040361808.jpg';

        $result = OcrService::instance()->getOcrInfo($url);

        return success($result);
    }
```

