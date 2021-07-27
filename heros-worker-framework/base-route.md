# 路由

框架路由使用的是 [fast-route](https://packagist.org/packages/nikic/fast-route)

## http路由配置

路由暂时没有对应的配置文件， 是通过 `@Controller` 和 `@RequestMapping` 两个注解设置
> tips: 路由一定要设置， 否则访问不到， 并且路由值需要是唯一的

### 注解定义路由

再控制器类上注解 `@Controller`, 接着在类方法上注解 `@RequestMapping`， 支持 `get|post|put|delete`

代码示例：

```php
<?php
namespace app\modules\demo\controller;

use framework\util\Result;
use framework\core\AbstractController;
use framework\annotations\RequestMapping;
use framework\annotations\Controller;

/**
 * Class RouteController
 * @package app\modules\demo\controller
 * @Controller(msg="")
 */
class RouteController extends AbstractController
{
    /**
     * @return Result
     * @RequestMapping(value="/demo/route/rGet[/{id}]", method={"get"}， msg="")
     */
    public function rGet($id)
    {
        return Result::ok()->data(['id' => $id]);
    }
}
```

*访问： `http://127.0.0.1:8080/demo/route/rGet/1`*

### @RequestMapping 注解参数

- msg: 备注信息
- value： 定义路由，这是必填的， 需要是唯一
- method: 请求方法 `get|post|put|delete`, 不设置默认是`get`请求， 设置允许多个 `method={"get", "post"}`

### 路由上参数匹配

- 必填参数： `/demo/route/rGet/{id}`
- 非必填： `/demo/route/rGet[/{id}]`， 如果不传默认值是`false`
- 正则匹配： `/demo/route/rGet/{id:\d+}`

## 异常

当路由不匹配时候会抛出`framework\exception\RouteNotFoundException`路由找不到异常，当请求方法不一致时候会抛出`framework\exception\RequestMethodException`请求方法错误异常。处理参考：[异常处理](heros-worker-framework/base-exception.md)
