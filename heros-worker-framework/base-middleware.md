# 中间件

中间件是作用于`request`请求和`response`响应之间

## 中间件代码

> 命令快速生成中间件： `php artisan make:middleware -path=app/modules/demo/middleware -class=M1Middleware`； 运行后会在 app/modules/demo/middleware 这个目录下生成`M1Middleware.php`文件

示例代码：

```php
<?php
namespace app\modules\demo\middleware;

use framework\http\Request;

/**
 * Class M1Middleware
 * @package app\modules\demo\middleware
 */
class M1Middleware
{
    public function handle(Request $request, array $vars, array $extVars, \Closure $next)
    {
        //doSomething
        return $next($request, $vars, $extVars);
    }
}
```

### 参数分析

- `Request $resqut` http请求， 可获取请求参数、头、路由等信息
- `array $vars` 包含路由匹配的参数 以及 http请求的参数
- `array $extVars` 包含一些框架内置参数 `framework\http\Request` 请求， `framework\http\Response` 响应， `framework\http\Session` session, `Workerman\Connection\TcpConnection` 当前tcp连接
- `\Closure $next` 下一个处理闭包

## 在配置文件中配置中间件

配置文件 `config/middleware.php`,  `global` 为全局中间件， 其他为自定义配置， 配置键值是由路由来匹配，比如访问路由：`http://127.0.0.1:8080/demo/middleware/test2` 就会匹配到`/demo/middleware`键值配置的中间件
执行顺序: `GlobalMiddleware-> M1Middleware -> M2Middleware`

```php
<?php

return [
    'global' => [
        \app\middleware\GlobalMiddleware::class,
    ],
    '/demo/middleware' => [
        \app\modules\demo\middleware\M1Middleware::class,
        \app\modules\demo\middleware\M2Middleware::class,
    ],
];
```

## 局部自定义中间件

除了在配置文件中配置大范围的中间件，还可以在控制器中使用配置局部中间件。一定是要在`_initialize()` 使用设置中间件方法 `middleware($middleware, $except, $only)`

代码示例：

```php
<?php
namespace app\modules\demo\controller;

use framework\util\Result;
use framework\core\AbstractController;
use framework\annotations\RequestMapping;
use framework\annotations\Controller;

/**
 * Class MiddlewareController
 * @package app\modules\demo\controller
 * @Controller(msg="")
 */
class MiddlewareController extends AbstractController
{
    public function _initialize()
    {
        //doSomething init
        //意思在这个控制器中， 所有的方法都有应用这个中间件
        $this->middleware(M3Middleware::class);
    }
}
```

### middleware() 参数解析

- 多个设置则传递数组
- `$middleware` 配置的中间件，当配置单个，可以直接类名`M3Middleware::class`, 配置多个则是用数组eg: `[M3Middleware::class, M4Middleware::class]`
- `$except` 排除某个/多个方法不应用`$middleware`参数的中间件，eg `['except' => 'test']`, 排除 `test`方法不应用中间件
- `$only` 仅仅 某个/多个 方法应用此中间件 eg: `['only' => 'test']`

> 注意： `$except` 和 `$only` 是排斥， 设置了`$except`就不用设置`$noly`, 同理相反 eg: `$this->middleware(M3Middleware::class, ['except' => ['test', 'test2']])` 或者 `$this->middleware(M4Middleware::class, [], ['only' => 'test3'])`

### 排除某个/多个方法不应用中间件

```php
//...snip
    public function _initialize()
    {
        //意思在这个控制器中， 所有的方法都有应用这个中间件
        $this->middleware(M3Middleware::class)
        //排除test2 方法不应用 M1Middleware 中
            ->middleware(M1Middleware::class, ['except' => 'test2']);
    }
//..snip
```

### 某个/多个方法只应用中间件

```php
//...snip
    public function _initialize()
    {
        //意思在这个控制器中， 所有的方法都有应用这个中间件
        $this->middleware(M3Middleware::class)
            //排除test2 方法不应用 M1Middleware 中间件
            ->middleware(M1Middleware::class, ['except' => 'test2']);
            //那么这个控制器， 只有 test2 方法会应用 M2Middleware 中间件， 其他方法都不会应用 M2Middleware 中间件
            ->middleware(M2Middleware::class, [], ['only' => 'test2']);
    }
//..snip
```

## 权限中间件示例

```php
<?php

namespace app\modules\demo\middleware;

use app\exception\AuthException;
use framework\http\Request;

/**
 * Class AuthMiddleware
 * @package app\modules\demo\middleware
 */
class AuthMiddleware
{
    /**
     * @param Request $request
     * @param array $vars
     * @param array $extVars
     * @param \Closure $next
     * @return mixed
     */
    public function handle(Request $request, array $vars, array $extVars, \Closure $next)
    {
        //验证登陆
        $session = $request->session();
        $data = $session->get('loginUser');
        if (empty($data)) {
            throw new AuthException();
        }
        return $next($request, $vars, $extVars);
    }
}
```

## 中间件执行顺序

先是配置文件中的->再是控制器中配置的局部中间件

重置className
