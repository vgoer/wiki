---
title: 17Fun函数库
description: 
published: 1
date: 2023-08-17T11:17:30.480Z
tags: 
editor: markdown
dateCreated: 2023-08-17T11:17:28.952Z
---

<center>php函数库</center>

[toc]



### 常用函数
> php 常用的函数库



1. 删除目录

```php
function rm_file($dir){
    /**
     * 删除目录
     */
    $all = scandir($dir);
    foreach($all as $file){
        if($file == '.' || $file == '..'){
            continue;
        }
        $path = $dir.'/'.$file;
        if(is_file($path)){
            unlink($path);
        }else{
            rm_file($path);
        }
    }

    rmdir($dir);
}
```

2. 打印数组函数

```php
function print_p($arr){
    /**
     * 打印数组
     */
    echo '<pre>';
    print_r($arr);
    echo '</pre>';
}

```


3. php获取客户端真实ip(即便他使用代理服务器)

```php

function getRealIpAddr() { 
    if (!empty($_SERVER['HTTP_CLIENT_IP'])) { 
        $ip=$_SERVER['HTTP_CLIENT_IP']; 
    } elseif (!empty($_SERVER['HTTP_X_FORWARDED_FOR'])) { 
        $ip=$_SERVER['HTTP_X_FORWARDED_FOR']; 
    } else { 
        $ip=$_SERVER['REMOTE_ADDR']; 
    } 
     
    return $ip; 
}


```


4. 文件下载

```php

function force_download($file)
{
    if ((isset($file)) && (file_exists($file))) {
        //这是一个文件流格式的文件
        Header("Content-type: application/octet-stream");
        //请求范围的度量单位--字节
        Header("Accept-Ranges: bytes");
        //Content-Length是指定包含于请求或响应中数据的字节长度
        Header("Accept-Length: " . filesize($file));
        //用来告诉浏览器，文件是可以当做附件被下载，下载后的文件名称为$file_name该变量的值。
        Header("Content-Disposition: attachment; filename=".date('Y-m-d-H:i:s',time()) . $file);
        readfile("$file");
    } else {
        echo "No file selected";
    }
}

// a链接下载文件
<a href="http://study.cn/doload.php">下载zip22</a>


```


5. 随机生成不重复的字符串

```php
function round_str($num){
    /**
     * 随机生成字符串 不重复的
     */

    // 合并数组 array_merge
    $arr = array_merge(range('A','Z'),range(0,9),range('a','z'),range('a','Z'));
    
    $rand_arr = [];
    for($i =0; $i < $num; $i++){
        // array_rand 返回数组中的一个随机键名
        $rand = $arr[array_rand($arr)];
        if(!in_array($rand,$rand_arr)){
            $rand_arr[] = $rand;
        }else{
            $i--;
        }
    }

    $rand_str = join('',$rand_arr);
    return $rand_str;

}

echo round_str(60);
```


6. 发送请求函数

```php
// 初始化 curl库
    public function initCurl($url,$data)
    {
        $options = array(
            'http' => array(
                'method'  => 'POST',
                'header'  => 'Content-type:text/plain',
                'content' => $data
                )
            );
        $context = stream_context_create($options);
        $resopse = file_get_contents($url,false,$context);

        if($resopse === false) return false;

        return json_decode($resopse);
    }

```


7. 返回函数

```php
/**
 * 成功打印的函数
 *
 * @param array $data 数据
 * @param integer $code 状态码
 * @param string $msg   返回提示
 * @param integer $count 获取次数
 * @return json 返回json
 */
function success($data = [], $code = 200, $msg ='', $count = 0)
{
    return json_encode([
        'code'   =>   $code,
        'msg'    =>   $msg == '' ? '操作成功' : $msg,
        'count'  =>   $count == 0 ? count($data) : $count,
        'data'   =>   $data
    ]);
}
```

8. 返回错误信息

```php
/**
 * 返回错误信息
 *
 * @param integer $code 错误码
 * @return void|json $res_data 返回json
 */
function error($code = 500001)
{
    $error_code = [
        // 系统
        500000 => '系统繁忙',
        500001 => '操作失败',
        500002 => '无效参数',
        500003 => '文件上传失败',

        // 登录返回
        500100 =>   '账号不存在',

        // 等等
    ];

    if(empty($code)) return false;

    $res_json = [
        'code'  => $code,
        'msg'   => $error_code[$code]
    ];

    return json_encode($res_json);
}
```

9. 转财务货币

