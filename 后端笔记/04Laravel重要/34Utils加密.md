<center>Utils加密</center>





[toc]









## Utils

> 加密加签解密解签。让数据传输更安全



```php
<?php

namespace App\Http\Controllers;
class RSAUtil
{

    // 公钥&私钥
	public static  $RIVATE_KEY = "MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAKBTLSQEnW2bQfUg2UrJn4lEmpQEnLfn9JKHV2ejmJavoWWJ9cOZOVhF11mH51MRL1M42LBzgvAKpI2rmTGjMAuZTwZMWlME013ka6t+1qPmxPpnfRwsTwwq9hHY8zNEjyRRW82grM6CRU2Ztgm2wq0bY0Bk3CWB8ZOBscFTr/LlAgMBAAECgYBavdkNysLjt31EZXw29Rkj0z1+S4H8IP/vM0UINrL1jqBV3RjJxV6MlLMHTIFkJZTYkJMsg6R3gj6SpK4HRyq4bf0pX6EGSbofjmWtPWLN8p76BN4AIaxtLgWnowTfHgL4ZwjCWJX+Yw3rFfGH8UqwFzKbXkeDMJ6VJfYkO4uD/QJBAOtJTXrf9nvAk2V/1wyxKmTAskYQhgRhiHF22pOFtJnKMsD1EIeCmP1k6WoWIZm8lLRvpRrFYoLp3bSCrAPAxp8CQQCucHPGwdBjDoRYOPUai1/BU0seubt5mqJ0Rnr93mPHg4VQGSfNFiV4+eKBpwenn6xhKcs4u3bnzQ4ol95UX6v7AkEA29PF9yqvERpw3GEf3DTe5fl/1qRzgj5aC6C/QRuoDBP1bYDJ68HiDMWuqzZ4ODoQObEh8iw/CQ9V2+RGsM75AwJAErLJJkgGP2gB9bb9RwAjnoSAK+X625kgytf3PRlGls9ZTfG0W36BO8uFZSJzZptuDeg9+XHW2BgZ6W4GDgNHWwJAIG2rucsSVZXUx/bKtDp2lUwRvo4hHuqlrW+U/krMDcMUZgWkSOv/kyCZEErEXeorIFnW/TiX6QWbPTAXfEiwDA==";
	public static  $PUBLIC_KEY = "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCgUy0kBJ1tm0H1INlKyZ+JRJqUBJy35/SSh1dno5iWr6FlifXDmTlYRddZh+dTES9TONiwc4LwCqSNq5kxozALmU8GTFpTBNNd5Gurftaj5sT6Z30cLE8MKvYR2PMzRI8kUVvNoKzOgkVNmbYJtsKtG2NAZNwlgfGTgbHBU6/y5QIDAQAB";


    /**
     * RSA最大加密明文大小
     */
    const MAX_ENCRYPT_BLOCK = 117;

    /**
     * RSA最大解密密文大小
     */
    const MAX_DECRYPT_BLOCK = 128;


    /**
     * 获取密钥对
     *
     * @return array 密钥对
     */
    public static function getKeyPair()
    {
        $config = array(
            "digest_alg"       =>  "sha512",
            "private_key_bits" =>  1024,
            "private_key_type" =>  OPENSSL_KEYTYPE_RSA,
        );
        // 生成一个密钥对
        $res = openssl_pkey_new($config);
        
        if ($res === false) {
            $err = openssl_error_string();
            echo "密钥对生成失败: " . $err . PHP_EOL;
            exit;
        }

        openssl_pkey_export($res, $privateKey);
        $publicKey = openssl_pkey_get_details($res);
        $publicKey = $publicKey["key"];
        return array("private" => $privateKey, "public" => $publicKey);
    }

    /**
     * 获取私钥
     *
     * @param privateKey 私钥字符串
     * @return PriveteKey
     */
    public static function getPrivateKey($privateKey)
    {
        # 改为 PEM 格式
        $privateKey = "-----BEGIN PRIVATE KEY-----\n" . chunk_split($privateKey, 64, "\n") . "-----END PRIVATE KEY-----\n";
        // $res = openssl_pkey_get_private($privateKey);
        // var_dump($res);
        // exit;
        return openssl_pkey_get_private($privateKey);
    }

    /**
     * 获取公钥
     *
     * @param publicKey 公钥字符串
     * @return PublicKey
     */
    public static function getPublicKey($publicKey)
    {   

        # 改为 PEM 格式
        $publicKey = "-----BEGIN PUBLIC KEY-----\n" . chunk_split($publicKey, 64, "\n") . "-----END PUBLIC KEY-----\n";
        return openssl_pkey_get_public($publicKey);
    }

    /**
     * RSA加密
     *
     * @param data      待加密数据
     * @param publicKey 公钥
     * @return String
     */
    public static function encrypt($data, $publicKey)
    {
        
        $encryptedData = '';
        // 对数据分段加密
        while (strlen($data)) {
            $chunk = substr($data, 0, static::MAX_ENCRYPT_BLOCK);
            $data = substr($data, static::MAX_ENCRYPT_BLOCK);

            $ret = openssl_public_encrypt($chunk, $encryptData, $publicKey, OPENSSL_PKCS1_PADDING);

            if (!$ret) {
                return false;
            }
            $encryptedData .= $encryptData;
        }
        
        // 获取加密内容使用base64进行编码
        return base64_encode($encryptedData);
    }

    /**
     * RSA解密
     *
     * @param data       待解密数据
     * @param privateKey 私钥
     * @return String
     */
    public static function decrypt($data, $privateKey)
    {
        $decryptedData = '';
        # 上面 用了 base64编码了，现在要解码
        $data = base64_decode($data);
        
        // 对数据分段解密
        while (strlen($data)) {
            $chunk = substr($data, 0, static::MAX_DECRYPT_BLOCK);
            $data = substr($data, static::MAX_DECRYPT_BLOCK);

            $ret = openssl_private_decrypt($chunk, $decryptData, $privateKey, OPENSSL_PKCS1_PADDING);

            if (!$ret) {
                return false;
            }
            $decryptedData .= $decryptData;
        }
        
        // 为什么是空呢        
        // var_dump($decryptData);
        
        return $decryptedData;
    }

    /**
     * 签名
     *
     * @param data       待签名数据
     * @param privateKey 私钥
     * @return String
     */
    public static function sign($data, $privateKey)
    {
        $signature = '';
        openssl_sign($data, $signature, $privateKey, OPENSSL_ALGO_MD5);
        // var_dump($signature);

        // 获取签名内容使用base64进行编码
        return base64_encode($signature);
    }

    /**
     * 验签
     *
     * @param srcData   原始字符串
     * @param publicKey 公钥
     * @param sign      签名
     * @return bool 是否验签通过
     */
    public static function verify($srcData, $publicKey, $sign)
    {
        
        $signature = base64_decode($sign);
        return openssl_verify($srcData, $signature, $publicKey, OPENSSL_ALGO_MD5) == 1;
    }

    /**
     * 加密 Map
     *
     * @param data map 数据
     * @return String 包含签名后的加密内容
     */
    public static function encryptMap($data)
    {
        // 对数据进行排列
        ksort($data);
        
        if(isset($data['files'])){
            ksort($data['files'][0]);
        }

        // 将map转化为Json串并统一格式化，带上sign参数

        $signData = json_encode((object)$data,JSON_UNESCAPED_UNICODE);
        // var_dump($signData);
        // var_dump($signData);echo '<br>';
        // exit;
        
        // var_dump(self::$RIVATE_KEY);
        
        $data['sign'] = self::sign($signData, self::getPrivateKey(self::$RIVATE_KEY));

        // RSA加密
        $paramData = json_encode((object)$data,JSON_UNESCAPED_UNICODE);
        
        return self::encrypt($paramData, self::getPublicKey(self::$PUBLIC_KEY));
    }

    /**
     * 解密 Map
     *
     * @param param 待解密字符串
     * @return Array 签名正常则返回 map 数据内容否也为空
     */
    public static function decryptMap($param)
    {
        try {
            //解密
            $param = self::decrypt($param, self::getPrivateKey(self::$RIVATE_KEY));
            
            # $param = self::decrypt($param, self::$RIVATE_KEY);
            if (empty($param)) {
                return null;
            }
                
            //拿到真实数据&签名后数据
            $map = json_decode($param, JSON_UNESCAPED_UNICODE);
            $sign = $map['sign'];
                
            unset($map['sign']);
            //按照字典顺序对待验值字段排序（先升序排列参数，然后按照键值拼接）
            ksort($map);

            // array_walk_recursive($map,'ksort');
            // var_dump($map);eixt;
            // $str = json_encode($map);echo $str;exit;
            
            // var_dump($map);exit;
            
            //验签
            $verify = self::verify(json_encode($map,JSON_UNESCAPED_UNICODE), self::getPublicKey(self::$PUBLIC_KEY), $sign);
            # $verify = self::verify(json_encode($map, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT), self::$PUBLIC_KEY, $sign);
            if ($verify) {
                return $map;
            } else {
                echo "签名认证失败";
                exit;
            }
        } catch (Exception $e) {
            echo $e->getMessage();
        };
        
        return null;
    }
}


```

> `encryptMap`加密 `decryptMap`解密







### 2. laravel加密

> Laravel 的加密服务提供了一个简单、方便的接口，使用 OpenSSL 所提供的 AES-256 和 AES-128 加密和解密文本。所有 Laravel 加密的结果都会使用消息认证码 (MAC) 进行签名，因此一旦加密，其底层值就不能被修改或篡改。
>



```php
# 生成key
php artisan key:generate
    
# 加密
    /**
     *  为用户存储一个 DigitalOcean API 令牌。
     */
    public function store(Request $request): RedirectResponse
{
    $request->user()->fill([
        'token' => Crypt::encryptString($request->token),
    ])->save();

    return redirect('/secrets');
}

```

> 解密

```php
use Illuminate\Contracts\Encryption\DecryptException;
use Illuminate\Support\Facades\Crypt;

try {
    $decrypted = Crypt::decryptString($encryptedValue);
} catch (DecryptException $e) {
    // ...
}
```

