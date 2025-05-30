# Steps to Set Up PHP, Nginx, MySQL, and phpMyAdmin on Ubuntu

Setting up a LNMP (Linux, Nginx, MySQL, PHP) stack on Ubuntu is a common task for web developers. This guide will walk you through the steps to install and configure Apache, MySQL, PHP, and phpMyAdmin on your Ubuntu server.
This guide assumes you have a fresh installation of Ubuntu 22.04 LTS and have root access to the server.


## Step 1: Install Nginx

### Update and Upgrade Your System
First, ensure your system is up to date:

```bash
sudo apt update && sudo apt upgrade
```

### Install Nginx
Install Nginx using the following command:

```bash
sudo apt install nginx
```

### Setup Firewall

Set up the Uncomplicated Firewall (UFW) to allow public access on default web ports for HTTP and HTTPS:



```bash
sudo ufw allow 'Nginx Full'
```


allow 22/tcp port for SSH

```bash
sudo ufw allow 22/tcp
```

Show all allowed connections

```bash
sudo ufw status
```

Enable UFW

```bash
sudo ufw enable
```

List all allowed connections

```bash
sudo ufw app list
```

Apache management commands

Check the status of Nginx

```bash
sudo systemctl status nginx
```

Start Nginx

```bash
sudo systemctl start nginx
```

Stop Nginx

```bash
sudo systemctl stop nginx
```

Restart Nginx
```bash
sudo systemctl restart nginx
```

Reload Nginx

```bash
sudo systemctl reload nginx
```

Check Version of Nginx
```bash
nginx -v
```

## Steps 2 Install MySQL

Install MySQL

```bash
sudo apt install mysql-server
```

Secure MySQL Installation

```bash
sudo mysql_secure_installation
```

Follow the prompts to secure your installation.

Yes -> 0 -> Yes -> Yes -> Yes

Set the marchine password for MySQL and ignore 


### Create a new user and grant privileges

Access Mysql

```bash
sudo mysql
```
Creat User 

Replace dbusername with your desired username and dbpassword with your desired password

```bash
CREATE USER 'dbusername'@'localhost' IDENTIFIED BY 'dbpassword';
```

Grant privileges

```bash
GRANT ALL PRIVILEGES ON *.* TO 'dbusername'@'localhost';
```

Flush privileges

```bash
FLUSH PRIVILEGES;
```

Exit MySQL

```bash
exit
```

### MySQL Management Commands

Check the status of MySQL

```bash
sudo systemctl status mysql
```

Start MySQL

```bash
sudo systemctl start mysql
```

Stop MySQL

```bash
sudo systemctl stop mysql
```

Restart MySQL

```bash
sudo systemctl restart mysql
```

## Steps 3 Install PHP

Install PHP

```bash
sudo apt install software-properties-common -y
```

```bash
sudo add-apt-repository ppa:ondrej/php -y
```

```bash
sudo apt install php8.2-fpm php8.2 php8.2-common php8.2-mysql php8.2-xml php8.2-xmlrpc php8.2-curl php8.2-gd php8.2-imagick php8.2-cli php8.2-imap php8.2-mbstring php8.2-opcache php8.2-soap php8.2-zip php8.2-intl php8.2-bcmath unzip -y
```

```bash
systemctl status php8.2-fpm
```


Check the version of PHP

```bash
php -v
```

Configure PHP

```bash
sudo nano /etc/php/8.2/fpm/php.ini
```

Change the following lines

```bash
upload_max_filesize = 32M
post_max_size = 48M
memory_limit = 256M
max_execution_time = 600
max_input_vars = 3000
max_input_time = 1000
```

Save and exit

or

```bash
sudo sed -i 's/^upload_max_filesize.*/upload_max_filesize = 32M/; s/^post_max_size.*/post_max_size = 48M/; s/^memory_limit.*/memory_limit = 256M/; s/^max_execution_time.*/max_execution_time = 600/; s/^max_input_vars.*/max_input_vars = 3000/; s/^max_input_time.*/max_input_time = 1000/' /etc/php/8.2/fpm/php.ini
```


Restart Nginx

```bash
sudo systemctl restart nginx
```


```bash
sudo nginx -t
sudo systemctl restart nginx
sudo systemctl restart php8.2-fpm
sudo systemctl restart nginx
```

### 4. Install PhpMyAdmin

```bash
sudo apt install phpmyadmin
```

When prompted:

DO NOT select Apache2 (just press Tab to skip and Enter).

Choose Yes for dbconfig-common. 

Provide your MySQL root password.

Link phpMyAdmin to Nginx Web Root

```bash
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
```

### Configure Nginx to Use PHP
Edit the Nginx configuration file

```bash
sudo nano /etc/nginx/sites-available/default
```

Add the following lines inside the server block

```nginx
location /phpmyadmin {
    root /var/www/html;
    index index.php index.html index.htm;
    location ~ ^/phpmyadmin/(.+\.php)$ {
        try_files $uri =404;
        root /usr/share/;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
    location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
        root /usr/share/;
    }
}
```

Test Nginx Configuration

```bash
sudo nginx -t
```

If you see a message like "nginx: configuration file /etc/nginx/nginx.conf test is successful", it means your configuration is correct.

```bash
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
If you see an error, check the configuration file for typos or syntax errors.

### Restart Nginx

Restart Nginx to apply the changes

```bash
sudo systemctl restart nginx
```

Open your web browser and navigate to:

```bash
http://your_server_ip/phpmyadmin
```

Log in with your MySQL username and password.



## Step 5: Install Git
Install Git

```bash
sudo apt install git
```

Check Git version

```bash
git --version
```

## Step 6: Install Composer
Install Composer

```bash
sudo apt install composer
```
Check Composer version

```bash
composer --version
```

# Change the default index.html page

```bash
sudo rm /var/www/html/index.html
```

```bash
sudo nano /var/www/html/index.html
```


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Welcome to Nginx!</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-color: #0d1117;
            color: #00ff88;
            font-family: 'Courier New', Courier, monospace;
            height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        .terminal {
            background-color: #161b22;
            border: 1px solid #30363d;
            border-radius: 10px;
            padding: 40px 60px;
            box-shadow: 0 0 30px rgba(0, 255, 136, 0.2);
            animation: fadeInUp 1.2s ease-out;
            width: 90%;
            max-width: 600px;
        }

        .terminal h1 {
            color: #00ff88;
            font-size: 2.2rem;
            margin-bottom: 15px;
        }

        .terminal p {
            font-size: 1.1rem;
            margin: 0;
            color: #8affc1;
        }

        .terminal::before {
            content: "$ nginx -t";
            display: block;
            color: #39ff14;
            margin-bottom: 10px;
            animation: typewriter 2s steps(20) 1s 1 normal both;
        }

        @keyframes fadeInUp {
            0% {
                transform: translateY(30px);
                opacity: 0;
            }
            100% {
                transform: translateY(0);
                opacity: 1;
            }
        }

        @keyframes typewriter {
            from { width: 0; }
            to { width: 100%; }
        }

        .blinker {
            animation: blink 1s step-start infinite;
        }

        @keyframes blink {
            50% {
                opacity: 0;
            }
        }

        .prompt-line {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="terminal">
        <h1>✅ Success!</h1>
        <p>Nginx is running and your server is configured correctly.</p>
        <div class="prompt-line"><span>&gt;_</span><span class="blinker">█</span></div>
    </div>
</body>
</html>
``` 






# ADDITIONAL SERVER CONFIGURATION 

### How to add cron job in Ubuntu

```bash
crontab -e
```

Add the following line to run the script every 5 minutes

```bash
*/5 * * * * /usr/bin/php /var/www/html/cron.php
```

Save and exit

### How to add a instll composer in Ubuntu 

```bash
sudo apt install composer
```

Chcek Composer version

```bash
composer --version
```

### How to link an push file with git

Generate token from github vist :

[Generate Github Token](https://github.com/settings/tokens)

Then use the token to clone the repo

```bash
sudo git clone https://<ACCESSTOKEN>@github.com/username/ProjectName.git
```

Or you can use WinSCP to upload files to the server

### How to Point a domain

User ClodeFare to point to the server ip ten add this 

```bash
sudo nano  /etc/nginx/sites-available/mydomain.com
```

Add the following lines


```nginx
server {
    listen 80;
    server_name mydomain.com; # Replace with your domain name

    root /var/www/html/ProjectName;  # Path to your PHP application 'ProjectName'
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.4-fpm.sock;  # Adjust the PHP version if necessary
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    error_log /var/log/nginx/auto157_error.log;
    access_log /var/log/nginx/auto157_access.log;
}
```

Save and exit

Create a symbolic link to enable the site

```bash
sudo ln -s /etc/nginx/sites-available/mydomain.com /etc/nginx/sites-enabled/
```

Test the Nginx configuration

```bash
sudo nginx -t
```
If you see a message like "nginx: configuration file /etc/nginx/nginx.conf test is successful", it means your configuration is correct.

```bash
sudo systemctl restart nginx
```

Happy Coding! :smile:


