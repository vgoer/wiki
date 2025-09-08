<center>PHP对接阿里云sms</center>







[toc]







## PHP对接阿里云sms

> 对接阿里云短信服务。
>
> [sms](https://www.aliyun.com/product/sms)

> 不使用官方的sdk： [easy-sms](https://github.com/overtrue/easy-sms)



### 1. 获取密钥对

> 鼠标移至头像，选择 AccessKey 管理。

![image-20250321165902802](.\assets\image-20250321165902802.png)

> 创建密钥对

![image-20250321165953158](.\assets\image-20250321165953158.png)

> 创建短信签名和模板

![image-20250321170045335](.\assets\image-20250321170045335.png)







### 2. 使用sdk

```shell
composer require alibabacloud/dysmsapi
```

> 新建`AliyunSms`

```php
<?php

namespace utils;

use AlibabaCloud\Client\AlibabaCloud;
use AlibabaCloud\Client\Exception\ClientException;
use AlibabaCloud\Client\Exception\ServerException;

class AliyunSms
{
    private $accessKeyId;
    private $accessKeySecret;
    private $signName;
    private $templateCode;

    // 单例实例
    private static $instance;

    // 私有构造函数，防止外部实例化
    private function __construct($accessKeyId, $accessKeySecret, $signName, $templateCode)
    {
        $this->accessKeyId = $accessKeyId;
        $this->accessKeySecret = $accessKeySecret;
        $this->signName = $signName;
        $this->templateCode = $templateCode;
    }

    // 获取单例实例
    public static function getInstance()
    {
        if (!self::$instance) {
            $accessKeyId = getenv("ALIYUN_KEY");
            $accessKeySecret = getenv("ALIYUN_SECRET");
            $signName = getenv("ALIYUN_SIGN_NAME");
            $templateCode = getenv("ALIYUN_TEMPLATE_CODE");

            self::$instance = new self($accessKeyId, $accessKeySecret, $signName, $templateCode);
        }
        return self::$instance;
    }

    /**
     * 发送验证码
     * @param $phone 手机号
     * @param $templateParam 验证码等参数
     * @return bool 成功是否
     */
    public function sendSms($phone, $templateParam)
    {
        try {
            AlibabaCloud::accessKeyClient($this->accessKeyId, $this->accessKeySecret)
                ->regionId('cn-hangzhou')
                ->asDefaultClient();

            $result = AlibabaCloud::rpc()
                ->product('Dysmsapi')
                ->version('2017-05-25')
                ->action('SendSms')
                ->method('POST')
                ->options([
                    'query' => [
                        'PhoneNumbers' => $phone,
                        'SignName' => $this->signName,
                        'TemplateCode' => $this->templateCode,
                        'TemplateParam' => json_encode($templateParam),
                    ],
                ])
                ->request();

            $response = $result->toArray();

            if ($response['Code'] == 'OK') {
                return true;
            } else {
                return false;
            }
        } catch (ClientException $e) {
            echo "客户端异常: " . $e->getErrorMessage();
            return false;
        } catch (ServerException $e) {
            echo "服务器异常: " . $e->getErrorMessage();
            return false;
        }
    }

}
```

> 使用： 

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use utils\AliyunSms;
use Illuminate\Support\Facades\Redis;

class SmsController extends Controller
{
    #[
        Apidoc\Title("1.发送验证码"),
        Apidoc\Method("POST"),
        Apidoc\Param(name:"phone", type: "string", require: true, desc: "手机号"),
        Apidoc\Returned("id", type: "int", desc: "文件id"),
        Apidoc\Returned("code", type: "string", desc: "验证码code"),
    ]
    public function sendRegisterCode(Request $request)
    {
        $params = $request->all();

        if (empty($params['phone'])) {
            return fail("手机号不能为空");
        }

        // 判断手机号是否已注册
        if ($this->isPhoneRegistered($params['phone'])) {
            return fail("该手机号已注册");
        }

        // 生成验证码
        $code = $this->generateCode();

        // 存储验证码到 Redis，设置五分钟有效期
        $redisKey = 'sms_code_' . $params['phone'];
        Redis::set($redisKey, $code, 'EX', 300); // 300秒 = 5分钟

        // 设置短信接收号码和模板参数
        $templateParam = [
            'code' => $code  // 验证码
        ];

        $sms = AliyunSms::getInstance();
        $result = $sms->sendSms($params['phone'], $templateParam);

        if ($result) {
            return success("短信发送成功");
        } else {
            return fail("短信发送失败");
        }
    }

    // 判断手机号是否已注册
    private function isPhoneRegistered($phone)
    {
        // 假设用户表为 users，手机号字段为 phone
        $user = \App\Models\User::where('phone', $phone)->first();
        return $user !== null;
    }

    // 生成验证码
    private function generateCode()
    {
        return rand(100000, 999999); // 生成6位随机数字
    }
}
```

