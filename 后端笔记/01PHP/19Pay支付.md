<center>Pay支付</center>





[toc]









### Pay支付

> php对接各个支付平台。
>
> [github](https://github.com/yansongda/pay) [doc](https://pay.yansongda.cn/)







### 1. 安装

* ### hyperf/laravel 用户

```shell
composer require yansongda/pay:~3.7.0 -vvv
composer require guzzlehttp/guzzle:^7.0 # 默认情况下，相关框架已自带，无需额外安装
```

* ### 其它框架/无框架 用户

```shell
composer require yansongda/pay:~3.7.0 -vvv
composer require guzzlehttp/guzzle:^7.0
composer require hyperf/pimple:~2.2.0  # 或者 composer require illuminate/container
```







### 2. 初始化

> SDK 一旦初始化后，底层使用单例模式保存配置信息，所以，每次使用只需初始化一次即可，无需多次，后续重复初始化将不会生效 当然，您也可以使用 `_force` 参数强制初始化覆盖原来的配置项。

```php
<?php
return [
    'alipay' => [
        'default' => [
            // 「必填」支付宝分配的 app_id
            'app_id' => '2016082000295641',
            // 「必填」应用私钥 字符串或路径
            // 在 https://open.alipay.com/develop/manage 《应用详情->开发设置->接口加签方式》中设置
            'app_secret_cert' => 'MIIEvAIBADANBgkqhkiG9w0BAQEFAASCBKYwggSiAgEAAoIBAQCDRjOg5DnX+8L+rB8d2MbrQ30Z7JPM4hiDhawHSwQCQ7RlmQNpl6b/N6IrPLcPFC1uii179U5Il5xTZynfjkUyJjnHusqnmHskftLJDKkmGbSUFMAlOv+NlpUWMJ2A+VUopl+9FLyqcV+XgbaWizxU3LsTtt64v89iZ2iC16H6/6a3YcP+hDZUjiNGQx9cuwi9eJyykvcwhDkFPxeBxHbfwppsul+DYUyTCcl0Ltbga/mUechk5BksW6yPPwprYHQBXyM16Jc3q5HbNxh3660FyvUBFLuVWIBs6RtR2gZCa6b8rOtCkPQKhUKvzRMlgheOowXsWdk99GjxGQDK5W4XAgMBAAECggEAYPKnjlr+nRPBnnNfR5ugzH67FToyrU0M7ZT6xygPfdyijaXDb2ggXLupeGUOjIRKSSijDrjLZ7EQMkguFHvtfmvcoDTDFaL2zq0a3oALK6gwRGxOuzAnK1naINkmeOmqiqrUab+21emEv098mRGbLNEXGCgltCtz7SiRdo/pgIPZ1wHj4MH0b0K2bFG3xwr51EyaLXKYH4j6w9YAXXsTdvzcJ+eRE0Yq4uGPfkziqg8d0xXSEt90HmCGHKo4O2eh1w1IlBcHfK0F6vkeUAtrtAV01MU2bNoRU147vKFxjDOVBlY1nIZY/drsbiPMuAfSsodL0hJxGSYivbKTX4CWgQKBgQDd0MkF5AIPPdFC+fhWdNclePRw4gUkBwPTIUljMP4o+MhJNrHp0sEy0sr1mzYsOT4J20hsbw/qTnMKGdgy784bySf6/CC7lv2hHp0wyS3Es0DRJuN+aTyyONOKGvQqd8gvuQtuYJy+hkIoHygjvC3TKndX1v66f9vCr/7TS0QPywKBgQCXgVHERHP+CarSAEDG6bzI878/5yqyJVlUeVMG5OXdlwCl0GAAl4mDvfqweUawSVFE7qiSqy3Eaok8KHkYcoRlQmAefHg/C8t2PNFfNrANDdDB99f7UhqhXTdBA6DPyW02eKIaBcXjZ7jEXZzA41a/zxZydKgHvz4pUq1BdbU5ZQKBgHyqGCDgaavpQVAUL1df6X8dALzkuqDp9GNXxOgjo+ShFefX/pv8oCqRQBJTflnSfiSKAqU2skosdwlJRzIxhrQlFPxBcaAcl0VTcGL33mo7mIU0Bw2H1d4QhAuNZIbttSvlIyCQ2edWi54DDMswusyAhHxwz88/huJfiad1GLaLAoGASIweMVNuD5lleMWyPw2x3rAJRnpVUZTc37xw6340LBWgs8XCEsZ9jN4t6s9H8CZLiiyWABWEBufU6z+eLPy5NRvBlxeXJOlq9iVNRMCVMMsKybb6b1fzdI2EZdds69LSPyEozjkxdyE1sqH468xwv8xUPV5rD7qd83+pgwzwSJkCgYBrRV0OZmicfVJ7RqbWyneBG03r7ziA0WTcLdRWDnOujQ9orhrkm+EY2evhLEkkF6TOYv4QFBGSHfGJ0SwD7ghbCQC/8oBvNvuQiPWI8B+00LwyxXNrkFOxy7UfIUdUmLoLc1s/VdBHku+JEd0YmEY+p4sjmcRnlu4AlzLxkWUTTg==',
            // 「必填」应用公钥证书 路径
            // 设置应用私钥后，即可下载得到以下3个证书
            'app_public_cert_path' => '/Users/yansongda/pay/cert/appCertPublicKey_2016082000295641.crt',
            // 「必填」支付宝公钥证书 路径
            'alipay_public_cert_path' => '/Users/yansongda/pay/cert/alipayCertPublicKey_RSA2.crt',
            // 「必填」支付宝根证书 路径
            'alipay_root_cert_path' => '/Users/yansongda/pay/cert/alipayRootCert.crt',
            'return_url' => 'https://yansongda.cn/alipay/return',
            'notify_url' => 'https://yansongda.cn/alipay/notify',
            // 「选填」第三方应用授权token
            'app_auth_token' => '',
            // 「选填」服务商模式下的服务商 id，当 mode 为 Pay::MODE_SERVICE 时使用该参数
            'service_provider_id' => '',
            // 「选填」默认为正常模式。可选为： MODE_NORMAL, MODE_SANDBOX, MODE_SERVICE
            'mode' => Pay::MODE_NORMAL,
        ]
    ],
    'wechat' => [
        'default' => [
            // 「必填」商户号，服务商模式下为服务商商户号
            // 可在 https://pay.weixin.qq.com/ 账户中心->商户信息 查看
            'mch_id' => '',
            // 「选填」v2商户私钥
            'mch_secret_key_v2' => '',
            // 「必填」v3 商户秘钥
            // 即 API v3 密钥(32字节，形如md5值)，可在 账户中心->API安全 中设置
            'mch_secret_key' => '',
            // 「必填」商户私钥 字符串或路径
            // 即 API证书 PRIVATE KEY，可在 账户中心->API安全->申请API证书 里获得
            // 文件名形如：apiclient_key.pem
            'mch_secret_cert' => '',
            // 「必填」商户公钥证书路径
            // 即 API证书 CERTIFICATE，可在 账户中心->API安全->申请API证书 里获得
            // 文件名形如：apiclient_cert.pem
            'mch_public_cert_path' => '',
            // 「必填」微信回调url
            // 不能有参数，如?号，空格等，否则会无法正确回调
            'notify_url' => 'https://yansongda.cn/wechat/notify',
            // 「选填」公众号 的 app_id
            // 可在 mp.weixin.qq.com 设置与开发->基本配置->开发者ID(AppID) 查看
            'mp_app_id' => '2016082000291234',
            // 「选填」小程序 的 app_id
            'mini_app_id' => '',
            // 「选填」app 的 app_id
            'app_id' => '',
            // 「选填」服务商模式下，子公众号 的 app_id
            'sub_mp_app_id' => '',
            // 「选填」服务商模式下，子 app 的 app_id
            'sub_app_id' => '',
            // 「选填」服务商模式下，子小程序 的 app_id
            'sub_mini_app_id' => '',
            // 「选填」服务商模式下，子商户id
            'sub_mch_id' => '',
            // 「选填」（适用于 2024-11 及之前开通微信支付的老商户）微信支付平台证书序列号及证书路径，强烈建议 php-fpm 模式下配置此参数
            // 「必填」微信支付公钥ID及证书路径，key 填写形如 PUB_KEY_ID_0000000000000024101100397200000006 的公钥id，见 https://pay.weixin.qq.com/doc/v3/merchant/4013053249
            'wechat_public_cert_path' => [
                '45F59D4DABF31918AFCEC556D5D2C6E376675D57' => __DIR__.'/Cert/wechatPublicKey.crt',
                'PUB_KEY_ID_0000000000000024101100397200000006' => __DIR__.'/Cert/publickey.pem',
            ],
            // 「选填」默认为正常模式。可选为： MODE_NORMAL, MODE_SERVICE
            'mode' => Pay::MODE_NORMAL,
        ]
    ],
    'unipay' => [
        'default' => [
            // 「必填」商户号
            'mch_id' => '777290058167151',
            // 「选填」商户密钥：为银联条码支付综合前置平台配置：https://up.95516.com/open/openapi?code=unionpay
            'mch_secret_key' => '979da4cfccbae7923641daa5dd7047c2',
            // 「必填」商户公私钥
            'mch_cert_path' => __DIR__.'/Cert/unipayAppCert.pfx',
            // 「必填」商户公私钥密码
            'mch_cert_password' => '000000',
            // 「必填」银联公钥证书路径
            'unipay_public_cert_path' => __DIR__.'/Cert/unipayCertPublicKey.cer',
            // 「必填」
            'return_url' => 'https://yansongda.cn/unipay/return',
            // 「必填」
            'notify_url' => 'https://yansongda.cn/unipay/notify',
            'mode' => Pay::MODE_NORMAL,
        ],
    ],
    'douyin' => [
        'default' => [
            // 「选填」商户号
            // 抖音开放平台 --> 应用详情 --> 支付信息 --> 产品管理 --> 商户号
            'mch_id' => '73744242495132490630',
            // 「必填」支付 Token，用于支付回调签名
            // 抖音开放平台 --> 应用详情 --> 支付信息 --> 支付设置 --> Token(令牌)
            'mch_secret_token' => 'douyin_mini_token',
            // 「必填」支付 SALT，用于支付签名
            // 抖音开放平台 --> 应用详情 --> 支付信息 --> 支付设置 --> SALT
            'mch_secret_salt' => 'oDxWDBr4U7FAAQ8hnGDm29i4A6pbTMDKme4WLLvA',
            // 「必填」小程序 app_id
            // 抖音开放平台 --> 应用详情 --> 支付信息 --> 支付设置 --> 小程序appid
            'mini_app_id' => 'tt226e54d3bd581bf801',
            // 「选填」抖音开放平台服务商id
            'thirdparty_id' => '',
            // 「选填」抖音支付回调地址
            'notify_url' => 'https://yansongda.cn/douyin/notify',
        ],
    ],
    'jsb' => [
        'default' => [
            // 服务代码
            'svr_code' => '',
            // 「必填」合作商ID
            'partner_id' => '',
            // 「必填」公私钥对编号
            'public_key_code' => '00',
            // 「必填」商户私钥(加密签名)
            'mch_secret_cert_path' => '',
            // 「必填」商户公钥证书路径(提供江苏银行进行验证签名用)
            'mch_public_cert_path' => '',
            // 「必填」江苏银行的公钥(用于解密江苏银行返回的数据)
            'jsb_public_cert_path' => '',
            // 支付通知地址
            'notify_url' => '',
            // 「选填」默认为正常模式。可选为： MODE_NORMAL:正式环境, MODE_SANDBOX:测试环境
            'mode' => Pay::MODE_NORMAL,
        ],
    ],
    'logger' => [
        'enable' => false,
        'file' => './logs/pay.log',
        'level' => 'info', // 建议生产环境等级调整为 info，开发环境为 debug
        'type' => 'single', // optional, 可选 daily.
        'max_file' => 30, // optional, 当 type 为 daily 时有效，默认 30 天
    ],
    'http' => [ // optional
        'timeout' => 5.0,
        'connect_timeout' => 5.0,
        // 更多配置项请参考 [Guzzle](https://guzzle-cn.readthedocs.io/zh_CN/latest/request-options.html)
    ],
];
```



### 3. 支付宝

> 支付宝

```php
1. 网页

Pay::config($this->config);

// 注意返回类型为 Response，具体见详细文档
return Pay::alipay()->web([
    'out_trade_no' => ''.time(),
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 1',
]);

// 如果想获取跳转代码（表单形式），可以使用如下代码（详情请自行了解 PSR 规范）
// $web = Pay::alipay()->web([
//     'out_trade_no' => ''.time(),
//     'total_amount' => '0.01',
//     'subject' => 'yansongda 测试 - 1',
// ]);

// return (string) $web->getBody();


2. h5
 
Pay::config($this->config);

// 注意返回类型为 Response，具体见详细文档
return Pay::alipay()->h5([
    'out_trade_no' => time(),
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 01',
    'quit_url' => 'https://yansongda.cn',
 ]);


3. app
    
Pay::config($this->config);

// 注意返回类型为 Response，具体见详细文档
return Pay::alipay()->app([
    'out_trade_no' => time(),
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 01',
]);


4. 小程序
    
Pay::config($this->config);

$result = Pay::alipay()->mini([
    'out_trade_no' => time().'',
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 01',
    'buyer_id' => '2088622190161234',
]);

return $result->get('trade_no');  // 支付宝交易号
// return $result->trade_no;

5. 刷卡支付（付款码，被扫码）
    
Pay::config($this->config);

$result = Pay::alipay()->pos([
    'out_trade_no' => time(),
    'auth_code' => '284776044441477959',
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 01',
]);


6. 扫描支付
    
Pay::config($this->config);

$result = Pay::alipay()->scan([
    'out_trade_no' => time(),
    'total_amount' => '0.01',
    'subject' => 'yansongda 测试 - 01',
]);

return $result->qr_code; // 二维码 url


7. 转账
    
Pay::config($this->config);

$result = Pay::alipay()->transfer([
    'out_biz_no' => '202106051432',
    'trans_amount' => '0.01',
    'product_code' => 'TRANS_ACCOUNT_NO_PWD',
    'biz_scene' => 'DIRECT_TRANSFER',
    'payee_info' => [
        'identity' => 'ghdhjw7124@sandbox.com',
        'identity_type' => 'ALIPAY_LOGON_ID',
        'name' => '沙箱环境'
    ],
]);

8. 退款
    
Pay::config($this->config);

$result = Pay::alipay()->refund([
    'out_trade_no' => '1623160012',
    'refund_amount' => '0.01',
    // '_action' => 'agreement', // 商家收款退款
    // '_action' => 'authorization', // 预授权退款
    // '_action' => 'transfer', // 转账退款
]);


9. 查询订单
    
Pay::config($this->config);

$order = [
    'out_trade_no' => '1514027114',
    // '_action' => 'agreement', // 商家收款查询
    // '_action' => 'authorization', // 预授权查询
    // '_action' => 'transfer', // 转账查询
    // '_action' => 'face' // 刷脸结果信息查询
    // '_action' => 'transfer' // 转账查询
    // '_action' => 'refund' // 退款查询
];

$result = Pay::alipay()->query($order);

10 支付宝回调处理
    
Pay::config($this->config);

$result = Pay::alipay()->callback();

11.响应支付宝回调
    
Pay::config($this->config);

return Pay::alipay()->success();
```

