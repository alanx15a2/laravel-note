# docker 

## Laravel 效能

在 windows 下由於 filesystem 的因素，透過 volume 讀取專案會嚴重托慢速度
以下有兩種解法
1. 利用將 vendor 安裝在 container 內來加速反應速度

* RUN COMPOSER_VENDOR_DIR="/srv/vendor" composer install

`public/index.php` and `artisan` 中
include 置換為 `require '/srv/vendor/autoload.php';`  

2. 掛載 volumes 的時候將 vendor 獨立設定

建立一個 docker-entrypoint.sh 並在 Build image 時候 Copy 進去  
COPY ./docker-entrypoint.sh /docker-entrypoint.sh
``` sh
composer install && php-fpm
```
docker compose 在 service 新增以下參數
``` dockerfile
  service:
    .
    .
    .
    volumes:
      - ${APP_LOCATION}:/var/www/html
      - '/var/www/html/vendor'
    command: sh -c /docker-entrypoint.sh
```

## 安裝 composer 

1. COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
2. RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
