# 帮助函数

框架内置的一些快捷函数

签名 | 描述
- | -
`config_path()` | 获取配置目录： `/var/www/heros-worker-demo/config`
`app_path()` | 获取配置目录： `/var/www/heros-worker-demo/app`
`runtime_path()` | 获取配置目录： `/var/www/heros-worker-demo/runtime`
`view($template, $vars = [])` | 渲染模板页面，详见 [视图](heros-worker-framework/base-view.md) 章节
`assign(string $name, $value)` | 给模板页面赋值
`config($key = null, $default = null)` | 获取配置值，多层数据使用`.`拼接
`print_line($msg)` | 打印一行并还行
`print_ok($message)` | 打印绿色信息
`print_error($message)` | 打印红色信息
`print_warning('warning');` | 打印黄色信息
`cpu_count()` | cpu使用数量
`container()` | 获取框架容器， 详见[容器](heros-worker-framework/core-container.md)章节
`load(string $clazz)` | 根据类名加载类， 详见[容器](heros-worker-framework/core-container.md)章节
`response($body = '', $status = 200, $headers = [])` | 返回`Response`
`packResult($result)` | 包装对象（实现`__toString()`）并转换为 `Response`
`redirect($location, $status = 302, $headers = [])` | 设置重定向，并返回 `Response`
`request()` | 返回`Request`对象(`framework\http\Request`)
