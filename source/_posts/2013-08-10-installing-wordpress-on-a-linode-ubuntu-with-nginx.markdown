---
layout: post
title: "installing wordpress on a linode ubuntu with nginx"
date: 2013-08-10 16:38
comments: true
categories: nginx, php, wordpress

---

I need mysql.

    apt-get install mysql-client-core-5.5

follow the dialogs.  create a root user and password. remember the password.

Go ahead and log into the MySQL Shell:

    mysql -u root -p

    CREATE DATABASE wordpress;
    CREATE USER wordpressuser@localhost;
    Query OK, 0 rows affected (0.00 sec)
    # use a different password below....
    SET PASSWORD FOR wordpressuser@localhost= PASSWORD("password");
    => Query OK, 0 rows affected (0.00 sec)
    # stays as password.  this is the column name password... not your password
    GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost IDENTIFIED BY 'password';
    => Query OK, 0 rows affected (0.00 sec)
    FLUSH PRIVILEGES;

install php

    sudo apt-get install php5-fpm

Get wordpress

cd ~/
wget http://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
cp wordpress/wp-config-sample.php wordpress/wp-config.php

Install php5-mysql:

    sudo apt-get install php5-mysql

meh.  I'm bored.  followed these tutorials below mostly.

# https://www.digitalocean.com/community/articles/initial-server-setup-with-ubuntu-12-04
# https://www.digitalocean.com/community/articles/how-to-install-wordpress-with-nginx-on-ubuntu-12-04
# https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04