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
Log out from current ssh session

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
    php8.2-fpm php8.2-mysql php8.2-gd php8.2-mbstring php8.2-xml php8.2-zip \
    neovim \
    certbot python3-certbot-nginx \
    supervisor    
```

1.5 (optional) add your device to ssh, to login without password [do it from your local device]
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub reddot@$SERVER_IP
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
https://getcomposer.org/download/
```

# Run PHP/NGINX as other user
## Nginx
```
sudo vim /etc/nginx/nginx.conf
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
sudo vim /etc/php/8.2/fpm/pool.d/www.conf
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

## Change log file permission
```
sudo chown -R reddot:reddot /var/log/nginx/
```

Restart php-fpm/nginx
```
sudo systemctl restart php8.2-fpm.service nginx.service
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

# 4. Install SSL
Install
```
ALREADY INSTALLED
```
Change Server Name (`/etc/nginx/sites-available...`)
```
    server_name _;
```
```
    server_name [...].ge www.[...].ge;
```
Run Certbot
```
sudo certbot --nginx -d example.com
```
1. [LEMP stack - Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-20-04)
2. [PHP 8](https://linuxize.com/post/how-to-install-php-8-on-ubuntu-20-04)
