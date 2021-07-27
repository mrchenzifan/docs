# 测试环境

| 名称         | 值                                                           |
| ------------ | ------------------------------------------------------------ |
| 操作系统     | macOS 10.15.7                                                |
| Linux Kernel | 19.6.0                                                       |
| CPU          | 2.3 GHz 四核八线Intel Core i7                                |
| 内存         | ( DDR4 2666MHz 8GB ) x 2 = 16GB                              |
| 磁盘         | SSD                                                          |
| 运行环境     | Apache/2.4.46                                                |
| 数据库       | Mysql5.7                                                     |
| PHP 版本     | 7.2.6                                                        |
| PHP 插件     | bcmath  Core ctype curl date dom fileinfo filter ftp gd hash iconv json libxml mbstring mysqli mysqlnd openssl pcre PDO pdo_mysql pdo_sqlite Phar Posix redis Reflection session SimpleXML sockets SPL sqlite3 standard sysvmsg sysvsem sysvshm tokenizer xdebug xml xmlreader xmlwriter zlib |

# 测试框架及版本

1. Laravel 6.20.8

2. thinkphp 6.0.0

3. heros-worker 1.0.7


# 测试流程

使用 `httpd` 自带的 `ab` 工具进行测试，测试要点如下：

1. 所有站点以子目录形式放置在 `Nginx` 默认 `Document Root（/Applications/MxSrvs/www）`下，`Apache` 配置文件无任何修改。
2. 每次测试完成 `5000` 个请求，并发 1、5、20 各进行 2 次，取`Time per request: *[ms] (mean, across all concurrent requests)`的最小值为该并发下的成绩
3. `ab` 命令和 `Mysql` 都运行在这台服务器上。
4. 只对比请求耗时，不对比内存占用。

# Hello World

定义

> Hello World 测试：使用一个框架的正常流程，除了“不调用 Model”（不连接数据库）之外，使用正打印出 “Hello World！”几个字符串的测试。

# 测试结果

平均每次请求消耗的时间，单位为毫秒`(ms)`。

| 并发数 | laravel | thinkphp  | heros-work |
| ------ | ------- | -------- | ---------- |
| 1      | 81.544  | 24.550   | 0.252      |
| 5      | 29.685  | 7.539    | 0.094      |
| 20     | 33.188  | 6.880    | 0.061      |

# 简单api测试

> 为了测试框架性能，尽量减少数据库对性能的影响，我们新安装` Mysql`，在`test` 库中新增 `users` 表，一共有三行数据：

| id   | name |
| ---- | ---- |
| 1    | a    |
| 2    | b    |
| 3    | c    |

## 测试代码

laravel：

```php
public function getUserList()
{
  return json_encode(\App\User::all()->toArray());
 }
```

thinkphp:

```php
public function getUserList()
{
  return (new \app\model\User)->select();
}
```

Heros-worker:

```php
 public function getUserList()
 {
   return User::query()->get()->toArray();
 }
```

# 测试结果

平均每次请求消耗的时间，单位为毫秒`(ms)`。

| 并发数 | laravel | thinkphp  | heros-worker |
| ------ | ------- | -------- | -------- |
| 1      | 126.923 | 33.965   | 12.264 |
| 5      | 49.230  | 11.972   | 3.101 |
| 20     | 47.548  | 10.910   | 1.381 |

