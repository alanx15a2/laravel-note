## /etc/nginx/nginx.conf

```
http {
    ssl_protocols TLSv1.2; # 限制使用 TLS 1.2 版
    ...
}
```

## /etc/nginx/sites-available/www.conf

```
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri; ## 第一次 http 會自動轉到 https
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name example.com;
    root /srv/example.com/public;
 
    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";
    add_header Strict-Transport-Security "max-age=10886400; includeSubDomains"; # 啟動 HSTS
 
    index index.php;
 
    charset utf-8;
 
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
 
    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }
 
    error_page 404 /index.php;
 
    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php8.0-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }
 
    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

## gzip

```
gzip on;

## compression level (1-9) ##
## 4 is a good compromise between CPU usage and file size. ##
gzip_comp_level 1;

## minimum file size limit in bytes, to low can have negative impact. ##
gzip_min_length 200;

## compress data for clients connecting via proxies ##
gzip_proxied any;

## disables GZIP compression for ancient browsers that don't support it. ##
gzip_disable "msie6";

## compress outputs labeled with the following MIME-types. ##
## do not add text/html as this is enabled by default. ##
gzip_types
    application/json
    application/javascript
    application/xml
    text/css
    text/javascript
    text/plain
    text/xml;
```
