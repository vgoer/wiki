<center>Webman Exception</center>





[toc]







## Webman Exception

> webman 异常处理。





> 1. 重写异常 `utils/Handler.php`

```php
<?php
namespace utils;

use Throwable;
use Webman\Http\Request;
use Webman\Http\Response;
use Webman\Exception\ExceptionHandler;

/**
 * Class Handler
 * @package plugin\saiadmin\app\exception
 */
class Handler extends ExceptionHandler
{
    public function render(Request $request, Throwable $exception): Response
    {
        $debug = config('app.debug', true);
        $code = $exception->getCode();
        $json = [
            'code' => $code ? $code : 500,
            'message' => $code !== 500 ? $exception->getMessage() : 'Server internal error',
        ];
        if ($debug) {
            $json['request_url'] = $request->method() . ' ' . $request->uri();
            $json['timestamp'] = date('Y-m-d H:i:s');
            $json['client_ip'] = $request->getRealIp();
            $json['request_param'] = $request->all();
            $json['exception_handle'] = get_class($exception);
            $json['exception_info'] = [
                'code' => $exception->getCode(),
                'message' => $exception->getMessage(),
                'file' => $exception->getFile(),
                'line' => $exception->getLine(),
                'trace' => explode("\n", $exception->getTraceAsString())
            ];
        }
        return new Response(200, ['Content-Type' => 'application/json;charset=utf-8'], json_encode($json));
    }
}

```

> 修改： `config/exception.php`

```php
<?php
/**
 * This file is part of webman.
 *
 * Licensed under The MIT License
 * For full copyright and license information, please see the MIT-LICENSE.txt
 * Redistributions of files must retain the above copyright notice.
 *
 * @author    walkor<walkor@workerman.net>
 * @copyright walkor<walkor@workerman.net>
 * @link      http://www.workerman.net/
 * @license   http://www.opensource.org/licenses/mit-license.php MIT License
 */

return [
//    '' => support\exception\Handler::class,
    '' => \utils\Handler::class,
];
```

> 使用：

```php
use support\exception\BusinessException;
use app\enum\CommonEnum;

throw new BusinessException('查询失败', CommonEnum::RETURN_CODE_FAIL);
```

