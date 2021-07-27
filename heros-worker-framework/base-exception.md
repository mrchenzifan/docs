# 异常处理

对于框架抛出的异常或又是用户自己抛出的异常默认会交至`\app\exception\Handler` 类处理， 这个是可以在 `config/exception.php` 配置文件中设置（如果有需要设置成别的异常处理类）

## 定义异常处理类

异常处理类需要继承`framework\exception\ExceptionHandler`类， eg:

```php
<?php
namespace app\exception;

use framework\exception\ExceptionHandler;
use framework\http\Request;
use framework\http\Response;
use Throwable;

/**
 * Class Handler
 * @package app\exception
 */
class Handler extends ExceptionHandler
{
    public $ignoreReport = [
        AuthException::class,
    ];

    /**
     * @param Request $request
     * @param Throwable $e
     * @return Response
     */
    public function render(Request $request, Throwable $e): Response
    {
        return parent::render($request, $e);
    }
}
```

- `$ingoreReport` 属性，排除某些不需要记录日志的异常

- `render(Request $request, Throwable $e): Response` 处理异常， 返回回需要是 `framework\http\Response` 对象， 可以使用 `packResult($object)` 把对象包装成 *Response* 对象， 传递的 *$object* 需要实现 `__toString()` 方法

> 在 `framework\exception\ExceptionHandler::render` 方法中， 如果异常存在 `render()` 方法， 则会进一步递交给异常本身的 `render()` 处理， 否则会返回 `500` 响应

### 异常定义

项目中异常定义， 可以实现 `render()` 方法， 这样异常处理就交至异常类自己处理逻辑， eg:

```php
<?php

namespace app\exception;

use framework\http\Response;
use framework\util\Result;

class AuthException extends \RuntimeException
{
    public function render(): Response
    {
        return packResult(Result::error()->code('002')->message('登陆超时，请重新登陆'));
    }
}
```
