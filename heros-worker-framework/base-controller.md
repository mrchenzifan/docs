# 控制器

控制器主要是负责接收http请求，调度服务

## 代码示例

> 命令快速生成控制器： `php artisan make:controller -path=app/modules/demo/controller -class=TestController`； 运行后会在 app/modules/demo/controller 这个目录下生成`TestController.php`文件, 命令详见 [make:controller](heros-worker-framework/command.md?id=controller) 部分

```php
<?php
namespace app\modules\demo\controller;

use framework\util\Result;
use framework\core\AbstractController;
use framework\annotations\RequestMapping;
use framework\annotations\Controller;

/**
 * Class TestController
 * @package app\modules\demo\controller
 * @Controller(msg="")
 */
class TestController extends AbstractController
{
    public function _initialize()
    {
        //doSomething init
        //such as 配置一些局部中间件
        //$this->middleware()
    }

    /**
     * @return Result
     * @RequestMapping(value="/demo/test/test", method={"get"}, msg="test")
     */
    public function test(): Result
    {
        return Result::ok();
    }
}
```

访问 `http://127.0.0.1:8080/demo/test/test` 就会正常响应

```json
{
    "code": "000",
    "success": true,
    "message": "操作成功"
}
```

### _initialize() 初始化工作

在这个方法可以初始化一些自己的，比如配置局部[中间件](heros-worker-framework/base-middleware.md)以满足不同方法的应用不同的中间件

### 请求参数

对于控制器中的方法可以接收以下这些参数

#### 接收`Request`请求

具体`Request`请参考[请求](heros-worker-framework/base-request.md)

- eg：

    ```php
        <?php
        namespace app\modules\demo\controller;

        use framework\http\Request;
        use framework\util\Result;
        use framework\core\AbstractController;
        use framework\annotations\RequestMapping;
        use framework\annotations\Controller;

        /**
        * Class TestController
        * @package app\modules\demo\controller
        * @Controller(msg="")
        */
        class TestController extends AbstractController
        {

            /**
            * @param Request $request
            * @return Result
            * @RequestMapping(value="/demo/test/test", method={"get"}, msg="test")
            */
            public function test(Request $request): Result
            {
                return Result::ok()->data(['id' => $request->getParameter('id', '')]);
            }
        }
    ```

- 响应结果

    ```json
    {"code":"000","success":true,"message":"操作成功","data":{"id":"1"}}
    ```

#### 接收`Response`响应

`Response` 类使用具体参考[响应](heros-worker-framework/base-response.md)

- eg:

    ```php
    <?php
    //--snip--
    use framework\http\Response;
    //--snip--

        /**
        * @param Request $request
        * @param Response $response
        * @return Response
        * @RequestMapping(value="/demo/test/test", method={"get"}, msg="test")
        */
        public function test(Request $request, Response $response): Response
        {
            $response->body(['id' => $request->getParameter('id', '')]);
            return $response;
        }

    ```

- 响应结果

    ```json
    {"id":"1"}
    ```

#### Session会话

接收`Session`对象， 进行会话操作， 具体使用参考[session](heros-worker-framework/base-session.md)

- eg:

    ```php
    //--snip--
    use framework\http\Session;
    //--snip--

        /**
        * @param Session $session
        * @return mixed
        * @RequestMapping(value="/demo/test/testSession", method={"get"}, msg="test")
        */
        public function testSession(Session $session)
        {
            $session->set('name', 'tom');
            return $session->get('name');
        }
    //--snip--
    ```

- 响应结果

    ```txt
    tom
    ```

### 普通参数

接收路由上参数或者http请求参数

- eg:

    ```php
    //--snip--
        /**
        * @param $id
        * @param $name
        * @return Result
        * @RequestMapping(value="/demo/test/test2/{id}", method={"get"}, msg="test")
        */
        public function test2($id, $name): Result
        {
            return Result::ok()
                ->data([
                    'id' => $id,
                    'name' => $name
                ]);
        }
    //--snip--
    ```

- `http://127.0.0.1:8080/demo/test/test2/2?name=tom`响应结果

    ```json
    {"code":"000","success":true,"message":"操作成功","data":{"id":"2","name":"tom"}}
    ```

#### vo表单对象

接收`vo`表单对象，对象理的属性值和http请求参数/路由参数对应，框架会自动吧参数赋`vo`对象中属性，详见[vo](heros-worker-framework/utils-vo.md)

- eg:

    ```php
    //--snip--
    use app\modules\demo\vo\TestUserAddVo;
    //--snip--
        /**
        * @param TestUserAddVo $vo
        * @return Result
        * @RequestMapping(value="/demo/test/test3", method={"get"}, msg="test")
        */
        public function test3(TestUserAddVo $vo): Result
        {
            return Result::ok()->data([
                'name' => $vo->getName(),
            ]);
        }
    //--snip--
    ```

- `http://127.0.0.1:8080/demo/test/test3?name=tom`响应结果

    ```json
    {"code":"000","success":true,"message":"操作成功","data":{"name":"tom"}}
    ```

#### TcpConnect连接

接收当前连接对象， 基本是用不到， 除非有特殊需求， 具体可以参考 wokerman [TcpConnection类使用]http://doc.workerman.net/tcp-connection.html

- eg:

    ```php
    //--snip--
    use Workerman\Connection\TcpConnection;
    //--snip--

        /**
        * @param TcpConnection $connection
        * @return mixed
        * @RequestMapping(value="/demo/test/testConnect", method={"get"}, msg="test")
        */
        public function testConnect(TcpConnection $connection)
        {
            return $connection->getRemoteIp();
        }
    //--snip--
    ```

- 响应结果

    ```txt
    127.0.0.1
    ```
