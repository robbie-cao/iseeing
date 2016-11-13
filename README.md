# Web Site with Drupal

## Server Setup

### Basic

```
# LAMP
$ sudo apt-get install wget curl apache2 mysql-server sqlite php5

# PHP Modules
$ sudo apt-get install php5-gd
$ sudo apt-get install php5-mysql php5-sqlite php5-pgsql
$ sudo apt-get install php5-curl
$ sudo php5enmod curl

$ sudo apt-get install libssh2-php
$ sudo php5enmod ssh2

$ sudo service apache2 restart
```

> https://www.drupal.org/docs/develop/local-server-setup/linux-development-environments/installing-php-mysql-and-apache-under

### PHP Configuration

```
--- a/etc/php5/apache2/php.ini
+++ b/etc/php5/apache2/php.ini
@@ -373,7 +373,7 @@ zend.enable_gc = On
 ; threat in any way, but it makes it possible to determine whether you use PHP
 ; on your server or not.
 ; http://php.net/expose-php
-expose_php = On
+expose_php = Off
 
 ;;;;;;;;;;;;;;;;;;;
 ; Resource Limits ;
@@ -403,7 +403,7 @@ max_input_time = 60
 
 ; Maximum amount of memory a script may consume (128MB)
 ; http://php.net/memory-limit
-memory_limit = 128M
+memory_limit = 196M
 
 ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
 ; Error handling and logging ;
@@ -813,7 +813,7 @@ max_file_uploads = 20
 
 ; Whether to allow the treatment of URLs (like http:// or ftp://) as files.
 ; http://php.net/allow-url-fopen
-allow_url_fopen = On
+allow_url_fopen = Off
 
 ; Whether to allow include/require to open URLs (like http:// or ftp://) as files.
 ; http://php.net/allow-url-include
@@ -865,6 +865,14 @@ default_socket_timeout = 60
 ; default extension directory.
 ;
 
+extension=gd.so
+extension=pdo.so
+extension=pdo_mysql.so
+extension=pdo_pgsql.so
+extension=pdo_sqlite.so
+
+extension=ssh2.so
+
 ;;;;;;;;;;;;;;;;;;;
 ; Module Settings ;
 ;;;;;;;;;;;;;;;;;;;
```

**Remember run `sudo service apache2 restart` after config php.**

> https://www.drupal.org/docs/7/system-requirements/php

## Install Drupal

### Download

```
$ wget https://ftp.drupal.org/files/projects/drupal-7.51.tar.gz
$ tar zxvf drupal-7.51.tar.gz
$ mv drupal-7.51 drupal
```

### Config Apache

**/etc/apache2/apache2.conf**

```
<Directory /web/samples/drupal-7.51/>
       Options Indexes FollowSymLinks
       AllowOverride All
       Require all granted
</Directory>
```

**/etc/apache2/sites-available/002-drupal.conf**

```
Listen 8001

<VirtualHost *:8001>
       # The ServerName directive sets the request scheme, hostname and port that
       # the server uses to identify itself. This is used when creating
       # redirection URLs. In the context of virtual hosts, the ServerName
       # specifies what hostname must appear in the request's Host: header to
       # match this virtual host. For the default virtual host (this file) this
       # value is not decisive as it is used as a last resort host regardless.
       # However, you must set it for any further virtual host explicitly.
       #ServerName www.example.com

       ServerAdmin webmaster@localhost
       DocumentRoot /web/samples/drupal-7.51

       # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
       # error, crit, alert, emerg.
       # It is also possible to configure the loglevel for particular
       # modules, e.g.
       #LogLevel info ssl:warn

       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined

       # For most configuration files from conf-available/, which are
       # enabled or disabled at a global level, it is possible to
       # include a line for only one particular virtual host. For example the
       # following line enables the CGI configuration for this host only
       # after it has been globally disabled with "a2disconf".
       Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```

## Install Drupal

1. Create `settings.php` and `files` folder

   ```
   $ cd cd drupal-7.51/sites/default
   $ cp default.settings.php settings.php
   $ chmod a+w settings.php
   $ mkdir files
   $ chmod 0777 files
   ```

2. Install Drupal in web browser on address: http://<your-web-address or ip>:8001

3. Follow the steps on page to install and setup your site.


## Reference

- https://www.drupal.org/docs/7
- https://en.wikipedia.org/wiki/Drupal
