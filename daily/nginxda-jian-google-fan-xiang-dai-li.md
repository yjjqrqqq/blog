 

# Nginx搭建Google反向代理

[https://search.liuyx.net](https://search.liuyx.net)

---



参考：[http://www.ttlsa.com/nginx/nginx-google-ngx\_http\_google\_filter\_module/](http://www.ttlsa.com/nginx/nginx-google-ngx_http_google_filter_module/)

## 下载

NGINX源码：[http://nginx.org/download/](http://nginx.org/download/)

[http://nginx.org/download/nginx-1.13.10.tar.gz](http://nginx.org/download/nginx-1.13.10.tar.gz)

git clone [https://github.com/cuber/ngx\_http\_google\_filter\_module](https://github.com/cuber/ngx_http_google_filter_module)

git clone [https://github.com/yaoweibin/ngx\_http\_substitutions\_filter\_module](https://github.com/yaoweibin/ngx_http_substitutions_filter_module)

wget [http://mirrors.linuxeye.com/oneinstack/src/pcre-8.39.tar.gz](http://mirrors.linuxeye.com/oneinstack/src/pcre-8.39.tar.gz)

wget [http://mirrors.linuxeye.com/oneinstack/src/openssl-1.0.2j.tar.gz](http://mirrors.linuxeye.com/oneinstack/src/openssl-1.0.2j.tar.gz)

\# [http://www.zlib.net/](http://www.zlib.net/)

wget [http://zlib.net/fossils/zlib-1.2.11.tar.gz](http://zlib.net/fossils/zlib-1.2.11.tar.gz)

## make配置

./configure \

--prefix=/opt/nginx\

--with-pcre=../pcre-8.39 \

--with-openssl=../openssl-1.0.2j \

--with-zlib=../zlib-1.2.11 \

--with-http\_ssl\_module \

--add-module=../ngx\_http\_google\_filter\_module \

--add-module=../ngx\_http\_substitutions\_filter\_module

## 编译

make

make install

yum -y install gcc-c++

## nginx 配置

server{

```
server\_name search.liuyx.net;

resolver 8.8.8.8;

listen 443;

ssl on;

ssl\_certificate  /www/ssl/214765631580166.pem;

ssl\_certificate\_key /www/ssl/214765631580166.key;

location /{

    google on;

    google\_language zh-CN; 
```

\#        proxy\_pass [https://www.google.com](https://www.google.com);

```
}
```

}

server{

```
    server\_name search.liuyx.net;

listen 80;

location / {

    root /www/google;        

}

error\_page 404 https://search.liuyx.net;
```

}



**注：http强制以https访问**

**/www/google/index.html内容为：**

---

&lt;html&gt;  

&lt;meta http-equiv="refresh" content="0;url=https://search.liuyx.net"&gt;  

&lt;/html&gt; 

