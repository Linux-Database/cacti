## httpdバージョンチェック

```
$ httpd -v
Server version: Apache/2.4.51 ()
Server built:   Oct  8 2021 22:03:47
```

## phpインストールチェック

```
rpm -qa | grep php 
php-common-7.2.24-1.amzn2.0.1.x86_64
php-cli-7.2.24-1.amzn2.0.1.x86_64
php-process-7.2.24-1.amzn2.0.1.x86_64
php-devel-7.2.24-1.amzn2.0.1.x86_64
php-json-7.2.24-1.amzn2.0.1.x86_64
php-pdo-7.2.24-1.amzn2.0.1.x86_64
php-fpm-7.2.24-1.amzn2.0.1.x86_64
php-pear-1.10.12-9.amzn2.noarch
php-mysqlnd-7.2.24-1.amzn2.0.1.x86_64
php-xml-7.2.24-1.amzn2.0.1.x86_64
```

## php一部ないので、serverworld基準でインストール

```
yum -y install php php-mbstring
```

## php timezone変更

```
vi /etc/php.ini

[Date]
; Defines the default timezone used by the date functions
; http://php.net/date.timezone
- ;date.timezone = 
+ date.timezone = "Asia/Tokyo"
+ 
```

## mariadb-serverがいないのでインストールし、初期化

```
$ rpm -qa | grep db 
gdb-gdbserver-8.0.1-36.amzn2.0.1.x86_64
gdbm-1.13-6.amzn2.0.2.x86_64
apr-util-bdb-1.6.1-5.amzn2.0.2.x86_64
libdb-5.3.21-24.amzn2.0.3.x86_64
mariadb-libs-10.2.38-1.amzn2.0.1.x86_64
python-pillow-2.0.0-21.gitd1c6db8.amzn2.0.1.x86_64
dbus-libs-1.10.24-7.amzn2.x86_64
dbus-1.10.24-7.amzn2.x86_64
gdb-8.0.1-36.amzn2.0.1.x86_64
mariadb-common-10.2.38-1.amzn2.0.1.x86_64
mariadb-10.2.38-1.amzn2.0.1.x86_64
libdb-utils-5.3.21-24.amzn2.0.3.x86_64
man-db-2.6.3-9.amzn2.0.3.x86_64
mariadb-config-10.2.38-1.amzn2.0.1.x86_64
```

```
$ yum -y install mariadb-server

==================================================================================================
 Package                    Arch   Version               Repository                          Size
==================================================================================================
Installing:
 mariadb-server             x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2  17 M
Installing for dependencies:
 mariadb-backup             x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2 5.9 M
 mariadb-cracklib-password-check
                            x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2  37 k
 mariadb-errmsg             x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2 222 k
 mariadb-gssapi-server      x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2  39 k
 mariadb-rocksdb-engine     x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2 5.5 M
 mariadb-server-utils       x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2 1.6 M
 mariadb-tokudb-engine      x86_64 3:10.2.38-1.amzn2.0.1 amzn2extra-lamp-mariadb10.2-php7.2 833 k
 perl-Compress-Raw-Bzip2    x86_64 2.061-3.amzn2.0.2     amzn2-core                          32 k
 perl-Compress-Raw-Zlib     x86_64 1:2.061-4.amzn2.0.2   amzn2-core                          58 k
 perl-DBD-MySQL             x86_64 4.023-6.amzn2         amzn2-core                         141 k
 perl-DBI                   x86_64 1.627-4.amzn2.0.2     amzn2-core                         804 k
 perl-IO-Compress           noarch 2.061-2.amzn2         amzn2-core                         260 k
 perl-Net-Daemon            noarch 0.48-5.amzn2          amzn2-core                          51 k
 perl-PlRPC                 noarch 0.2020-14.amzn2       amzn2-core                          36 k
 
```


```
$ vi /etc/my.cnf 
 [mysqld]
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
+ character-set-server=utf8
```

## MariaDB起動&自動起動有効

```
systemctl enable --now mariadb
```


## 初期設定

