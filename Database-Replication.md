## Database Replication

To enable real-time synchronization between two MySQL databases hosted on Server A and Server B, the best approach is to set up MySQL Replication. MySQL replication ensures that any changes made on the Master server (Server A) are automatically propagated to the Slave server (Server B) in real-time.

Here’s a step-by-step guide to set up MySQL Replication for real-time database synchronization:

### Step 1: Prepare Server A (Master) for Replication

Edit MySQL Configuration:

On Server A, you need to configure MySQL to act as the Master server.

Open MySQL configuration file (typically /etc/mysql/my.cnf or /etc/my.cnf depending on your system):

```bash
sudo nano /etc/mysql/my.cnf
```

Add or modify the following lines in the [mysqld] section:

```ini
[mysqld]
server-id = 1  # Unique server ID for the master
log_bin = /var/log/mysql/mysql-bin.log  # Enable binary logging
binlog_do_db = cloudtik_account_create_db  # The database you want to replicate
```


Save and close the file.

Restart MySQL:

Restart MySQL to apply the changes:

```bash
sudo systemctl restart mysql
```


Create Replication User on Server A:

You need to create a special user for replication. This user will allow Server B to connect to Server A for replication purposes.

Log into MySQL:

```bash
mysql -u root -p
``` 


Run the following SQL command to create a replication user:

```sql
CREATE USER 'replication_user'@'%' IDENTIFIED BY 'replication_password';
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
FLUSH PRIVILEGES;
```

Replace 'replication_password' with a secure password for the replication user.


Get the Master Log Position:

To set up replication on Server B, you need to know the current position of the binary log. Run this command to get the log file name and position:

```sql
SHOW MASTER STATUS;
```

Take note of the File and Position values.

### Step 2: Prepare Server B (Slave) for Replication

Edit MySQL Configuration:

On Server B, configure MySQL to act as the Slave server.

Open MySQL configuration file (/etc/mysql/my.cnf or /etc/my.cnf):

```bash
sudo nano /etc/mysql/my.cnf
```

Add or modify the following lines in the [mysqld] section:

```ini
[mysqld]
server-id = 2  # Unique server ID for the slave (must be different from the master)
```

Save and close the file.

Restart MySQL on Server B:

Restart MySQL to apply the changes:

```bash
sudo systemctl restart mysql
```

Load the Initial Database Dump (Optional):

If Server B does not have the same data as Server A, you can create a dump from Server A and import it into Server B.

On Server A:

```bash
mysqldump -u root -p --all-databases --single-transaction --flush-logs --master-data > backup.sql
```

Transfer the dump file (backup.sql) to Server B:

```bash
scp backup.sql user@server_b_ip:/path/to/destination
```

Import the dump into MySQL on Server B:

```bash
mysql -u root -p < backup.sql
```


### Step 3: Set Up Replication on Server B


Log into MySQL on Server B:

```bash
mysql -u root -p
```

Configure Replication on Server B:

Run the following command to configure Server B to start replicating from Server A:

```sql
CHANGE MASTER TO
  MASTER_HOST = 'server_a_ip',  # IP address of Server A (Master)
  MASTER_USER = 'replication_user',
  MASTER_PASSWORD = 'replication_password',
  MASTER_LOG_FILE = 'mysql-bin.000001',  # Use the File value from SHOW MASTER STATUS
  MASTER_LOG_POS = 123;  # Use the Position value from SHOW MASTER STATUS
```

Replace 'server_a_ip', 'replication_user', 'replication_password', 'mysql-bin.000001', and 123 with the actual values from your setup.

Start Replication on Server B:

Start the replication process on Server B:

```sql
START SLAVE;
```

Check Replication Status:

You can check the replication status to ensure everything is working correctly:

```sql
SHOW SLAVE STATUS\G
```

Look for the following fields:

Slave_IO_Running: Should be Yes.
Slave_SQL_Running: Should be Yes.


If both are Yes, the replication is working correctly.

Step 4: Test and Monitor Replication
Test Replication:

To verify that replication is working in real time, make a change on Server A (e.g., insert data into cloudtik_account_create_db):

On Server A:

```sql
USE cloudtik_account_create_db;
INSERT INTO test_table (column1) VALUES ('Test Data');
```

Then, check the same table on Server B:


```sql
SELECT * FROM cloudtik_account_create_db.test_table;
```

The data should appear on Server B almost immediately.

Monitor Replication:

To monitor replication, periodically check the status:

```sql
SHOW SLAVE STATUS\G
```

Pay attention to the Seconds_Behind_Master field. Ideally, this should be 0 or close to it, indicating minimal lag in replication.

Conclusion

By setting up MySQL Replication, you can achieve real-time synchronization between the databases hosted on Server A (Master) and Server B (Slave). Any changes made on Server A will be automatically propagated to Server B, ensuring that both databases remain consistent.

Master: Server A (where writes happen)
Slave: Server B (which gets real-time updates from the master)
Let me know if you encounter any issues or need further clarification!







