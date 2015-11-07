### Cassandra Cluster Configuration<br/>
<hr/>
#### Summary<br/>
Cassandra is a high scalable database in comparison with other NoSQL databases. Especially, the performance of Cassandra growth linearly with the increased of the nodes in cluster. About how to make multiple Cassandra nodes to form a single cluster, you can reference this tutorial.
<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● Cassandra: 2.2.3<br/>

#### Prerequisite<br/>
You have prepared multiple Cassandra nodes.

#### Configuration Steps<br/>
Please follow the steps list below：<br/>
1.Stop Cassandra service<br/>
Stop Cassandra service on each nodes, if it is running.
```
sudo service cassandra stop
```
2.Enable Cassandra listen on the public network<br/>
Enable each nodes listen on the public network.
```
sudo vi /etc/cassandra/cassandra.yaml
```
Locate the line:
```
listen_address: localhost
```
and change it to: 
```
listen_address: host_ip_address
```
3.Define a cluster and thier nodes<br/>
Locate the line in /etc/cassandra/cassandra.yaml file:<br/>
```
cluster_name: 'Test Cluster'
```
and change it to:<br/>
```
cluster_name: 'self-defined cluster name'
```
Note: You must configure the corresponded cluster name on each nodes.<br/>
So far, you have defined multiple nodes belong to a specific cluster. But each nodes in the cluster is isolated yet.<br/>
4.Enable cluster nodes connect to each other<br/>
Locate the line in /etc/cassandra/cassandra.yaml file:<br/>
```
seeds: "127.0.0.1"
```
and change it to:<br/>
```
seeds: "other node's ip address"
```
5.Start Cassandra service<br/>
Start Cassandra service on each nodes.<br/>
```
sudo service cassandra start
```
6.Test Cluster<br/>
Pick a node among cluster and login cqlsh prompt.<br/>
```
cqlsh –u user_name –p user_password
```
Create a new keyspace called Test.<br/>
```
user_name@cqlsh>CREATE KEYSPACE Test WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
```
Add a new table called users to our new keyspace.<br/>
```
user_name@cqlsh>CREATE TABLE Test.users (user_name varchar PRIMARY KEY,password varchar,info varchar);
```
Insert a new record to the new table.<br/>
```
user_name@cqlsh>INSERT INTO Test.users (user_name,password,info) VALUES ('andy','1234','user info.');
```
Query the table to vaidate if the above actions is successful.<br/>
```
SELECT * from Test.users;
```
The output will be:
```
user_name  | info       | password
-----------+------------+----------
      andy | user info. |     1234


(1 rows)
```
If your cluster have successfully been configured, the other cluster nodes will establish the same keyspace replication automatically in a few minute later.<br/>

#### Reference<br/>
● DigitalOcean: https://www.digitalocean.com/community/tutorials/how-to-use-the-apache-cassandra-one-click-application-image