```
#  mysql_secure_installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

#現在パスワードを入力（なしなのでenter）
Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

#rootパスワード入力(Cnetuser)
Set root password? [Y/n] y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

# 匿名ユーザーは削除
Remove anonymous users? [Y/n] Y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

# root のリモートログインは有効のままにしとく
Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

# テストデータベースは削除
Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.


# 特権情報リロード
Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

# インストールできたか確認

```
$ rpm -qa | grep mariadb 
mariadb-errmsg-10.2.38-1.amzn2.0.1.x86_64
mariadb-backup-10.2.38-1.amzn2.0.1.x86_64
mariadb-libs-10.2.38-1.amzn2.0.1.x86_64
mariadb-common-10.2.38-1.amzn2.0.1.x86_64
mariadb-10.2.38-1.amzn2.0.1.x86_64
mariadb-rocksdb-engine-10.2.38-1.amzn2.0.1.x86_64
mariadb-cracklib-password-check-10.2.38-1.amzn2.0.1.x86_64
mariadb-server-utils-10.2.38-1.amzn2.0.1.x86_64
mariadb-gssapi-server-10.2.38-1.amzn2.0.1.x86_64
mariadb-tokudb-engine-10.2.38-1.amzn2.0.1.x86_64
mariadb-server-10.2.38-1.amzn2.0.1.x86_64
mariadb-config-10.2.38-1.amzn2.0.1.x86_64
```

# cacti を yum でインストールする
```
$ yum install cacti

=======================================================================================================
 Package              Arch   Version                          Repository                          Size
=======================================================================================================
Installing:
 cacti                noarch 1.2.19-1.el7                     epel                                31 M
Installing for dependencies:
 cairo                x86_64 1.15.12-4.amzn2                  amzn2-core                         732 k
 dejavu-sans-mono-fonts
                      noarch 2.33-6.amzn2                     amzn2-core                         433 k
 fribidi              x86_64 1.0.2-1.amzn2.1                  amzn2-core                          79 k
 graphite2            x86_64 1.3.10-1.amzn2.0.2               amzn2-core                         115 k
 harfbuzz             x86_64 1.7.5-2.amzn2                    amzn2-core                         279 k
 libX11               x86_64 1.6.7-3.amzn2.0.2                amzn2-core                         606 k
 libX11-common        noarch 1.6.7-3.amzn2.0.2                amzn2-core                         165 k
 libXau               x86_64 1.0.8-2.1.amzn2.0.2              amzn2-core                          29 k
 libXdamage           x86_64 1.1.4-4.1.amzn2.0.2              amzn2-core                          20 k
 libXext              x86_64 1.3.3-3.amzn2.0.2                amzn2-core                          39 k
 libXfixes            x86_64 5.0.3-1.amzn2.0.2                amzn2-core                          18 k
 libXft               x86_64 2.3.2-2.amzn2.0.2                amzn2-core                          60 k
 libXpm               x86_64 3.5.12-1.amzn2.0.2               amzn2-core                          57 k
 libXrender           x86_64 0.9.10-1.amzn2.0.2               amzn2-core                          26 k
 libXxf86vm           x86_64 1.1.4-1.amzn2.0.2                amzn2-core                          17 k
 libglvnd             x86_64 1:1.0.1-0.1.git5baa1e5.amzn2.0.1 amzn2-core                          89 k
 libglvnd-egl         x86_64 1:1.0.1-0.1.git5baa1e5.amzn2.0.1 amzn2-core                          43 k
 libglvnd-glx         x86_64 1:1.0.1-0.1.git5baa1e5.amzn2.0.1 amzn2-core                         125 k
 libthai              x86_64 0.1.14-9.amzn2.0.2               amzn2-core                         187 k
 libwayland-client    x86_64 1.17.0-1.amzn2                   amzn2-core                          34 k
 libwayland-server    x86_64 1.17.0-1.amzn2                   amzn2-core                          40 k
 libxcb               x86_64 1.12-1.amzn2.0.2                 amzn2-core                         216 k
 libxshmfence         x86_64 1.2-1.amzn2.0.2                  amzn2-core                         7.2 k
 mesa-libEGL          x86_64 18.3.4-5.amzn2.0.1               amzn2-core                         108 k
 mesa-libGL           x86_64 18.3.4-5.amzn2.0.1               amzn2-core                         162 k
 mesa-libgbm          x86_64 18.3.4-5.amzn2.0.1               amzn2-core                          38 k
 mesa-libglapi        x86_64 18.3.4-5.amzn2.0.1               amzn2-core                          45 k
 net-snmp             x86_64 1:5.7.2-49.amzn2.1               amzn2-core                         325 k
 net-snmp-agent-libs  x86_64 1:5.7.2-49.amzn2.1               amzn2-core                         701 k
 net-snmp-libs        x86_64 1:5.7.2-49.amzn2.1               amzn2-core                         748 k
 net-snmp-utils       x86_64 1:5.7.2-49.amzn2.1               amzn2-core                         200 k
 pango                x86_64 1.42.4-4.amzn2                   amzn2-core                         280 k
 php-gd               x86_64 7.2.24-1.amzn2.0.1               amzn2extra-lamp-mariadb10.2-php7.2 179 k
 php-gmp              x86_64 7.2.24-1.amzn2.0.1               amzn2extra-lamp-mariadb10.2-php7.2  75 k
 php-intl             x86_64 7.2.24-1.amzn2.0.1               amzn2extra-lamp-mariadb10.2-php7.2 223 k
 php-ldap             x86_64 7.2.24-1.amzn2.0.1               amzn2extra-lamp-mariadb10.2-php7.2  78 k
 php-snmp             x86_64 7.2.24-1.amzn2.0.1               amzn2extra-lamp-mariadb10.2-php7.2  72 k
 pixman               x86_64 0.34.0-1.amzn2.0.2               amzn2-core                         254 k
 rrdtool              x86_64 1.4.8-9.amzn2.0.1                amzn2-core                         369 k
