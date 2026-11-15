## Disable Root SSH Access Login
Imp: SSH allows you to connect to a Linux server, 

# sshd_config controls who can connect and how, and 
# sudoers controls what users can do after connecting.

vi /etc/ssh/sshd_config
Set permitrootlogin no
