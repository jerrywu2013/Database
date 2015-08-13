### MariaDB Galera Cluster Setting<br/>
<hr/>
#### Summary<br/>
Setting MariaDB Galera Cluster to integrate multiple DB servers(also called a node) into a cluster. Every node in the cluster can sychronize automatically and immediately with other cluster nodes.<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● MariaDB: 10.0<br/>
● MariaDB Galera Cluster: 10.0<br/>

#### Prerequisite<br/>
Before setting MariaDB Galera Cluster, you need to prepare at least three database servers. Install MariaDB on each server which you want it to be a node of the cluster. To install MariaDB, please follow the tutorial ["Install MariaDB on Ubuntu"](https://github.com/andychen1060/Database/blob/master/MariaDB/Install%20MariaDB%20on%20Ubuntu.md).<br/>
In this tutorial, I premised that you have prepared three servers and the ip of each server is 10.203.98.1, 10.203.98.2 and 10.203.98.3 respectively.<br/>

#### Setting Steps<br/>
Please follow the steps list below：<br/>
1.Install MariaDB Galera Cluster<br/>
Install MariaDB Galera Cluster on each node of cluster<br/>
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install mariadb-galera-server galera rsync
```
2.Create account used to sychronize MariaDB<br/>
Login in each database of nodes. Created an user account, which is used to sychronize database, and set its privilege.
```
MariaDB [(none)]> GRANT ALL PRIVILEGES ON *.* TO '<user_name>'@'%' IDENTIFIED BY '<user_password>';
```
3.Configure cluster info.<br/>
Configure each node to connect to the other cluster nodes to form a cluster.<br/>
Added the file "/etc/mysql/conf.d/cluster.cnf" and typed the below setting.<br/>
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
wsrep_node_address="This_Node_IP"
wsrep_provider=/usr/lib/galera/libgalera_smm.so
wsrep_cluster_name="Cluster_Name"
wsrep_cluster_address="gcomm://Other_Node1_IP,Other_Node2_IP"
wsrep_sst_auth=Cluster_User:Cluster_Password
wsrep_sst_method=rsync
```
Memo:<br/>
● Replace label This_Node_IP to the current node's IP address.<br/>
● Replace label Cluster_Name to your self-defined cluster name.<br/>
● Replace label Other_Node1_IP and Other_Node2_IP to the other two node's IP addresses of the cluster respectively.<br/>
● Replace label Cluster_User and Cluster_Password to your database account and password, which is used for database synchronization, respectively.<br/>
<br/>
4.Ensure every node has same password of debian-sys-maint<br/>
Choosed one node and opened the file /etc/mysql/debian.cnf. Copy its password to the same file of other cluster nodes. To ensure every node of cluster has same password of debian-sys-maint.<br/>
<br/>
5.Stop all cluster node<br/>
Stop run service mysql on every node of cluster<br/>
```
sudo service mysql stop
```
6.Open 3 ports in firewall<br/>
● 3306: MariaDB<br/>
● 4444: RSync<br/>
● 4567: Galera Cluster<br/>
<br/>
7.Start cluster node<br/>
Start first cluster node.
```
sudo service mysql start --wsrep-new-cluster
```
Start service mysql on the other cluster nodes.
```
sudo service mysql start
```
8.Test<br/>
Test if the cluster was configured successfully. Login in MariaDB of any cluster node. Type the command list below:
```
MariaDB [(none)]> show status like 'wsrep%';
```
The result will be:
```
Variable_name              | Value
-------------------------------------------------------------------------------
wsrep_local_state_comment  | Synced
wsrep_incoming_address     | 10.203.98.1:3306,10.203.98.2:3306,10.203.98.3:3306
wsrep_cluster_size         | 3
wsrep_cluster_status       | Primary
wsrep_connected            | ON
wsrep_ready                | ON
...                        | ...
-------------------------------------------------------------------------------
```
