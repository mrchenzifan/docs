# 生命周期

## 简介

以便开发，首先了解框架运行原理是必不可少的，这里将简要讲解下框架的工作流程

## 启动文件

启动文件在 *app/bin* 目录下， 一个是 *define.php*, 主要是定义一些系统常量， 加载配置（ *app/config* 目录下配置文件）， aop 切面代理文件生成； 接着就是 *start* 启动文件， 这里就是主要进程的初始化， 比如 `HttpServer` 进程， 以及 *app/config/process.php* 文件中定义的自定义 `Worker` 进程， 最后就是启动 `Worker::runAll()`

## `HttpServer` Worker进程

该进程主要处理http请求，作为 *web* 服务， 其类是 `framework\server\HttpServer`\

在 `onWorkerStart()` 中， 主要是初始化应用， 像框架中 *bootstrap* 目录下各项配置就是在这时候启动； 接着就是扫描项目目录， 对项目文件中注解进行收集， 像 *@Controller - 控制器 | @Resourse - 资源 | @RequestMapping - 路由 | ...*, 其中路由收集会把 *控制器方法* 包装成闭包， 和路由一一对应， 中间件也是在 *包装成闭包* 步骤加载在闭包中\

接着就是处理请求和返回响应， 在 `onMessage()` 中， 会根据 `Reuqest` 中 *请求方法，路径*， 从收集好的路由中匹配， 如果匹配上， 就会运行对应的闭包处理， 并传递 `Resquest|Response|Session|vo|请求参数` 各项参数； 匹配不上， 则会判断是否是静态文件（这里静态文件是 *app/public* 目录下的文件）， 如果是则返回静态资源， 不是则抛出异常， 最后就响应客户端\
