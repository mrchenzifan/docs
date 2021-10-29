# 安装

## 服务器要求

一般是运行在linux\mac上,

对于php扩展需要安装

- pcntl
- posix
- redis
- pdo
- ...

## composer 安装

### 创建项目

- `composer create-project mondagroup/heros-worker-demo`

### 启动项目

- `cd heros-worker-demo`
- `php bin/start start`

### Dockerfile

 如果觉得window下开发配置环境复杂，可以使用docker安装。
 
```shell
docker run -p 8080:8080 -v ${PWD}/:/var -it  church1117/heros-framework /bin/sh
cd /var
php bin/start start
```

访问 http://127.0.0.1:8080