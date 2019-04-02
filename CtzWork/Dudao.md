#项目描述

#技术栈
    * angularjs
    * sass
    * compass
    * bootsrap
    * jquery

#项目结构
    前端开发是放在pop-dudu-service/src/main/webapp路径下
    ├── common                    // 公共文件 
    │   ├── css                   // css文件包
    │   ├── file                  // 调查文件模板
    │   ├── font                  // iconfont
    │   ├── gif                   // gif图
    │   ├── img                   // 图片
    │   ├── js                    // js文件
    │   ├── svg                   // svg图片      
    ├── module                    // 各模块目录
    │   ├── workBench             // 工作台
    │   ├── company               // 信息披露
    │   ├── manuscript            // 底稿
    │   ├── daily                 // 日常业务
    │   ├── monhome               // 信息交流
    │   ├── message               // 消息中心
    │   ├── TrainingGarden        // 培训园地
    │   ├── reference             // 资料库
    │   ├── companyinfo           // 公司信息
    │   ├──tigerreport            // 软件下载
    ├── static                    // sass文件根目录
    │   ├──style                  // sass文件目录，编译建议下载compass，自动监听
    ├── index.html                // 文件总入口
    └── login.html                // 登录页面

#nginx代理配置
```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       8088;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            #proxy_pass http://tf.dudao360.com:8081;
            proxy_pass https://ceshi.dudao360.com;
            #proxy_pass http://114.215.28.157:8081;
            #proxy_pass https://gz.dudao360.com;
            #proxy_pass http://tf.ceshi.dudao360.com:8081;
            #proxy_pass https://fenzhi.dudao360.com;
            #proxy_pass https://htdd.htsec.com;
            #proxy_pass https://test.dudao360.com;
            #proxy_pass https://tpy.dudao360.com;
            #proxy_pass https://gj.dudao360.com;
            #proxy_pass https://xsb.ctsec.com;
        }

        location /login.html {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/login.html;
        }
        location /index.html {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/index.html;
        }
        location /lib {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/lib;
        }
        location /static {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/static;
        }
        location /module {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/module;
        }
        location /download {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/download;
        }
        location /common {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/common;
        }
        location /duduphone {
            alias F:/caizhengtong/gitDudao/dudao360/pop-dudu-service/src/main/webapp/duduphone;
        }
        location  /find {
            proxy_pass http://192.168.10.135:8080/find;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

}

```
    