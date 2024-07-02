1. Database Replication
Assuming you are using MySQL/MariaDB for your RADIUS database, you can use Master-Master or Master-Slave replication.

Master-Master Replication:

Install MySQL/MariaDB:
Install MySQL/MariaDB on both servers if not already installed.

bash
Copy code
sudo apt-get update
sudo apt-get install mysql-server
Configure Server 1:

Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf).
cnf
Copy code
[mysqld]
server-id=1
log_bin=binlog
binlog_do_db=radius_db
Create a replication user.
sql
Copy code
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
FLUSH PRIVILEGES;
Get the binary log file position.
sql
Copy code
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
Note the File and Position values and release the lock.
sql
Copy code
UNLOCK TABLES;
Configure Server 2:

Edit the MySQL configuration file on Server 2.
cnf
Copy code
[mysqld]
server-id=2
log_bin=binlog
binlog_do_db=radius_db
Set up replication on Server 2 using the details from Server 1.
sql
Copy code
CHANGE MASTER TO MASTER_HOST='192.168.1.1', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
Configure Server 1 to replicate from Server 2:

Get the binary log file position from Server 2.
sql
Copy code
SHOW MASTER STATUS;
Set up replication on Server 1 using the details from Server 2.
sql
Copy code
CHANGE MASTER TO MASTER_HOST='192.168.1.2', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
Verify Replication:

Check the status on both servers.
sql
Copy code
SHOW SLAVE STATUS\G;
Master-Slave Replication:

Configure Master (Server 1):

Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf).
cnf
Copy code
[mysqld]
server-id=1
log_bin=binlog
binlog_do_db=radius_db
Create a replication user.
sql
Copy code
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
FLUSH PRIVILEGES;
Get the binary log file position.
sql
Copy code
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
Note the File and Position values and release the lock.
sql
Copy code
UNLOCK TABLES;
Configure Slave (Server 2):

Edit the MySQL configuration file on Server 2.
cnf
Copy code
[mysqld]
server-id=2
Set up replication on the Slave server using the details from the Master server.
sql
Copy code
CHANGE MASTER TO MASTER_HOST='192.168.1.1', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
Verify Replication:

Check the status on the Slave server.
sql
Copy code
SHOW SLAVE STATUS\G;
