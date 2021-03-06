# Nginx配置记录

---

安装  
yum install nginx  
systemctl enable nginx.service  
systemctl status nginx.service  
systemctl restart nginx.service

```
http {
    include       mime.types;
    include /etc/tengine/conf.d/*.conf;
    default_type  application/octet-stream;
     client_max_body_size 20m;
}
```

/etc/nginx/nginx.conf

配置

纯粹的文件访问  
    server{  
        listen 80;  
        server\_name webfiles.zeekstar.cn;  
        root /www/webfiles;  
        location /{  
            root /www/webfiles;  
        }  
    }  
HTTPSser  
server {  
    server\_name YOUR\_DOMAINNAME\_HERE;  
    listen 443;  
    ssl on;  
    ssl\_certificate /usr/local/nginx/conf/server.crt;  
    ssl\_certificate\_key /usr/local/nginx/conf/server.key;  
}  
另外还可以加入如下代码实现80端口重定向到443IT人乐园  
server {  
    listen 80;  
    server\_name ww.centos.bz;  
    rewrite ^\(.\*\) [https://$server\_name$1](https://$server_name$1) permanent;  
}

反向代理

# 负责压缩数据流

gzip              on;  
gzip\_min\_length   1000;  
gzip\_types        text/plain text/css application/x-javascript;

# 设定负载均衡的服务器列表

# weigth参数表示权值，权值越高被分配到的几率越大

upstream hello{  
    server 192.168.68.43:8080 weight=1;  
    server 192.168.68.45:8080 weight=1;  
}

server {

```
#侦听的80端口:
listen       80;
server_name  localhost;
#设定查看Nginx状态的地址
location /nginxstatus{
     stub_status on;
     access_log on;
     auth_basic "nginxstatus";
     auth_basic_user_file htpasswd;
}
#匹配以jsp结尾的，tomcat的网页文件是以jsp结尾
location / {
    index index.jsp;
    proxy_pass   http://hello;    #在这里设置一个代理，和upstream的名字一样
    #以下是一些反向代理的配置可删除
    proxy_redirect             off; 
    #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
    proxy_set_header           Host $host; 
    proxy_set_header           X-Real-IP $remote_addr; 
    proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for; 
    client_max_body_size       10m; #允许客户端请求的最大单文件字节数
    client_body_buffer_size    128k; #缓冲区代理缓冲用户端请求的最大字节数
    proxy_connect_timeout      300; #nginx跟后端服务器连接超时时间(代理连接超时)
    proxy_send_timeout         300; #后端服务器数据回传时间(代理发送超时)
    proxy_read_timeout         300; #连接成功后，后端服务器响应时间(代理接收超时)
    proxy_buffer_size          4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
    proxy_buffers              4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
    proxy_busy_buffers_size    64k; #高负荷下缓冲大小（proxy_buffers*2）
    proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传
}
```

}

server{  
    listen 80;  
    server\_name ~^webmanager.zeekstar.\(cn\|com\)$;  
    index index.html;  
    root /www/webmanager;  
    location /{  
        root /www/webmanager;  
    }  
    location /webmanager{  
        proxy\_pass [http://local\_tomcat/webmanager](http://local_tomcat/webmanager);  
        proxy\_redirect             off;  
        proxy\_set\_header           Host $host;  
            proxy\_set\_header           X-Real-IP $remote\_addr;  
            proxy\_set\_header           X-Forwarded-For $proxy\_add\_x\_forwarded\_for;  
    }  
}

upstream local\_tomcat{

```
server localhost:8080 weight=1;
```

}

server{  
    listen 80;  
    server\_name ~^\(webserver\|webtest\).zeekstar.\(cn\|com\)$;

```
location /{
    proxy_pass http://local_tomcat;
    proxy_redirect             off; 
    proxy_set_header           Host $host; 
        proxy_set_header           X-Real-IP $remote_addr; 
        proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for; 
}
```

}

使用Web登录验证  
    server{  
        listen 80;  
        server\_name 127.0.0.1;  
        index index.html;  
        root D:/www/;  
        location /{  
            root D:/www/;  
            auth\_basic "liuyixintest";  
            auth\_basic\_user\_file D:/www/pwd.db;  
        }  
    }



Https证书

```

Https证书
server{
	listen 443;
	server_name www.xxx.com;
	ssl on;
	ssl_certificate /www/https_cert/213980415590699.pem;
	ssl_certificate_key /www/213980415590699.key;
	ssl_session_timeout 5m;
	ssl_protocols SSLv2 SSLv3 TLSv1;
	ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv2:+EXP;
	ssl_prefer_server_ciphers on;
	location / {
		proxy_pass http://local_tomcat;
		proxy_set_header           Host $host; 
	        proxy_set_header           X-Real-IP $remote_addr; 
        	proxy_set_header           X-Forwarded-For $proxy_add_x_forwarded_for; 
	} 

}

```



