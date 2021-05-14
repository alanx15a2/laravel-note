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

---

## 更新版本

### 一般伺服器

* cd /var/www/html/agent/t2_payment
* php artisan optimize:clear
* php artisan opcache:clear
* git checkout develop
* git reset --hard origin/develop
* git clean -df
* git pull
* composer install --no-dev
* php artisan migrate
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
