# 注意事項

## 資料夾權限

設定完後須重啟 php-fpm

### 以 Webserver 為擁有者 (官方推薦模式)

sudo chown -R www-data:www-data /path/to/your/laravel/root/directory

// 將 ubuntu 使用者加入 www-data ， 以避免 FTP 問題
sudo usermod -a -G www-data ubuntu

// 資料夾設為 755，檔案設為 644
sudo find /path/to/your/laravel/root/directory -type f -exec chmod 644 {} \;
sudo find /path/to/your/laravel/root/directory -type d -exec chmod 755 {} \;

### 以一般 user 為擁有者

cd /path/to/your/laravel/root/directory
sudo chown -R $USER:www-data .
sudo find . -type f -exec chmod 664 {} \;
sudo find . -type d -exec chmod 775 {} \;
sudo chgrp -R www-data storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache

### LOG 權限

```
'daily' => [
   'driver' => 'daily',
   'path' => storage_path('logs/laravel.log'),
   'level' => 'debug',
   'days' => 14,
   'permission' => 0666,
],
```

---

## 更新版本

### 一般伺服器

* cd /path/to/laravel/root
* php artisan optimize:clear
* php artisan opcache:clear
* git checkout develop
* git reset --hard origin/develop
* git clean -df
* git pull
* composer install --no-dev
* php artisan migrate --force
* php artisan optimize
* php artisan opcache:compile --force

### 排程伺服器 (僅開 queue worker 無 nginx)

* cd /var/www/html/agent/t2_payment
* php artisan optimize:clear
* git checkout develop
* git reset --hard origin/develop
* git clean -df
* git pull
* composer install --no-dev
* php artisan queue:restart

---

## 錯誤等級

* Emergency
  * 系統無法使用
* Alert
  * 需要立刻採取行動
* Critical
  * 嚴重錯誤
* Error
  * 一般錯誤
* Warning
  * 警告
* Notice
  * 一般但重要訊息
* Info
  * 一般訊息
* Debug
  * 除錯專用

---

## Index column size too large. The maximum column size is 767 bytes.

### mysql version < 5.7.9

* Set innodb_file_format=Barracuda
* Set innodb_large_prefix=1
* Set innodb_default_row_format=dynamic

### other

config.database.php > 'engine' => 'innodb row_format=dynamic',

---

## validation

* required = 必須有值
* sometimes = 當鍵出現的時候
* present = 必須有鍵
* sometimes|required = filled = 當鍵出現時必須有值
* present|required = required = 必須有鍵且必須有值

---

## docker 

在 windows 下由於 filesystem 的因素，透過 volume 讀取專案會嚴重托慢速度
以下有兩種解法
1.利用將 vendor 安裝在 container 內來加速反應速度

* RUN COMPOSER_VENDOR_DIR="/srv/vendor" composer install

`public/index.php` and `artisan` 中
include 置換為 `require '/srv/vendor/autoload.php';`
1.掛載 volumes 的時候將 vendor 獨立設定
```
    volumes:
      - ${APP_LOCATION}:/var/www/html
      - '/var/www/html/vendor'
```

