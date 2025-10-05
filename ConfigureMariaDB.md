### Install MariaDB

- sudo yum install mariadb-server
- systemctl start mariadb

### Create Dabase And Query

<pre>
CREATE DATABASE testdb;
CREATE TABLE mtestdb.users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(50));
INSERT INTO testdb.users (name) VALUES ('Anish');
SELECT * FROM testdb.users;
</pre>

### Backup And Restore

- mysqldump -u root -p database_name > backup.sql

### Delete database

- mysql -u root -p
- drop database testdb

### Create testdb again to test backup restore

- create database testdb
- mysql -u root -p testdb < backup.sql