```php

// 数字转财务货币
function rmb($money = 0, $int_unit = '元', $is_round = true, $is_extra_zero = false)
{
    // 将数字切分成两段
    $parts = explode( '.', $money, 2);
    $int = isset($parts[0]) ? strval($parts[0]) : '0';
    $dec = isset($parts[1]) ? strval($parts[1]) : '';
    // 如果小数点后多于2位，不四舍五入就直接截，否则就处理
    $dec_len = strlen ( $dec );
    if (isset ( $parts [1] ) && $dec_len > 2) {
        $dec = $is_round ? substr(strrchr(strval(round(floatval("0." . $dec), 2)), '.'), 1) : substr ($parts [1], 0, 2);
    }
    // 当number为0.001时，小数点后的金额为0元
    if (empty($int) && empty($dec)) {
        return '零';
    }
    // 定义
    $chs = ['0', '壹', '贰', '叁', '肆', '伍', '陆', '柒', '捌', '玖'];
    $uni = ['', '拾', '佰', '仟'];
    $dec_uni = ['角', '分'];
    $exp = ['', '万'];
    $res = '';
    // 整数部分从右向左找
    for($i = strlen($int) - 1, $k = 0; $i >= 0; $k ++) {
        $str = '';
        // 按照中文读写习惯，每4个字为一段进行转化，i一直在减
        for($j = 0; $j < 4 && $i >= 0; $j ++, $i --) {
            $u = $int[$i] > 0 ? $uni[$j] : ''; // 非0的数字后面添加单位
            $str = $chs[$int[$i]] . $u . $str;
        }
        $str = rtrim($str, '0'); // 去掉末尾的0
        $str = preg_replace("/0+/", "零", $str); // 替换多个连续的0
        if (! isset ($exp[$k])) {
            $exp[$k] = $exp[$k - 2] . '亿'; // 构建单位
        }
        $u2 = $str != '' ? $exp[$k] : '';
        $res = $str . $u2 . $res;
    }
    // 如果小数部分处理完之后是00，需要处理下
    $dec = rtrim($dec, '0');
    // 小数部分从左向右找
    if (! empty($dec)) {
        $res .= $int_unit;
        // 是否要在整数部分以0结尾的数字后附加0，有的系统有这要求
        if ($is_extra_zero) {
            if (substr($int, - 1) === '0') {
                $res .= '零';
            }
        }
        for($i = 0, $cnt = strlen($dec); $i < $cnt; $i ++) {                 
            $u = $dec[$i] > 0 ? $dec_uni[$i] : ''; // 非0的数字后面添加单位
            $res .= $chs[$dec[$i]] . $u;
            if ($cnt == 1) $res .= '整';
        }
        $res = rtrim($res, '0'); // 去掉末尾的0
        $res = preg_replace("/0+/", "零", $res); // 替换多个连续的0
    } else {
        $res .= $int_unit . '整';
    }
    return $res;
}
```

11. 函数用于获取当前的毫秒级时间戳

```php
// 取微秒
function mTime()
{
    list($msec, $sec) = explode(' ', microtime());
    return $sec * 1000 + intval($msec * 1000);
}

# 获取token等 
$token = encrypt(mTime());
```

12. 获取密码盐

```php
/**
 * @ Salt 生成密码盐
 * param len 默认长度 4
 */
function salt($len = 4)
{
    $rand_str = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $max = strlen($rand_str);
    $str = '';
    for ($i = 0; $i < $len; $i++) {
        $str .= $rand_str[mt_rand(0, $max - 1)];
    }
    return $str;
}
# 加密
$salt = salt(10);
$password = md5(md5($input['password']) . $salt);
```

13. 发送请求

```php

function curl($method = 'get', $url, $header = [], $data = [])
{
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_URL, $url);
    curl_setopt($curl, CURLOPT_SSL_VERIFYPEER, FALSE);
    curl_setopt($curl, CURLOPT_SSL_VERIFYHOST, FALSE);
    if ($header == []) {
        curl_setopt($curl, CURLOPT_HEADER, 0);
    } else {
        curl_setopt($curl, CURLOPT_HEADER, $header);
    }
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
    if ($method == 'post') {
        curl_setopt($curl, CURLOPT_POST, 1);
        curl_setopt($curl, CURLOPT_POSTFIELDS, $data);
    }
    $data = curl_exec($curl);
    curl_close($curl);
    return $data;
}
```

14. 修改状态的函数

```php
/**
 * 根据数据库状态返回一些值给前端
 *
 * @param Number $key
 * @return void
 */
function rolesType($key){
    switch($key){
        // 0 admin |  1 user |  2 待定 |  3 监管部门
        case 0: $value = ['admin'];      break;
        case 1: $value = ['user'];       break;
        // case 2: $value = ['user'];       break;
        case 3: $value = ['supervise'];  break;
    }
    return $value;
}

```

12. 是否是中国的手机号

```php
function validate_phone_number($phone) {
    // 去除字符串中的空格和特殊字符
    $cleaned_phone = preg_replace('/[^0-9]/', '', $phone);

    // 验证手机号码是否为11位数字
    if (strlen($cleaned_phone) !== 11) {
        return false;
    }

    // 验证手机号码是否符合中国的号码规则
    $pattern = '/^(?:\+?86)?1(?:3\d{3}|4[5-9]\d{2}|5\d{3}|6[2389]\d{2}|7[0-8]\d{2}|8\d{3}|9[189]\d{2})\d{6}$/';
    if (!preg_match($pattern, $cleaned_phone)) {
        return false;
    }

    // 手机号码验证通过
    return true;
}

```

13. 用于验证IP地址（支持IPv4和IPv6）是否合法：

```php
function validate_ip_address($ip) {
    // 验证IPv4地址
    if (filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4)) {
        return true;
    }

    // 验证IPv6地址
    if (filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV6)) {
        return true;
    }

    // IP地址格式不正确
    return false;
}
```

14. 用于将 JSON 字符串转换为数组

```php
function convert_json_to_array($json) {
    // 解码 JSON 字符串并转换为数组
    $array = json_decode($json, true);

    // 检查 JSON 解码是否成功
    if ($array === null && json_last_error() !== JSON_ERROR_NONE) {
        return null;
    }

    return $array;
}
```





