### Install MariaDB on Ubuntu<br/>
<hr/>
#### Summary<br/>
Install MariaDB on Ubuntu Server.<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● MariaDB: 10.0<br/>

#### Installing Step<br/>
Please following the steps list below：<br/>
1.Adding apt repository<br/>
Typing below command in terminal to add the MariaDB repository information in apt.<br/>
```
sudo apt-get install software-properties-common
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
sudo add-apt-repository 'deb http://download.nus.edu.sg/mirror/mariadb/repo/10.0/ubuntu trusty main'
```
2.Updating apt 
```
sudo apt-get update
```
3.Installing MariaDB database
```
sudo apt-get install mariadb-server
```
4.Login into MariaDB database
```
mysql -u root -p
```
5.Finish<br/>
If you install successfully, then the output will be like this.
```
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection is 45
Server version: 10.0.20-MariaDB-1~trusty mariadb.org binary distribution

Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
