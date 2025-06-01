# V-Server setup

## Description

A short guide how to generate SSH-Keys, disabling password authenitification and set up a basic V-Server with installing NGINX and creating a alternative html site on NGINX-Server.

## Prerequisites
You need a Ubuntu Cloud VM to setting your V-Server.

## Table of contents

1. [Create SSH-Key-Pair](#1-create-ssh-key-pair)
2. [Login with password](#2-login-with-password)
3. [Deacitvate passwordlogin](#3-deacitvate-passwordlogin)
4. [Install NGINX](#4-install-nginx)
5. [Create alternative site on NGINX-Server](#5-create-alternative-site-on-nginx-server)

## 1. Create SSH-Key-Pair
Create SSH-Key-Pair with standard ed25519 on your local maschine:
```
ssh-keygen -t ed25519 -C "your-email@example.com"
```
## 2. Login with Password
Login to your V-Server with your Username and password: 
```
ssh <username>@<ip-adress>
```


## 3. Deacitvate passwordlogin
1. after login in your V-Server:
```
sudo nano /etc/ssh/sshd_config
```
2. Search to "#PasswordAuthentication yes" and change it to "PasswordAuthentication no"
3. Save the file and exit
4. Restart the ssh service:
```
sudo sysrtemctl restart ssh.service" 
```

## 4. Install NGINX
1. After login in your V-Server update apt:
```
        sudo apt update" 
```
2. install NGINX:
```
        sudo apt install nginx -y
```
3. After the installation, verify the NGINX service status:
```
systemctl status nginx.service
```
    
## 5. Create alternative site on NGINX-Server
1. Create a new Directory:
```
mkdir -p /var/www/alternatives
```
2. Create a new HTML-File:
```
sudo touch /var/www/alternatives/alternate-index.html
```
3. Add a new alternate-index.html file configuration to /etc/nginx/sites-enabled/alternatives:
```
sudo nano /etc/nginx/sites-enabled/alternatives
```
the content:
```
        server {
            listen 8081;
            listen [::]:8081;

            root /var/www/alternatives;
            index alternate-index.html;

            location / {
                try_files $uri $uri/ =404;
            }
        }
```
And now you can run your alternative HTML-file in this case on Port 8081 in your explorer:
```
http://<ip-adress>:8081
```
