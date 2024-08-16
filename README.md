# Wordpress Container image running on Alpine Linux

This Container image is based on the minimal [Alpine Linux](http://alpinelinux.org/) ready for running [WordPress](https://www.wordpress.org/). (Requires external database)

### Alpine Version 3.20.2 (Released Jul 22, 2024)
##### Wordpress Version latest
##### PHP Version 8.2.22
##### Nginx Version 1.26.2

----

## 🏔️ What is Alpine Linux?
Alpine Linux is a Linux distribution built around musl libc and BusyBox. The image is only 5 MB in size and has access to a package repository that is much more complete than other BusyBox based images. This makes Alpine Linux a great image base for utilities and even production applications. Read more about Alpine Linux here and you can see how their mantra fits in right at home with Container images.

## What is Wordpress?
WordPress is an online, open source website creation tool written in PHP. But in non-geek speak, it's probably the easiest and most powerful blogging and website content management system (or CMS) in existence today.

## ✨ Features

* Minimal size only, minimal layers
* Memory usage is minimal on a simple install.

## 🏗️ Architectures

* ```:amd64```, ```:x86_64``` - 64 bit Intel/AMD (x86_64/amd64)

## Volume structure

* `/usr/html`: Webroot


## 🚀 How to use this image

## Creating an instance

Make sure you create the folder on the host before starting the container and obtain the correct permissions.

```
mkdir -p /data/{domain}/html

docker run -e VIRTUAL_HOST={domain}.com,www.{domain}.com -v /data/{domain}/html:/usr/html -p 80:80 yobasystems/alpine-php-wordpress

E.G

mkdir -p /data/yobasystems/html

docker run -e VIRTUAL_HOST=yobasystems.co.uk,www.yobasystems.co.uk -v /data/yobasystems/html:/usr/html -p 80:80 yobasystems/alpine-php-wordpress
```

The following user and group id are used, the files should be set to this:
User ID:
Group ID:

```
chown -R 100:101 /data/{domain}/html

E.G

chown -R 100:101 /data/yobasystems/html
```

Populate /data/{domain}/html with your WP files.


The following user and group id are used, the files should be set to this:

User ID:

Group ID:


```
chown -R 100:101 /data/{domain}/html
```

### WP-CLI

This image now includes WP-CLI wpcli.org baked in... Its best to `su nginx` before executing anything or else you can potentially compromise your host.

```
docker exec -it <container_name> bash
su nginx
cd /usr/html
wp-cli core download --locale=en_GB
```

### Redis Cache

Edit the wp-config.php file and include the line;

```
define('WP_REDIS_HOST', 'redis');
```

The next thing is to install the plugin [Redis Object Cache](https://wordpress.org/plugins/redis-cache/)


### SSL behind a proxy

If using SSL and running behind a proxy like HAproxy then the following needs to be added to the wp-config.php file (to stop infinite redirect);

```
define('FORCE_SSL_ADMIN', true);
if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false)
   $_SERVER['HTTPS']='on';
```

### Upload limit

The upload limit is 2048 Megabytes.

### Change php.ini value
modify files/php-fpm.conf

To modify php.ini variable, simply edit php-fpm.ini and add php_flag[variable] = value.

```
php_flag[display_errors] = on
```

### PHP Modules
#### List of available modules in Alpine Linux, not all these are installed.
##### In order to install a php module do, (leave out the version number i.e. -5.6.11-r0
```
docker exec <image_id> apk add <pkg_name>
docker restart <image_name>
```
Example:

```
docker exec <image_id> apk add php82-soap
docker restart <image_name>
```

```
php82-common
php82-pdo_sqlite
php82-pear
php82-ftp
php82-imap
php82-mysqli
php82-json
php82-mbstring
php82-soap
php82-litespeed
php82-sockets
php82-bcmath
php82-opcache
php82-dom
php82-zlib
php82-gettext
php82-fpm
php82-intl
php82-openssl
php82-session
php82-pdo_mysql
php82-embed
php82-xmlrpc
php82-wddx
php82-dba
php82-ldap
php82-xsl
php82-exif
php82-pdo_dblib
php82-bz2
php82-pdo
php82-pspell
php82-sysvmsg
php82-gmp
php82-apache2
php82-pdo_odbc
php82-shmop
php82-ctype
php82-phpdbg
php82-enchant
php82-sysvsem
php82-sqlite3
php82-odbc
php82-pcntl
php82-calendar
php82-xmlreader
php82-snmp
php82-zip
php82-posix
php82-iconv
php82-curl
php82-doc
php82-gd
php82-xml
php82-dev
php82-cgi
php82-sysvshm
php82-pgsql
php82-tidy
php82-pdo_pgsql
php82-phar
php82-mysqlnd
```

## Docker Compose example:

```yalm
wordpress:
  image: yobasystems/alpine-php-wordpress
  environment:
    VIRTUAL_HOST: example.co.uk
  expose:
    - "80"
  volumes:
    - /data/example/www:/usr/html
  restart: always
  links:
    - mysql:mysql
mysql:
  environment:
    MYSQL_DATABASE: wordpressdb
    MYSQL_PASSWORD: wordpresspass
    MYSQL_ROOT_PASSWORD: ''
    MYSQL_USER: wordpressuser
  image: yobasystems/alpine-mariadb
```

## 🔍 Image contents & Vulnerability analysis

| PACKAGE NAME          | PACKAGE VERSION | VULNERABILITIES |
|-----------------------|-----------------|-----------------|

