---
title: "Setup Magento Local Development with nginx"
date: 2019-07-16T21:15:52+07:00
draft: false
description: "Setup Magento Local Development on nginx, Fedora 30"
tags: ["magento", "php", "development", "nginx", "fedora"]
---

## Prasyarat ##

Tulisan ini memperlihatkan langkah-langkah dalam men-setup
Magento 2.3.2 dengan nginx di Fedora 30 ke atas.
Maka prasyarat yang harus dipenuhi antara lain:
1. OS Fedora 30 atau lebih baru
2. nginx
3. PHP 7.2
4. MySQL (MariaDB)
5. tarball Magento 2.3.2
6. composer

### Install _software_ di Fedora ###

PHP 7.2 tidak tersedia di repo utama Fedora mulai versi 28.
Maka, untuk Fedora 30, perlu menambahkan repo dari Remi
Ikuti panduan dari web Remi di <!--more--> https://rpms.remirepo.net/wizard/. Kemudian, install paket-paket yang dibutuhkan dengan DNF.

```
nginx mariadb-server \
php72-php-fpm php72-php-pdo php72-php-mysqlnd php72-php-opcache php72-php-xml \
php72-php-sodium php72-php-gd php72-php-devel php72-php-mysql php72-php-intl php72-php-mbstring \
php72-php-bcmath php72-php-json php72-php-iconv php72-php-soap php72-php-zip php72-php-cli
```

Unduh juga composer dari websitenya https://getcomposer.org/download/.
Tambahkan binary composer ke path Fedora, boleh path user wise, atau system.

```
export /path/to/composer/bin:$PATH
```

### Setup nginx ###

Magento sudah menyertakan konfigurasi untuk nginx virtualhost, letaknya di file `nginx.conf.sample`.
Jadi, file tersebut bisa di-include ke conf custom nginx, misalnya di `/etc/nginx/conf.d/magento.conf`. Isinya seperti ini:
```
upstream fastcgi_backend {
server unix:/var/opt/remi/php72/run/php-fpm/www.sock;
}
 
server {
listen 80;
listen [::]:80;
server_name magento.test; # menyesuaikan
set $MAGE_ROOT /app/sites/magento; # menyesuaikan
include /app/sites/magento/nginx.conf.sample;
}
```
Tambahkan entri domain custom (_magento.test_ dalam cuplikan kode di atas) ke file `/etc/hosts`.
Tes dengan `sudo nginx -t`, jika ok, restart systemd nginx.

### Permission ###

Default user worker nginx adalah 'nginx', dan PHP-FPM adalah 'apache'.
Demi konsistensi, bikin saja user baru 'www-data'.
Di `/etc/nginx/nginx.conf` ubah user menjadi 'www-data'.
Juga di `/etc/opt/remi/php72/php-fpm.d/www.conf`, ubah _user, group, listen-user_
dan _listen-group_ ke 'www-data'.
Tambahkan user existing ke grup 'www-data' juga.
```
useradd -Mr www-data && usermod -aG <user> www-data
```

Selain permission folder harus bisa _read-write_, Fedora juga perlu mengeset
permission SELinux.
```
semanage permissive -a httpd_t
chcon -R -t httpd_sys_content_t /sites/app/magento
chcon -R -t httpd_sys_rw_content_t /sites/app/magento
```

### Unduh tarball Magento 2.3.2 ###

Unduh tarball Magento terbaru dari laman resminya di https://magento.com/tech-resources/download.
Sebelum versi ini, Magento tidak menyediakan tarball di laman unduhan resmi mereka.

Ekstrak tarball dan rename foldernya menyesuaikan konfigurasi nginx tadi.
Sebelum mengakses web installer, kita perlu
menyesuaikan _permission_ file dan folder tertentu di Magento.

```
find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} +
find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} +
chown -R :www-data .
```

Juga perlu menghapus cache instalasi sebelum instalasi.
```
php72 bin/magento setup:uninstall
```

## Instalasi Magento ##

Setelah setup server lokal selesai, dilanjutkan dengan
instalasi Magento. Akses base-url yang sudah ditentukan pada konfigurasi nginx.

![magento check requirements to install](/img/2019/07/magento-install-1.png)

![magento database config](/img/2019/07/magento-install-2.png)

![magento web config](/img/2019/07/magento-install-3.png)

![magento customization](/img/2019/07/magento-install-4.png)

![magento admin account](/img/2019/07/magento-install-5.png)

![magento installation progress](/img/2019/07/magento-install-6.png)

![magento success install](/img/2019/07/magento-install-7.png)

![magento login admin](/img/2019/07/magento-install-8.png)