# Session

session 会话管理，存储用户数据，数据存储的驱动分为*redis*或者*文件*，在`cofing/session.php` 文件中配置, 推荐使用*redis*, 使用 `session` 对象进行操作，其命名空间是`framework\http\Session`

## 配置

配置位于 `config/session.php` 文件， eg:

```php
<?php

use Workerman\Protocols\Http\Session\FileSessionHandler;
use Workerman\Protocols\Http\Session\RedisSessionHandler;

return [
    'enable' => true,
    'handler' => FileSessionHandler::class,  //存储驱动
    'config' => [
        FileSessionHandler::class => [                      //文件存储
            'save_path' => runtime_path() . '/sessions',
        ],
        RedisSessionHandler::class => [                     //redis存储
            'host' => env('redis_host', '127.0.0.1'),
            'port' => env('redis_port', 6379),
            'auth' => env('redis_password', ''),
            'timeout' => 2,
            'database' => env('redis_db', 0),
            'prefix' => env('session_prefix', 'worker_demo_session_'),
        ],
    ],
    'session_name' => 'php_sid',                            //在cookie中的键值
];
```

## 获取`Session`对象

- 控制器方法中参数接收， eg：

    ```php
    //--snip--
    /**
     * @param Session $session
     * @return Result
     * @RequestMapping(value="/demo/session/set", method={"get"}, msg="test")
     */
    public function set(Session $session): Result
    {
       //--snip--
    }
    //--snip--
    ```

- 通过`Requset` `(framework\http\Request)`对象获取 eg:

    ```php
    //--snip--
    $session = $request->session();
    //--snip--
    ```

## 存储数据

- `set($name, $value)` eg:

    ```php
    //--snip--
    $session->set('name', 'tom')
    //--snip--
    ```

- 遍历数组存储数据数据， `put($key, $value = null)`, eg:

    ```php
    //--snip--
    $data = [
        'name' => 'tom',
        'loginTime' => date('Y-m-d H:i:s')
    ];
    $session->put($data);
    //--snip--
    //这个相当于 
    //$session->set('name', 'tom');
    //$session->set('loginTime', 'date('Y-m-d H:i:s')');
    ```

## 读取数据

- `get($name, $default = null)`, 没有返回默认值

    ```php
    //--snip--
    $name = $session->get('name')
    //--snip--
    ```

- 取出数据并删除 `pull($name, $default = null)`, 取出后便会从session数据中删除

    ```php
    //--snip--
    $name = $session->pull('name');
    //--snip--
    ```

- 取出全部数据 `all()`, 返回数组

    ```php
    //--snip--
    $all = $session->all();
    //--snip--
    ```

## 删除数据

- `delete($name)`, `forget($name)`, eg：

    ```php
    //--snip--
    $session->delete('name');
    //或者
    $session->forget('name');
    //--snip--
    ```

## 清空session数据

- `flush()` eg：

    ```php
    //--snip--
    $session->flush();
    //--snip--
    ```

## 判断数据

- `has($name)` 判断数据时候是否存在
- `exists($name)` 判断数据时候是否存在
