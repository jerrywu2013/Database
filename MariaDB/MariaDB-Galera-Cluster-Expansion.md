### MariaDB Galera Cluster Expansion<br/>
<hr/>
#### Summary<br/>
Adding a cluster node in MariaDB Galera Cluster is easy. There is no need to interrupt a running service. Just to do some simple settings on the MariaDB server which you want it to be added in a existing cluster.<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● MariaDB: 10.0<br/>
● MariaDB Galera Cluster: 10.0<br/>

#### Prerequisite<br/>
● There have an existing cluster.<br/> 
● A MariaDB server has prepared to add in a cluster.<br/>
Memo:<br/>
● About how to install MariaDB on a server, please reference the tutorial [Install MariaDB on Ubuntu](https://github.com/andychen1060/Database/blob/master/MariaDB/Install%20MariaDB%20on%20Ubuntu.md).<br/>
● About how to setting MariaDB Galera Cluster, please reference the tutorial [MariaDB Galera Cluster Setting](https://github.com/andychen1060/Database/blob/master/MariaDB/MariaDB-Galera-Cluster-Setting.md).<br/>  

#### Setting Steps<br/>
Please follow the steps list below：<br/>
1.Install MariaDB Galera Cluster<br/>
On the new cluster node, install MariaDB Galera Cluster on it.<br/>
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install mariadb-galera-server galera rsync
```
2.Create account used to sychronize MariaDB<br/>
Login in the new cluster node's database. Created an user account, which is used to sychronize database, and set its privilege.
```
MariaDB [(none)]> GRANT ALL PRIVILEGES ON *.* TO '<user_name>'@'%' IDENTIFIED BY '<user_password>';
```
3.Configure cluster info.<br/>
Configure the new cluster node to connect to an cluster.<br/>
On the new cluster node, added the file "/etc/mysql/conf.d/cluster.cnf" and typed the below configuration.<br/>
```
[mysqld]
# Mysql Settings
binlog_format=ROW
default-storage-engine=innodb
innodb_autoinc_lock_mode=2
query_cache_size=0
query_cache_type=0
bind-address=0.0.0.0

# Galera Settings
wsrep_node_address="New_Node_IP"
wsrep_provider=/usr/lib/galera/libgalera_smm.so
wsrep_cluster_name="Cluster_Name"
wsrep_cluster_address="gcomm://Other_Node_IP"
wsrep_sst_auth=Cluster_User:Cluster_Password
wsrep_sst_method=rsync
```
Memo:<br/>
● Replace label New_Node_IP with the new cluster node's IP address.<br/>
● Replace label Cluster_Name with the existing cluster name which you want the new cluster node to add in.<br/>
● Replace label Other_Node_IP with IP address of any other cluster node of the existing cluster.<br/>
● Replace label Cluster_User and Cluster_Password with your database account and password, which is used for database synchronization, respectively.<br/>
<br/>
4.Ensure the new cluster node has the same password of debian-sys-maint as existing cluster node<br/>
Choosed one existing cluster node and opened the file /etc/mysql/debian.cnf. Copy its password to the same file of the new cluster node.<br/>
<br/>
5.Stop the new cluster node<br/>
Stop run service mysql on the new cluster node.<br/>
```
sudo service mysql stop
```
6.Open 3 ports in firewall<br/>
● 3306: MariaDB<br/>
● 4444: RSync<br/>
● 4567: Galera Cluster<br/>
<br/>
7.Start the new cluster node<br/>
Start service mysql on the new cluster node.
```
sudo service mysql start
```

8.Test<br/>
Test if the new cluster node is successfully added in the cluster. Login in MariaDB of any existing cluster node. Type the command list below:
```
MariaDB [(none)]> show status like 'wsrep%';
```
The result will be:
```
Variable_name              | Value
------------------------------------------------------------------------------------------------
wsrep_local_state_comment  | Synced
wsrep_incoming_address     | 10.203.98.1:3306,10.203.98.2:3306,10.203.98.3:3306,10.203.98.4:3306
wsrep_cluster_size         | 4
wsrep_cluster_status       | Primary
wsrep_connected            | ON
wsrep_ready                | ON
...                        | ...
------------------------------------------------------------------------------------------------
```
If the value of attribute wsrep_cluster_size have plused by 1 ,it indicates that you have successfully expanded a cluster.  
