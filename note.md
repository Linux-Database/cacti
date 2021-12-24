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