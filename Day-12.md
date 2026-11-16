## Trying to run the tomcat server on each of the servers and then access them via jump host

```
ss -tulnp
sudo fuser -v 3001/tcp
sudo systemctl stop sendmail
```

```
sudo vi /etc/tomcat/server.xml
```

# Allow port 3001 in iptables: Insert an ACCEPT rule for TCP port 3001 above any catch-all REJECT rules (adjust the line number based on your iptables -L output, typically line 1 or right before the reject rule).

```
iptables -I INPUT 1 -p tcp --dport 3001 -j ACCEPT
```
