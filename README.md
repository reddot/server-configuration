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
