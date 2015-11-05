### Security Configuration<br/>
<hr/>
#### Summary<br/>
In this tutorial, I will configure Cassandra to make it more secure by user authentication and removing some default setting.
<br/>

#### Version Info.<br/>
● Ubuntu: 14.04 LTS<br/> 
● Cassandra: 2.2.3<br/>

#### Prerequisite<br/>
You have a basic Cassandra being installed.

#### Configuration Steps<br/>
Please follow the steps list below：<br/>
1.Stop Cassandra service<br/>
Stop Cassandra service, if it is running.
```
sudo service cassandra stop
```
2.User authentication<br/>
By default, Cassandra is logined without account and password. To make Cassandra more secure, we need to restrict it to do user authentication.<br/>
Modify Cassandra configuration.<br/>
```
sudo vi /etc/cassandra/cassandra.yaml
```
Locate the line:
```
authenticator: AllowAllAuthenticator
```
and change it to: 
```
authenticator: PasswordAuthenticator
```
You are now being restricted to login Cassandra with an account and password.<br/>
3.Login Cassandra<br/>
For the first time, we need to login Cassandra using a default account and passsword which is offered by itself.<br/>
```
cqlsh –u cassandra –p cassandra
```
4.Create a new administrator<br/>
```
cassandra@cqlsh>CREATE USER new_admin WITH PASSWORD 'new_password' SUPERUSER;
cassandra@cqlsh>exit
cqlsh –u new_admin –p 'new_password’
```
5.Change the default administrator(cassandra)<br/>
Enhance its password and remove its SUPERUSER authority.<br/>
```
new_admin@cqlsh>ALTER USER cassandra WITH PASSWORD ‘a more enhanced password’ NOSUPERUSER;
```


#### Reference:<br/>
● DigitalOcean: https://www.digitalocean.com/community/tutorials/how-to-use-the-apache-cassandra-one-click-application-image
