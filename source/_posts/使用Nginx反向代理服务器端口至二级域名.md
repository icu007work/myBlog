---
title: 使用Nginx反向代理服务器端口至二级域名
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://api.ixiaowai.cn/mcapi/mcapi.php'
cover: https://pic.ziyuan.wang/2023/08/22/376f855a0dfc5.jpg
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
abbrlink: 13767a3a
date: 2023-04-12 18:59:50
authorAbout:
authorDesc:
tags:
keywords:
description:
password:
message:
---

要将 Nginx 配置为反向代理到另一个端口，需要进行以下步骤：

## 一、安装 Nginx

如果还没有安装 Nginx，可以使用以下命令在 Ubuntu 上进行安装：

```
 复制代码sudo apt update
 sudo apt install nginx
```

## 二、配置 Nginx 反向代理

### 2.1 不使用 HTTPS 

- 例如需要反代我们服务器的8080端口且不适用HTTPS，可以进行以下操作。
- 在 Nginx 的配置文件(一般为Nginx安装目录下的 `nginx.conf` )中添加以下内容，将 HTTP 请求代理到服务器的 8080 端口。需要将 `example.com` 替换为域名或 IP 地址，`/` 后面的路径应该是需要代理的应用程序的路径。

```
 server {
   listen 80;
   server_name example.com; //需要更改为你的域名
 
   location / {
     proxy_pass http://localhost:8080/; //需要更改为要反代的端口
     proxy_set_header Host $host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   }
 }
```

### 2.2 使用 HTTPS

- 使用HTTPS的情况下，需要先在服务器上安装SSL证书，可以通过Let's Encrypt等服务获取免费的SSL证书。然后，可以按照以下步骤进行反向代理：
- 编辑Nginx配置文件`nginx.conf`，添加以下内容：

```
 server {
     listen 443 ssl;
     server_name your_domain_name.com;               #需要更改
 
     ssl_certificate /path/to/your/cert.pem;         #需要更改
     ssl_certificate_key /path/to/your/key.key;      #需要更改
 
     location / {
       proxy_pass http://my_app;                     #需要更改
     }
 }
 
 # 或者以下命令
 server 
     {
         listen       80;
         listen       443;
         server_name  your_domain_name.com;          #需要更改
         ssl on;
         ssl_certificate /path/to/your/cert.pem;
         ssl_certificate_key /path/to/your/key.key;
         ssl_session_timeout 5m;
         ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;  
         ssl_protocols TLSv1 TLSv1.1 TLSv1.2;  
         ssl_prefer_server_ciphers on;
         if ($host ~* ^\d+\.\d+\.\d+\.\d+$) { 
            return 444; # 如果请求的是 IP 地址 返回空响应包，禁止访问
            }
         location / {
                 client_max_body_size 500M;
 
                 #proxy_redirect off;
                 proxy_set_header Host $proxy_host;
                 proxy_set_header X-Real-IP $remote_addr;
                 proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                 proxy_pass http://localhost:8080;
         }
     }
 
```

- 其中，`your_server_port`是你的服务器应用端口号，`your_domain_name.com`是你的域名，`/path/to/your/cert.pem`和`/path/to/your/key.key`分别是你的SSL证书和私钥文件的路径。该配置将来自NGINX监听的80端口的请求，重定向到443端口，并使用SSL证书对HTTPS请求进行加密。

## 三、重新加载 Nginx 配置

- 执行以下命令以重新加载 Nginx 配置：

```
 sudo nginx -t && sudo service nginx reload
 // 或者执行以下命令重启 nginx
 sudo systemctl restart nginx
```

- 现在，Nginx 将代理到服务器的 8080 端口。在浏览器中反代过的域名或 IP 地址，应该就能够访问应用程序了。
