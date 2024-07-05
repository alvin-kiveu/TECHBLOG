#  Database Replication

Database replication is the process of copying data from one database to another. It is used to improve the availability, reliability, and performance of databases. There are several types of database replication, including Master-Slave, Master-Master, and Multi-Master replication.

In this tutorial, we will focus on setting up Master-Master,  Master-Slave and Multi-Master replication for a database used for authentication and accounting in a network environment.

## Master-Master Replication

Master-Master replication is a type of database replication where two or more database servers act as both master and slave to each other. This allows for read and write operations on both servers, providing high availability and fault tolerance.


Install MySQL/MariaDB on both servers if not already installed.

```bash
sudo apt-get update
sudo apt-get install mysql-server
```

### Configure Server 1:

Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf).

```bash
[mysqld]
server-id=1
log_bin=binlog
binlog_do_db=radius_db
```

Create a replication user.


```sql
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
FLUSH PRIVILEGES;
```

Get the binary log file position.

```sql
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```

Note the File and Position values and release the lock.


```sql
UNLOCK TABLES;
```


Configure Server 2:

Edit the MySQL configuration file on Server 2.

```cnf
[mysqld]
server-id=2
log_bin=binlog
binlog_do_db=radius_db
```


Set up replication on Server 2 using the details from Server 1.

```sql
CHANGE MASTER TO MASTER_HOST='192.168.1.1', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
```

Configure Server 1 to replicate from Server 2:

Get the binary log file position from Server 2.

```bash
SHOW MASTER STATUS;
```

Set up replication on Server 1 using the details from Server 2.

```bash
CHANGE MASTER TO MASTER_HOST='192.168.1.2', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
```

Verify Replication:

Check the status on both servers.

```sql
SHOW SLAVE STATUS\G;
```


## Master-Slave Replication:

Master-Slave replication is a type of database replication where one database server acts as the master and another server acts as the slave. The master server handles write operations, while the slave server handles read operations.


Configure Master (Server 1):

Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf).

```cnf
[mysqld]
server-id=1
log_bin=binlog
binlog_do_db=radius_db
```


Create a replication user.

```sql
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
```

```sql
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
```

```sql
FLUSH PRIVILEGES;
```
Get the binary log file position.

```sql
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```
SHOW MASTER STATUS;
Note the File and Position values and release the lock.

```sql
UNLOCK TABLES;
```


Configure Slave (Server 2):

Edit the MySQL configuration file on Server 2.

```cnf
[mysqld]
server-id=2
log_bin=binlog
binlog_do_db=radius_db
```

Set up replication on the Slave server using the details from the Master server.

```sql
CHANGE MASTER TO MASTER_HOST='192.168.1.1', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
START SLAVE;
```


Check the status on the Slave server.

```sql
SHOW SLAVE STATUS\G;
```


## Multi-Master replication.

Multi-Master replication is a type of database replication where multiple database servers act as both master and slave to each other. This allows for read and write operations on all servers, providing high availability and fault tolerance.


Configure Server 1:

Edit the MySQL configuration file (/etc/mysql/mysql.conf.d/mysqld.cnf).

```cnf
[mysqld]
server-id=1
log_bin=binlog
binlog_do_db=radius_db
```

Create a replication user.

```sql
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
```

```sql
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
```

```sql
FLUSH PRIVILEGES;
```

Get the binary log file position.

```sql
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```

Note the File and Position values and release the lock.

```sql
UNLOCK TABLES;
```

Configure Server 2:

Edit the MySQL configuration file on Server 2.

```cnf
[mysqld]
server-id=2
log_bin=binlog
binlog_do_db=radius_db
```

Set up replication on Server 2 using the details from Server 1.

```sql
CHANGE MASTER TO MASTER_HOST='192.168.1.1', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
```

```sql
START SLAVE;
```

Configure Server 1 to replicate from Server 2:

Get the binary log file position from Server 2.

```sql
SHOW MASTER STATUS;
```

Set up replication on Server 1 using the details from Server 2.

```sql
CHANGE MASTER TO MASTER_HOST='192.168.1.2', MASTER_USER='repl', MASTER_PASSWORD='password', MASTER_LOG_FILE='binlog.000001', MASTER_LOG_POS=154;
```

```sql
START SLAVE;
```

Verify Replication:

Check the status on both servers.

```sql
SHOW SLAVE STATUS\G;
```

## Conclusion

In this tutorial, you learned how to set up Master-Master, Master-Slave, and Multi-Master replication for a database used for authentication and accounting in a network environment. Replication provides high availability, fault tolerance, and improved performance for databases. You can further customize the configuration to meet your specific requirements.

## Support the Blog

Support the blog by sharing this tutorial with others. You can also support the blog by donating to the author. Thank you for reading.

[]: # (END)
[]: # (Database-Replication.md)


Happy coding! :smiley:






