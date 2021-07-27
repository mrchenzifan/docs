# 项目目录规范

建议按以下目录存放代码， 根据`heros-worker-demo`为示例

## / 根目录下

```txt
/-
  |-app   //项目主要代码
  |-bin   //启动文件
  |-config      //项目配置文件
  |-docs        //项目一些文档， 比如sql文件
  |-public      //静态资源
  |-runtime     //运行时目录
  |-script      //存放脚本文件
  |-vender      //第三方包文件
  |-view        //模板文件
  .env          //环境配置
  composer.json
  env_dev
  env_test
  env_prod
  artisan       //artisan 命令行脚本
```

## app 目录

```txt
app/-
    |-aspect        //aop切面文件
    |-command       //自定义 artisan command 命令
    |-cron          //定时任务
    |-entity        //公共 orm 实体
    |-exception     //项目异常类
    |-middleware    //全局中间件
    |-modules       //模块， 存放不同模块
    |-process       //自定义进程
    |-queue         //队列消费者
    functions.php   //公共函数
```

## modules 模块

```txt
modules/-                   //每个模块都单独，互不影响
    |-demo                  //demo模块
        |-controller        //控制器-调度服务
        |-dao               //dao层-主要处理增删改查等，不处理业务
        |-middleware        //模块中间件
        |-service           //服务-处理业务，调度dao层
        |-validator         //表单验证器
        |-vo                //表单vo, getter-setter
    |-admin                 //其他模块
    ...
```
