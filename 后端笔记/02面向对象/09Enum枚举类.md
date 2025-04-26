<center>枚举类</center>



[toc]







## 枚举类

> 枚举相关共用的都放在一起。





```php
<?php

namespace utils;

enum CommonEnum
{


    /**
     * 状态-正常
     */
     const STATUS_Y = 1;
    /**
     * 状态-禁用
     */
    const STATUS_N = 2;

    /**
     * 成功返回code
     */
    const RETURN_CODE_SUCCESS = 200;

    /**
     * 需要弹窗提示的code
     */
    const RETURN_CODE_ERROR_PARAM = 300;

    /**
     * 失败返回code
     */
    const RETURN_CODE_FAIL = 400;

    /**
     * 认证失败返回code
     */
    const RETURN_CODE_ERROR_AUTH = 401;

    /**
     * 微信自动登录失败-跳转至短信登录
     */
    const LOGIN_FAIL = 402;






}

```

> 小的枚举类

```php
<?php

namespace app\enum;

enum PaymentRecordEnum
{


    /**
     * 支付状态10=支付中20=支付成功30=支付失败
     * @var array
     */
    const STATUS = [
        'ing' => [
            'value' => 10,
            'str' => '支付中',
        ],
        'success' => [
            'value' => 20,
            'str' => '支付成功',
        ],
        'fail' => [
            'value' => 30,
            'str' => '支付失败',
        ],

    ];

    public static function getStatusList(): array
    {
        return getEnumList(self::STATUS);
    }



}

```

