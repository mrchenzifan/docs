# 日志

日志采用是 *monolog* 记录， 详情可以了解 *monolog* 扩展

## 配置

在 `config/log.php` 文件中配置， eg:

```php
<?php
return [
    'default' => [
        'handlers' => [
            [
                'class' => Monolog\Handler\RotatingFileHandler::class,
                'constructor' => [
                    //日志记录目录
                    runtime_path() . '/logs/worker.log', 
                    //日志文件数量限制， 0 不限制
                    0, 
                    //日志记录级别
                    Monolog\Logger::DEBUG,      
                ],
                'formatter' => [
                    'class' => Monolog\Formatter\LineFormatter::class,
                    'constructor' => [null, 'Y-m-d H:i:s', false],
                ],
            ],
        ],
    ],
];
```

## 日志记录

通过使用 `ramework\bootstrap\Log` 类记录日志

- `Log::debug($message, array $context = [])`
- `Log::info($message, array $context = [])`
- `Log::notice($message, array $context = [])`
- `Log::warning($message, array $context = [])`
- `Log::error($message, array $context = [])`
- `Log::critical($messag, array $context = [])`
- `Log::alert($message, array $context = [])`
- `Log::emergency($message, array $context = [])`
