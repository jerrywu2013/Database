### mysql Service Instruction on Ubuntu<br/>
<hr/>
#### Summary<br/>
Noting some common and useful instructions to run mysql service on Ubuntu. The DB uses mysql service include MySQL and MariaDB.<br/>

#### System Environment.<br/>
● Operating System: Ubuntu 14.04 LTS<br/>

####Instructions<br/>
● 查看Mysql狀態
```
sudo netstat -tap | grep mysql
```
● 啟動Mysql
```
/etc/init.d/mysql start
```
或在任何目錄下執行
```
service mysql start
```
● 登入<br/>
有設定密碼
```
mysql -u root -p
```
沒有設定密碼
```
mysql -u root
```
● 建立Mysql密碼
```
mysqladmin -u root password 密碼值
```
● 顯示所有資料庫
```
mysql> show databases;
```
● 切換資料庫
```
mysql> use 資料庫名稱;
```
● 檢視指定資料庫中(切換後的資料庫)的所有資料表
```
mysql> show TABLES;
```
● 顯示特定資料表的欄位資訊(資料表的結構)
```
mysql> show columns from 資料表名稱;
```
● 顯示系統狀態(詳細)
```
mysql> show status;
```
● 退出mysql服務
```
mysql> quit;
```




