# server-configuration

# 1. Create User
1.1
```
sudo useradd -m reddot
```
1.2 Set Password
```
sudo passwd reddot
```
1.3 Add User to sudoers
```
usermod -aG sudo reddot
```
1.4 (optional) change shell
```
chsh -s /bin/bash
```

# 2. LEMP 
NGINX
```
sudo apt update
sudo apt install nginx
```
MYSQL
```
sudo apt install mysql-server
```
```
sudo mysql_secure_installation
```
PHP
```
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php

sudo apt update
sudo apt install php8.0-fpm
sudo apt install php8.0-mysql php8.0-gd
```


1. [LEMP stack - Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04)
2. [PHP 8](https://linuxize.com/post/how-to-install-php-8-on-ubuntu-20-04)