```

## cacti ユーザ作成 
```
$ mysql -u root -p
CREATE USER 'cacti'@'localhost' IDENTIFIED BY 'Cnetuser';
```

## cacti DB作成
```
$ CREATE DATABASE cacti;

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| cacti              |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.00 sec)
```

## cacti に cacti DBの権限付与

```
grant all on cacti.* to cacti@localhost identified by 'Cnetuser';
flush privileges;
```

## cacti DB に データ挿入

```
$ mysql -u cacti -p cacti < /usr/share/doc/cacti-1.2.19/cacti.sql
(Password Cnetuser)
```

## httpd 起動 & 自動起動有効 

```
$ systemctl enable httpd.service --now
```

## cacti.conf 設定変更

```
$ vi /etc/httpd/conf.d/cacti.conf

Alias /cacti    /usr/share/cacti

<Directory /usr/share/cacti/>
        <IfModule mod_authz_core.c>
                # httpd 2.4
-                Require host localhost
+                Require all grented
        </IfModule>
        <IfModule !mod_authz_core.c>
                # httpd 2.2
                Order deny,allow
                Deny from all
                Allow from localhost
        </IfModule>
</Directory>
```

## cacti DBの設定変更

```
$ vi /etc/cacti/db.php

$database_type     = 'mysql';
$database_default  = 'cacti';
$database_hostname = 'localhost';
$database_username = 'cacti';
$database_password = 'Cnetuser';
$database_port     = '3306';
$database_retries  = 5;
$database_ssl      = false;
$database_ssl_key  = '';
$database_ssl_cert = '';
$database_ssl_ca   = '';
$database_persist  = false;
```

## cron にpollerを登録

```
vi /etc/conf.d/cacti
*/5 * * * * cacti /usr/bin/php /usr/share/cacti/poller.php > /dev/null 2>&1
(既にいるのでコメント解除でOK)
```

## 設定できるとログインページが表示
(Chrome でサイトアクセス)
![loginpage](https://raw.githubusercontent.com/Linux-Database/image/main/login.jpg)

ユーザ名   admin
パスワード admin

## ログインできるとパスワード変更ページが表示
![changepass](https://raw.githubusercontent.com/Linux-Database/image/main/passchenge.jpg)
いまのパスワード (admin)

新しいパスワード (Cnetuser1_)

## パスワードが変更できると言語選択になるので［日本語］を選択してGPL への同意をチェック
![setup1](https://raw.githubusercontent.com/Linux-Database/image/main/setup1.jpg)

## 次にインストール時のチェック画面が表示
各項目が警告などになっている箇所があるので、推奨値になるよう修正する。
![setup2](https://raw.githubusercontent.com/Linux-Database/image/main/setup2.jpg)

## MySQL - Timezone Support がエラーなので、修正する
![setup2_error](https://raw.githubusercontent.com/Linux-Database/image/main/setup2_error.jpg)

## タイムゾーンのテーブル作成 & データ挿入

```
$ mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root -p mysql
Enter password: 
Warning: Unable to load '/usr/share/zoneinfo/leapseconds' as time zone. Skipping it.
Warning: Unable to load '/usr/share/zoneinfo/tzdata.zi' as time zone. Skipping it.
```

## cacti にタイムゾーンテーブル権限付与

```
mysql -u root mysql
GRANT SELECT ON mysql.time_zone_name TO cacti@localhost;
flush privileges;
```

## /etc/php.ini を推奨値に設定変変更

```
; Maximum amount of memory a script may consume (128MB)
; http://php.net/memory-limit
- memory_limit = 128M
+ memory_limit = 400M

