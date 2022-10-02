
#  Implement a Client Server Architecture using MySQL Database Management System (DBMS).

This project implements a Client-Server architecture using MySQL database, a database server is insalled on a separate instance and a client to manage the database server is installed on another instance. This can be used in hardening the security of the database server because the security settings can be set to allow security from the other instance and thereby blocking internet connection to the database server. MySQL database runs on port 3306 by default and the default settings for MySQL can be found in **/etc/mysql/**.


## Project Requirements

- Two EC2 instances running on AWS
- MySQL server installed on one instance
- MySQL client installed on the other instance
- Security rule: 3306 for MySQL, 22 for SSH
- Terminal, MobxTerm, Termius, EC2 Instance Connect, RDP, Putty

## EC2 Instances

![EC2 Instances](https://github.com/scholarship-task/database-client-server-architecture/blob/main/project5-ec2.png?raw=true)

## Installation

Install **mysql-server** on MySQL-Server EC2 instance with the following commands to get the latest packages from the repo and also install MySQL-server and also Confirm that MySQL server is running

```bash
  sudo apt update -y
  sudo apt install mysql-server -y
  sudo systemctl status mysql
```
    
Install **mysql-client** on MySQL-Server EC2 instance with the following commands to get the latest packages from the repo and also install MySQL-Client

```bash
  sudo apt update -y
  sudo apt install mysql-client -y
```


## Configuring the MySQL Server to accept connection from the MySQL-Client IP address

configure MySQL server to allow connections from remote hosts.
Replace the IP address of the **bind_address** in the configuration from **127.0.0.1** setting with **0.0.0.0**

```bash
    sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
Configuring MySQL server
Open the MySQL database server as a Root user
```bash
    sudo mysql
```
In the MySQL terminal enter the following commands to perform the following actions respectively:
- Create a user to allow connection from the client IP
- Grant the user the required permissions to be able to carry out the required operations, the principle of least privilege should be applicable for real production database systems.
- Apply the changes with immediate effect
- Restart the database server

```bash
  CREATE USER mysql_user@172.31.83.227 IDENTIFIED BY 'password';
  GRANT ALL ON . TO mysql_user@172.31.83.227;
  FLUSH PRIVILEGES;
  sudo systemctl restart mysql
```
    
Connect to the MySQL server instance from the MySQL client instance
This command will prompt for a password, the password will be the password used when creating the user on the MySQL server
```bash
  mysql -u mysql_user -p -h  172.31.91.42
  show databases;
```

show databases is MySQL query to show all the databses the user has the privilege to view, edit or update;
-u username <mysql_user>
-p port 3306 <default>
-h Hosts <IP-ADDRESS>

