### Install Cassandra on Ubuntu<br/>
<hr/>
#### Summary<br/>
Install Cassandra on Ubuntu Server.<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● Cassandra: 2.2.3<br/>

#### Prerequisite<br/>
Cassandra is developed using Java. To run it, we need the Java runtime environment to be installed first! About how to install JRE, please reference the tutorial [Installing Java SE Runtime Environment(JRE)](https://github.com/andychen1060/Ubuntu-Server/blob/master/Installing%20Java%20SE%20Runtime%20Environment(JRE).md).

#### Installing Step<br/>
Please follow the steps list below：<br/>
1.Set Cassandra repository
```
echo "deb http://www.apache.org/dist/cassandra/debian 22x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
echo "deb-src http://www.apache.org/dist/cassandra/debian 22x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
```
2.Update repository 
```
sudo apt-get update
```
A warning message relating to the Cassandra repository will output due to the lack of three public keys in the procedure of package signature.<br/>
```
...(omitted)
Fetched 2,749 kB in 35s (77.4 kB/s)                                            
Reading package lists... Done
W: GPG error: http://www.apache.org 22x InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 749D6EEC0353B12C
```
3.Add public keys<br/>
Add three public keys of Apache Software Foundation.<br/>
First key
```
gpg --keyserver pgp.mit.edu --recv-keys 749D6EEC0353B12C
gpg --export --armor 749D6EEC0353B12C | sudo apt-key add -
```
Memo: Due to the warning message from repository updating, We set the public key to 749D6EEC0353B12C.<br/>
Second key
```
gpg --keyserver pgp.mit.edu --recv-keys 2B5C1B00
gpg --export --armor 2B5C1B00 | sudo apt-key add -
```
Third Key
```
gpg --keyserver pgp.mit.edu --recv-keys 0353B12C
gpg --export --armor 0353B12C | sudo apt-key add -
```
Update repository
```
sudo apt-get update
```
Normally, there will no error message appeared relating to updating the cassandra repository source.<br/>
4.Install Cassandra
```
sudo apt-get install cassandra
```
5.Check the status of Cassandra<br/>
After installation, Cassandra should have been started automatically. Let's check the status of it.
```
sudo service cassandra status
```
Output:
```
* Cassandra is running
```
6.Check the status of the cluster
```
sudo nodetool status
```
Output:
```
Datacenter: datacenter1
=======================
Status=Up/Down
|/ State=Normal/Leaving/Joining/Moving
--  Address    Load    Tokens  Owns    Host ID                               Rack
UN  127.0.0.1  96.89 KB 256     ?        934eadb9-e37e-4c84-9c42-9588e3eb3ee9  rack1
Note: Non-system keyspaces don't have the same replication settings, effective ownership information is meaningless
```
Memo: In the output, UN means it's Up and Normal.<br/>
7.Login to Cassandra<br/>
```
cqlsh
```
Output:
```
Connected to Test Cluster at 127.0.0.1:9042.
[cqlsh 5.0.1 | Cassandra 2.2.3 | CQL spec 3.3.1 | Native protocol v4]
Use HELP for help.
cqlsh>
```
8.Logout Cassandra<br/>
```
cqlsh> exit
```
#### Reference:<br/>
● DigitalOcean: https://www.digitalocean.com/community/tutorials/how-to-install-cassandra-and-run-a-single-node-cluster-on-ubuntu-14-04
