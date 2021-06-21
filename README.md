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

Install All
```
sudo add-apt-repository ppa:ondrej/php

sudo apt update

sudo apt install -y \
    zip unzip nginx \
    mysql-server redis-server \
    software-properties-common \
    php8.0-fpm php8.0-mysql php8.0-gd php8.0-mbstring php8.0-xml php8.0-zip \
    supervisor    
```
# 2. LEMP 
NGINX
```
ALREADY INSTALLED
```
MYSQL
```
sudo mysql_secure_installation
```
MYSQL User/DB:
```
sudo mysql
```
```mysql
CREATE DATABASE project_db; # Create Database
CREATE USER 'project_user'@'%' IDENTIFIED WITH mysql_native_password BY '*********'; # Create User + Password
GRANT ALL ON project_db.* TO 'project_user'@'%'; # Grant permisions
```
PHP
```
ALREADY INSTALLED
```
PHP Composer:
```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
php -r "unlink('composer-setup.php');"
```

# Run PHP/NGINX as other user
## Nginx
```
cd /etc/nginx
sudo vim nginx.conf
```
old:
```
user www-data;
```
new:
```
user reddot;
```
## Php-fpm
```
cd /etc/php/8.0/fpm/pool.d
sudo vim www.conf
```
old:
```
...
user = www-data
group = www-data
...
listen.owner = www-data
listen.group = www-data
...
```
new:
```
...
user = reddot
group = reddot
...
listen.owner = reddot
listen.group = reddot
...
```
# Generate SSH Key (for git)
```
ssh-keygen -t ed25519 -C "info@reddot.ge"
cat ~/.ssh/id_ed25519.pub # copy this and paste in github
```

# 3. Redis
Install
```
ALREADY INSTALLED
```
Set Password
```
sudo vim /etc/redis/redis.conf
```
```
# requirepass foobared <-- Uncomment and change foobared to your new password
```
Restart
```
sudo systemctl restart redis
```
Test
```
$ redis-cli
127.0.0.1:6379> ping
(error) NOAUTH Authentication required. # Should throw error
127.0.0.1:6379> auth *YOUR_NEW_PASSWORD*
OK
```
1. [LEMP stack - Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04)
2. [PHP 8](https://linuxize.com/post/how-to-install-php-8-on-ubuntu-20-04)
