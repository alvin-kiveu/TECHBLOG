
# ALLOW REMOTE CONNECTION TO MYSQL

## MySQL Configuration

### MySQL Configuration File

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Change the bind-address to

```bash
bind-address = 0.0.0.0
```

Spacific ip address

```bash
bind-address = YOUR_SERVER_IP
```

To open port 3306 using ufw (Uncomplicated Firewall), run:

```bash
sudo ufw allow 3306
```

If you only want to allow a specific IP address (e.g., 3.10.130.61), use:

```bash
sudo ufw allow from 3.10.130.61 to any port 3306
```

Reload the firewall:

```bash
sudo ufw reload
```

### Restart MySQL Service

```bash
sudo service mysql restart
```



# MySQL

## MySQL Create User

```sql
CREATE USER 'RemoteUser'@'%' IDENTIFIED BY 'StrongPassword123!';

```

## MySQL Grant User

```sql
GRANT ALL PRIVILEGES ON cloudtik_account_create_db.* TO 'RemoteUser'@'%';

```

## MySQL Grant User with Grant Option

```sql
GRANT ALL PRIVILEGES ON cloudtik_account_create_db.* TO 'RemoteUser'@'%' WITH GRANT OPTION;

```

## MySQL Flush Privileges

```sql
FLUSH PRIVILEGES;

```

## MySQL Show Grants

```sql
SHOW GRANTS FOR 'RemoteNetgauird'@'%';

```

