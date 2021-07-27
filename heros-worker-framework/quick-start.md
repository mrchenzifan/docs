# 快速开始

本章节讲述如何快速通过路由访问服务

## http服务

框架内置Http服务,配置主要是在 *config/server.php*

可以在环境配置文件中配置相关参数， eg: env_dev/test/prod 文件中配置

```env
#server
server_listen=http://127.0.0.1:8080
server_process_count=1
server_max_request=10000
```

- `server_listen` 服务监听地址和端口
- `server_process_count` 启动进程数
- `server_max_request` 每个进程处理最大请求， 超过就会重启

## 定义路由

路由一般是定义在控制器上， 并且使用*注解*来完成

### 通过 @Controller, @RequestMapping 注解

`@Controller` 表明这是个控制器， 再配合`@RequestMapping` 注解定义路由， 两者结合才能一起使用，需要注意使用注解时候需要引用对应的命名空间

-注解-|-命名空间-
-|-
@Controller|use framework\annotations\Controller;
@RequestMapping|use framework\annotations\RequestMapping;

> tips: 可以使用**make:controller**命令快速创建文件 eg : `php artisan make:controller -path=app/modules/demo/controller -class=DemoController`， 命令详见 [make:controller](heros-worker-framework/command.md?id=controller) 部分

生成文件如下：

```php
<?php
namespace app\modules\demo\controller;

use framework\util\Result;
use framework\core\AbstractController;
use framework\annotations\RequestMapping;
use framework\annotations\Controller;

/**
 * Class DemoController
 * @package app\modules\demo\controller
 * @Controller(msg="")
 */
class DemoController extends AbstractController
{
    public function _initialize()
    {
        //doSomething init
        //such as 配置一些局部中间件
        //$this->middleware()
    }

    /**
     * @return Result
     * @RequestMapping(value="/demo/demo/test", method={"get"}, msg="test")
     */
    public function test(): Result
    {
        return Result::ok();
    }
}

```

> 在 `@RequestMapping` 注解中必须定义`value` 值， 而且需要是唯一。

访问 ： 直接就是定义的 `value` 的值， http://127.0.0.1:8081/demo/demo/test

## 处理http请求

通过 `Request` 接收参数

```php
<?php
namespace app\modules\demo\controller;

use framework\http\Request;
use framework\util\Result;
use framework\core\AbstractController;
use framework\annotations\RequestMapping;
use framework\annotations\Controller;

/**
 * Class DemoController
 * @package app\modules\demo\controller
 * @Controller(msg="")
 */
class DemoController extends AbstractController
{
    public function _initialize()
    {
        //doSomething init
        //such as 配置一些局部中间件
        //$this->middleware()
    }

    /**
     * @param Request $request
     * @return Result
     * @RequestMapping(value="/demo/demo/test", method={"get"}, msg="test")
     */
    public function test(Request $request): Result
    {
        $name = $request->getParameter('name');
        return Result::ok()->data(['name' => $name]);
    }
}
```

先重启服务 `php bin/start restart` 让代码生效

测试： http://127.0.0.1:8081/demo/demo/test?name=tom

浏览器会输出： `{"code":"000","success":true,"message":"操作成功","data":{"name":"tom"}}`

## 服务相关

- 启动 `php bin/start start [-d]` -d 是后台运行
- 重启 `php bin/start restart`
- 关闭 `php bin/start stop`
- 查看状态 `php bin/start status`

## 代码更新

每次修改代码，都需要重启服务
