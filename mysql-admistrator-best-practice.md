# Administrator best practice commands

#### MYSQL service [Linux]
$ > sudo apt install mysql-server mysql-client
$ > sudo apt install mysql-workbench
$ > sudo service mysqld stop
$ > > sudo service mysqld start

#### MySQL Database Users
> CREATE USER IF NOT EXISTS 'local_user1'@'localhost' IDENTIFIED BY 'mypass'; (Remote connection restricted for this user)
> CREATE USER IF NOT EXISTS 'remote_user1'@'%' IDENTIFIED BY 'mypass';

- Get user info
> SELECT user,host FROM mysql.user;
- User Rename
> RENAME USER 'abc'@'localhost' TO 'xyz'@'%';
- Drop User
>  DROP IF EXISTS USER 'remote_user1'@'%’;
- Change or Update Password
> ALETR USER IF EXISTS 'remote_user1'@'%' IDENTIFIED BY 'mypass';
- Password expire user account
> ALTER USER IF EXISTS 'remote_user1'@'%' PASSWORD EXPIRE;
- Lock User Account
> ALTER USER IF EXISTS 'remote_user1'@'%' ACCOUNT LOCK;

- MySQL Database Users Access Restrictions using privileges.
- Grant all privileges
> GRANT ALL PRIVILEGES ON db1.* TO 'remote_user1'@'%';
- Grant selected privileges on db
> GRANT SELECT, INSERT, UPDATE, DELETE ON db1.* TO 'remote_user1'@'%';
- Grant SELECT privilege single table access to user
> GRANT SELECT ON db1.table1 TO 'remote_user1'@'%';
- Revoking privileges from user:
> REVOKE SELECT, INSERT, UPDATE, DELETE ON db1.* FROM 'remote_user1'@'%';
- Check User Privileges using SHOW GRANTS command
> SHOW GRANTS FOR 'mysqldba'@'localhost';
> SHOW GRANTS; (It will display the privileges granted to the current account)
> SHOW GRANTS FOR 'remote_user1'@'%';

#### MySQL database and table
- Create database if not exist
> CREATE DATABASE IF NOT EXISTS test_db ;
> Use test_db ;
- REATE TABLE:
> CREATE TABLE IF NOT EXISTS t1 (id int(11) primary key auto_incremnet ,uname varchar(50), comments text);
- INSERT INTO TABLE:
> INSERT INTO t1 (id, uname, comments) VALUES(101,’lalit’,’mysql DBA’);
- Show databases.
> SHOW DATABASES;
- show tables;
> SHOW TABLES;
> SELECT TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA = 'test_db';
- show views
> select * from information_schema.VIEWS where TABLE_SCHEMA = 'db_name';

#### MySQL monitoring
- Check Active users:
> show processlist ;
- DB status
> SHOW STATUS;
> SHOW ENGINE INNODB STATUS;

#### Mysqldump  Backup-Restore:
- Full Database backup:
> mysqldump -u root  -p --single-transaction --databases db1  --routines > db1_fullbkp.sql
> mysqldump -u root  -p --single-transaction  --databases db1 --routines | gzip >  db1_fullbkp.sql.gz
- Single table backup:
> mysqldump -u  -h  -p --single-transaction db_name table_name --routines > db1_full.sql
- Restore:
> mysql -u username -p db_name < db1_fullbkp.sql
> gunzip < db1_fullbkp.sql.gz | mysql -u username -p db_name

##### MySQL Replication:
- Create replication user on MASTER with replication privileges.
> CREATE USER [IF NOT EXISTS] 'rpluser'@'%' IDENTIFIED BY 'rpluser1234';GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'rpluser'@'%';
- On Slave: setup replication as follows:
> CHANGE MASTER TO MASTER_HOST='<MASTER_IP',MASTER_USER='rpluser',MASTER_PASSWORD='rpluser1234',MASTER_PORT=3306,MASTER_AUTO_POSITION=1;
- Start slave
> START SLAVE;
> SHOW SLAVE STATUS;

