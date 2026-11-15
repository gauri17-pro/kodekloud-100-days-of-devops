## Install the required SELinux packages.
```sudo yum install -y selinux-policy selinux-policy-targeted```

## Permanently disable SELinux 
```vi /etc/selinux/config```
Change it from enforcing to disabled
```SELinux=disabled```
