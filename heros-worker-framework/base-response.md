# 响应

响应请求结束，返回给客户端内容，响应头`Content-Type`默认是`application/json;charset=utf-8`, 对于响应内容，可以自定义多种方式， 支持普通类型，对象自定义对象需要实现`__toString()`方法来自定义输出，这些都会转化成字符串设置到 `Response` *body* 中

- eg:

    ```php
    <?php
    namespace app\modules\demo\controller;

    use framework\http\Response;
    use framework\util\Result;
    use framework\core\AbstractController;
    use framework\annotations\RequestMapping;
    use framework\annotations\Controller;

    /**
    * Class ResController
    * @package app\modules\demo\controller
    * @Controller(msg="")
    */
    class ResController extends AbstractController
    {
       
        /**
        * @return miexd
        * @RequestMapping(value="/demo/res/res1/{type}")
        */
        public function res1($type, Response $response)
        {
            switch ($type) {
                case 'bool':
                    return true;
                case 'int':
                    return 2;
                case 'string':
                    return "hello";
                case 'array':
                    return ['hello'];
                case 'object':
                    return new Res();
                case 'view':
                    return view('demo/res');
                case 'response':
                    return $response->body('response body');
                default:
                    return Result::ok();
            }
        }
    }

    class Res
    {
        public function __toString()
        {
            return 'Res object';
        }
    }
    ```

## 返回`Result`类

框架内置了`Result`类作为返回结果，定义了格式化，统一的格式； 推荐使用此类作为返回结果, 使用方法详见 [Result类](heros-worker-framework/utils-result.md) 章节

## 返回`Response`类

当有些不满足需求，可以控制器接收`Response`参数（`framework\http\Response`)，更加细致的制定返回结果

### 返回不同的响应码

使用 `status(int $code)` 方法返回不同状态码， eg: 500 `$response->status(500)`

### 添加响应头

使用 `header($name, $value)`; eg: `$response->header('cus', 'tom')`

### 重定向

- 通过`Response::redirect` 方法

    ```php
    //--snip--
    /**
     * @RequestMapping(value="/demo/res/redirect", method={"get"}, msg="test")
     */
    public function redirect(Response $response)
    {
        return $response->redirect('http://docs.monda.inet');
    }
    //--snip--
    ```

- 通过帮助函数 `redirect()`, 返回的是`Response`对象

    ```php
    //--snip--
    /**
     * @RequestMapping(value="/demo/res/redi2", method={"get"}, msg="test")
     */
    public function redirect2()
    {
        return redirect('http://www.baidu.com');
    }
    //--snip--
    ```

### 设置`cookie`

通过 `Response::cookie` 方法, eg: `$response->cookie('name', 'tom')`

### 设置`body`

通过 `Response::body` 方法， eg: `$response->body('body')`

### 文件下载

通过 `Response::download` 方法， eg：

```php
//--snip--
    /**
     * @param Response $response
     * @return Response
     * @RequestMapping(value="/demo/res/download", method={"get"}, msg="test")
     */
    public function download(Response  $response)
    {
        return $response->download(runtime_path() . '/upload/test.jpeg', 'test.jpeg');
    }
//--snip--
```
