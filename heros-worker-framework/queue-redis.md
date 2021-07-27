# redis 队列

rides 队列是基于 *redis* 的 *list (列表)* *sorted set(有序集合)* 的性质

## 配置

首先需要在配置 `redis_queue.php` 和 `config/process.php` 两个配置文件中配置

- `redis_queue.php` code eg:

    ```php
    <?php

    return [
        'default' => [
            'host' => 'redis://' . env('redis_host') . ':' . env('redis_port', 6379),
            'options' => [
                'auth' => env('redis_password'),     // 密码，可选参数
                'db' => env('redis_db'),      // 数据库
                'max_attempts' => 10, // 消费失败后，重试次数
                'retry_seconds' => 5, // 重试间隔，单位秒
            ]
        ]
    ];
    ```

- `process.php`, 定义队列消费进程，code eg:

    ```php
    <?php
    use framework\queue\redis\proccess\Consumer;

    return [
        //--snip--
        'redis_consumer' => [
            'enable' => true,
            'handler' => Consumer::class,
            'count' => 1, // 可以设置多进程
            'constructor' => [
                // 消费者类目录
                'consumer_dir' => app_path() . '/queue'
            ]
        ]
    ];
    ```

## 消费者

在消费者类目录下面定义消费者类， 类需要实现 `framework\queue\redis\Consumer` 类, eg:

```php
<?php

namespace app\queue;

use framework\bootstrap\Log;
use framework\queue\redis\Consumer;

class DemoQueue implements Consumer
{
    //要消费的队列名
    public $queue = 'demo';

    // 连接名，对应 config/redis_queue.php 里的连接`
    public $connection = 'default';

    public function consume($data)
    {
        Log::debug('demo queue receive: ' . $data);
    }
}
```

## 生产者

使用 `framework\queue\redis\Client::send($queue, $messge)` 发送信息, eg:

```php
    //--snip--
    Client::sent('demo','tomcat');
    //--snip--
```

- `$queue` 是队列名称， 也就是消费者类的 *$queue* 属性
- `$message` 是队列消息， 需要是字符串

会发现日志文件多出一条日志：

```log
//--snip--
[2021-06-16 16:07:14] default.DEBUG: demo queue receive: tomcat [] []
//--snip--
```
