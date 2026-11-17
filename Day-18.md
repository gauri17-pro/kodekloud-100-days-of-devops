# Day 18: Install and Configure DB Server

## Install mariadb
```
sudo yum install mariadb-server -y
```

## Start the server and connect to the database server
```
sudo systemctl start mariadb
sudo mariadb
```

## Connect to the server and execute following queries 
```
CREATE DATABASE kodekloud_db7;

CREATE USER 'kodekloud_roy'@'localhost' IDENTIFIED BY 'BruCStnMT5';

GRANT ALL PRIVILEGES ON kodekloud_db7.* TO 'kodekloud_roy'@'localhost';

FLUSH PRIVILEGES;
```
