httpdバージョンチェック
$ httpd -v
Server version: Apache/2.4.51 ()
Server built:   Oct  8 2021 22:03:47

phpインストールチェック
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

php一部ないので、serverworld基準でインストール
yum -y install php php-mbstring

php timezone変更
vi /etc/php.ini

[Date]
; Defines the default timezone used by the date functions
; http://php.net/date.timezone
- ;date.timezone = 
+ date.timezone = "Asia/Tokyo"
+ 

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

mariadb-serverがいないのでインストールし、初期化
yum -y install mariadb-server

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
 
 
vi /etc/my.cnf 
 [mysqld]
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
+ character-set-server=utf8

systemctl start mariadb


[root@ip-172-31-44-47 repo]#  mysql_secure_installation 

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



$ rpm -qa | grep mariadb 
mariadb-server-10.2.38-1.amzn2.0.1.x86_64
