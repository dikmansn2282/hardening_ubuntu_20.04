# hardening_ubuntu_20.04
Security Hardening Ubuntu 20.04

# Patching software
sudo apt update
sudo apt upgrade

# Creating New User
adduser username

usermod -aG sudo username
su - username
sudo whoami

exit
exit

ssh username@xxx.xxx.xxx.xxx

# Locking root for ssh login
sudo vim /etc/ssh/sshd_config

PermitRootLogin no

sudo service ssh restart

# Changing SSH port and account lockout policy
sudo ufw status
sudo ufw allow ssh

sudo ufw allow 222


# change the port


sudo vim /etc/ssh/sshd_config

Now find following lines, uncomment them (remove #) and type in following

Port value will change port from 22 to 222 and MaxAuthTries will lock out IP address if it enters wrong password in more than 5 attempts (you can set this lower or higher).

Port 222
MaxAuthTries 5

sudo service ssh restart

ssh username@xxx.xxx.xxx.xxx -p 222

# Other SSH settings

We will be going through few more settings in SSH config file, check them out and see what fits for your workflow.

So, we will be editing

sudo nano /etc/ssh/sshd_config

# Protocol 2

First thing, we will enable Protocol 2. SSH by default works with Protocol 1. Protocol 2 is newer more secure and robust version introduced in 2006.

Protocol 2








# Link:
Security Hardening Ubuntu 20.04:
https://www.informaticar.net/security-hardening-ubuntu-20-04/

How to Install Rkhunter on Ubuntu 20.04:
https://blog.eldernode.com/install-rkhunter-on-ubuntu/
