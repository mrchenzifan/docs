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

 如果觉得window下开发配置环境复杂，可以使用如下Dockerfile。
 
```shell
FROM php:7.2.34-cli-alpine
# timezone
RUN apk add --no-cache tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && apk del tzdata

# Packages
RUN apk --update --no-cache add \
    autoconf \
    libaio-dev \
    libzip \
    libzip-dev \
    zlib-dev \
    curl \
    freetype-dev \
    libjpeg-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libtool \
    libbz2 \
    bzip2 \
    bzip2-dev \
    libstdc++ \
    libxslt-dev \
    libevent-dev \
    make \
    unzip \
    wget \
    git \
    && docker-php-ext-install bcmath zip bz2 pdo_mysql mysqli simplexml opcache sockets mbstring pcntl xsl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-enable sockets \
    # redis
    && pecl install redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install gd \
    && rm -rf /var/cache/apk/*
```