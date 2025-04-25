# NuxSaas

## NuxSaas
NuxSaas is a SaaS application for managing PHPNuxBill that lets you create and manage your own SaaS application. It is a simple and easy-to-use application built on top of PHPNuxBill using the same database structure.

## Features

- Create and manage your own SaaS application
- Create and manage your own users
- Create and manage your own plans
- Create and manage your own invoices
- Create and manage your own payments
- Create and manage your own subscriptions

## Requirements

- VPS or Dedicated server
- MySQL, Apache, PHP 7.4 or higher installed. [How to install LAMP stack on Ubuntu](https://github.com/alvin-kiveu/TECHBLOG/blob/main/Steps-to-Set-Up-PHP-Apache-MySQL-and-phpMyAdmin-on-Ubuntu.md)
- Cloudflare Account
- Domain name

## Installation Manual

1. Buy and download the latest version from [https://alvinkiveu.com/script/nuxsaas-for-phpnuxbill](https://alvinkiveu.com/script/nuxsaas-for-phpnuxbill) and unzip the files.
2. Rename the folder to `nuxsaas`.
3. Upload the folder to your server using SFTP or SSH to the directory `/var/www/html/`
4. Unzip the file:

```bash
cd /var/www/html/
sudo unzip nuxsaas.zip
```

5. Change ownership and permissions:

```bash
sudo chown -R www-data:www-data /var/www/html/ && sudo chmod -R 755 /var/www/html/
sudo chown -R www-data:www-data /var/www/html/nuxsaas/
```

6. Log in to Cloudflare and add the domain name for your NuxSaas app:
    - Add the domain to your Cloudflare account
    - Change your domain's nameservers to the ones provided by Cloudflare
    - Wait for the DNS to propagate (status should change to active)
    - Add A record:

        - **If using a subdomain:**
            - Type: A
            - Name: `nuxsaas`
            - IPv4: your server IP
            - TTL: Auto
            - Proxy status: DNS only
        
        - **If using full domain:**
            - Type: A
            - Name: `@`
            - IPv4: your server IP
            - TTL: Auto
            - Proxy status: DNS only

### 📌 Notes:
- **Cloudflare Token:** Go to your Cloudflare dashboard → My Profile → API Tokens → Create Token. Choose template or custom permissions (Zone.Zone, Zone.DNS, Zone.Cache Purge).
- **Cloudflare Zone ID:** Go to the domain overview in Cloudflare. You'll find the Zone ID at the bottom of the page.

7. Add the domain to your server:

```bash
cd /etc/apache2/sites-available/
sudo nano nuxsaas.conf
```

8. Add the following config (replace `example.com` with your domain):

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAdmin mail@example.com
    DocumentRoot /var/www/html/nuxsaas

    <Directory /var/www/html/nuxsaas/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    <IfModule mod_dir.c>
        DirectoryIndex index.html index.php
    </IfModule>
</VirtualHost>
```

9. Enable site and required modules:

```bash
sudo a2ensite nuxsaas.conf
sudo a2enmod rewrite
```

10. Restart Apache:

```bash
sudo systemctl restart apache2
```

11. Check configuration:

```bash
sudo apache2ctl configtest
# Should output: Syntax OK
```

12. Optional: Allow `www-data` to reload Apache without password:

```bash
sudo visudo
# Add this line at the end:
www-data ALL=(ALL) NOPASSWD: /usr/sbin/a2ensite, /bin/systemctl reload apache2
```

13. Open your browser and visit the domain you added.

## NuxSaas Installation

1. Go to your domain in the browser
2. Fill in the installation form and click Install
3. If successful, you’ll see “Installation Successful”
4. If not, fix the displayed errors and try again

**Default login credentials:**
```
username: admin
password: admin
```

### 📌 Notes:
- **Telegram Bot Token:** Create a bot using BotFather on Telegram. After creating, you'll receive the token.
- **Telegram Chat ID:**
    1. Send a message to your bot
    2. Visit: `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates`
    3. Look for `chat.id` in the response JSON (e.g., `chat":{"id":123456789,...}`)

