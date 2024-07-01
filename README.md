# Steps-to-Set-Up-PHP-Apache-MySQL-and-phpMyAdmin-on-Ubuntu


## Steps 1  Install Apache


Update and upgrade your system

```bash
sudo apt update && sudo apt upgrade
```

Install Apache

```bash
sudo apt install apache2
```

### Setup Firewall

Once the apache installation has been finished, we need to set up Uncomplicated Firewall (UFW) with Apache to allow public access on default web ports for HTTP and HTTPS

```bash
sudo ufw allow 'Apache'
```

Enbale Apache Full

```bash
sudo ufw allow 'Apache Full'
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

Check the status of Apache
```bash
sudo systemctl status apache2
```

Start Apache
```bash
sudo systemctl start apache2
```

Stop Apache
```bash
sudo systemctl stop apache2
```

Restart Apache
```bash
sudo systemctl restart apache2
```

Reload Apache
```bash
sudo systemctl reload apache2
```

Check Version of Apache
```bash
sudo apachectl -v
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

Follow the instructions to secure the installation
Yes -> 0 -> Yes -> Yes -> Yes

Set the marchine password for MySQL and ignore 


Create a new user and grant privileges

Access Mysql

```bash
sudo mysql
```
Creat User 

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
sudo apt install php-fpm php libapache2-mod-php php-common php-mysql php-xml php-xmlrpc php-curl php-gd php-imagick php-cli php-imap php-mbstring php-opcache php-soap php-zip php-intl php-bcmath unzip -y
```

Check the version of PHP

```bash
php -v
```

Configure PHP

```bash
sudo nano /etc/php/8.1/apache2/php.ini
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

Restart Apache

```bash
sudo systemctl restart apache2
```

### 4. Install PhpMyAdmin

```bash
sudo apt install phpmyadmin
```

Restart Apache

```bash
sudo systemctl restart apache2
```

Cofigure PHPMYADMIN with Apache

```bash
sudo ln -s /etc/phpmyadmin/apache.conf /etc/apache2/conf-available/phpmyadmin.conf
```

Then, enable the configuration:

```bash
sudo a2enconf phpmyadmin
```

Restart Apache

```bash
sudo systemctl restart apache2
```

Access Php
  
  ```bash
  http://sever-ip/phpmyadmin
  ```

Happy Coding! :smile:


