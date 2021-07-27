# 部署

## 启动

### 普通启动

    `php bin/start start`

### docker 容器启动

- 构建镜像 `sudo docker build -t heros-worker-demo:v1 .`
- 启动容器 `sudo docker run --name=heros-worker-demo --network=host -d heros-worker-demo:v1`

> 在启动时候， 确保数据库配置（config/database.php）和redis（config/redis.php）配置是正确的，可连接的 \
> 可直接 http:127.0.0.1:8080/直接访问， 也可以 配置nginx 反向代理

## nginx 反向代理

配置如下：

    ```conf
    server {
        listen 80;
        server_name www.demo.com;

        location / {
            proxy_pass http://127.0.0.1:8080/;
            #proxy_http_version 1.1;
            proxy_set_header X-Real-IP $remote_addr;
        }

        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        location ~ /\.ht {
            deny all;
        }
    }
    ```
