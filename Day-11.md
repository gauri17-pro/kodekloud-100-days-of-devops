## Install tomcat 
```
sudo yum install tomcat -y
```

## Change the connector port from 8080 to 5002
```
sudo vi /etc/tomcat/server.xml
```
```
<Connector port="5002" protocol="HTTP/1.1"
```