; Maximum execution time of each script, in seconds
; http://php.net/max-execution-time
; Note: This directive is hardcoded to 0 for the CLI SAPI
- max_execution_time = 30
+ max_execution_time = 60
```

## /etc/my.cnf を推奨値に設定変更

```
$ vi /etc/my.cnf

[mysqld]
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
character-set-server=utf8

+ collation_server=utf8mb4_unicode_ci
+ character_set_server=utf8mb4
+ character_set_client=utf8mb4
+ max_heap_table_size=512M
+ max_allowed_packet=16777216
+ tmp_table_size=128M
+ join_buffer_size=128M
+ innodb_file_per_table=ON
+ innodb_buffer_pool_size=1024M
+ innodb_doublewrite=OFF
+ innodb_flush_log_at_timeout=3
+ innodb_read_io_threads=32
+ innodb_write_io_threads=16
+ innodb_buffer_pool_instances=11
+ innodb_io_capacity=5000
+ innodb_io_capacity_max=10000
```

## DB再起動

```
$ systemctl restart mariadb
```

![setup2_ok](https://raw.githubusercontent.com/Linux-Database/image/main/setup2_ok.jpg)
ページを更新してこのようになればOK

![setup3](https://raw.githubusercontent.com/Linux-Database/image/main/setup3.jpg)
このまま次へ

![setup4](https://raw.githubusercontent.com/Linux-Database/image/main/setup4.jpg)
今回は全てOKだった。何か警告が出ていれば修正する。

![setup5](https://raw.githubusercontent.com/Linux-Database/image/main/setup5.jpg)
今回はバイナリパスは全てOKだった。何かエラーが出ていればインストールする。

![setup6](https://raw.githubusercontent.com/Linux-Database/image/main/setup6.jog)
チェックを付けて次へ

![setup7](https://raw.githubusercontent.com/Linux-Database/image/main/setup7.jpg)
今回は、検証のために1分ごとに情報を取得するように設定した。
ネットワークは環境に合わせて 172.16.32.0/20 に設定した。

![setup8](https://raw.githubusercontent.com/Linux-Database/image/main/setup8.jpg)
テンプテートの設定。今回は全てチェックマークに入れた。

![setup9](https://raw.githubusercontent.com/Linux-Database/image/main/setup9.jpg)
DBに関する画面

![setup9_error](https://raw.githubusercontent.com/Linux-Database/image/main/setup9_error.jpg)
DBの文字コードを変更する

$ mysql -u root -p
ALTER DATABASE cacti CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

![setup9_ok](https://raw.githubusercontent.com/Linux-Database/image/main/setup9_ok.jpg)
このようになればOK

![setup10](https://raw.githubusercontent.com/Linux-Database/image/main/setup10)
チェックマークを入れてインストールボタンを押すとインストールが開始される

![setup11](https://raw.githubusercontent.com/Linux-Database/image/main/setup11.jpg)
インストールできた様子　始めるを押すとコンソールページに移動される。

