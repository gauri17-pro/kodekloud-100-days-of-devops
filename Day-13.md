We have one of our websites up and running on our Nautilus infrastructure in Stratos DC. Our security team has raised a concern that right now Apache’s port i.e 8082 is open for all since there is no firewall installed on these hosts. So we have decided to add some security layer for these hosts and after discussions and recommendations we have come up with the following requirements:



#### 1. Install iptables and all its dependencies (including iptables-services) on each app server (stapp01, stapp02 and stapp03). iptables-services alone does not provide the iptables command, so install both packages before you enable the service.
`sudo yum install iptables iptables-services`

#### 2. Block incoming port 8082 on all app servers for everyone except the LBR (load balancer) host stlb01. Either DROP or REJECT is fine.
`getent hosts stlb01`
`LB_IP=$(getent hosts stlb01 | awk '{print $1; exit}')`
```sudo iptables -L INPUT -n```
`sudo iptables -I INPUT 1 -p tcp -s "$LB_IP" --dport 8082 -j ACCEPT`
`sudo iptables -I INPUT 2 -p tcp --dport 8082 -j DROP`
`sudo iptables-save | sudo tee /etc/sysconfig/iptables`

#### 3. Make sure the rules remain, even after system reboot: save them to /etc/sysconfig/iptables and enable the iptables service. Do not reboot the servers; the check reads the saved rules and the service state.
`sudo systemctl enable iptables`
