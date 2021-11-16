# hardening_ubuntu_20.04
Security Hardening Ubuntu 20.04

# Patching software
- sudo apt update
- sudo apt upgrade

# Creating New User
- adduser username

usermod -aG sudo username
- su - username
- sudo whoami

- exit
- exit

- ssh username@xxx.xxx.xxx.xxx

# Locking root for ssh login
- sudo vim /etc/ssh/sshd_config

PermitRootLogin no

- sudo service ssh restart

# Changing SSH port and account lockout policy
- sudo ufw status
- sudo ufw allow ssh

- sudo ufw allow 222

# change the port

- sudo vim /etc/ssh/sshd_config

Now find following lines, uncomment them (remove #) and type in following

Port value will change port from 22 to 222 and MaxAuthTries will lock out IP address if it enters wrong password in more than 5 attempts (you can set this lower or higher).

- Port 222
- MaxAuthTries 5

- sudo service ssh restart

- ssh username@xxx.xxx.xxx.xxx -p 222

# Other SSH settings

We will be going through few more settings in SSH config file, check them out and see what fits for your workflow.

So, we will be editing

- sudo nano /etc/ssh/sshd_config

# Protocol 2

First thing, we will enable Protocol 2. SSH by default works with Protocol 1. Protocol 2 is newer more secure and robust version introduced in 2006.

- Protocol 2

Restart SSH

- sudo systemctl restart sshd

- ssh -1 username@xxx.xxx.xxx.xxx -p 222

- ssh -2 username@xxx.xxx.xxx.xxx -p 222


# Timeout Idle Value
If you leave your PC unattended for some time with your SSH connections active, it can be an issue. To be honest, I don’t find this setting such a big deal, because one open SSH connection is least of my worries if someone gains physical access to my PC.

If you wish, you can uncomment and set following value (I set 180 seconds, which is 3 minutes)

- ClientAliveInterval 180

# Limit SSH access to some users

You can define user accounts that are enabled to connect to SSH. Every other user would be refused. I will allow access to two users, zeljko and informaticar.

Add following line to your sshd_config file

- AllowUsers zeljko informaticar

You will need to restart ssh service after adding this

- sudo systemctl restart sshd

# Firewall setup

- sudo ufw status

First we will allow outgoing but block incoming connections.

- sudo ufw default allow outgoing
- sudo ufw default deny incoming

We need to let SSH pass through (in case you haven’t changed default port 22 to 222)

- sudo ufw allow ssh

Since I changed default SSH port to 222, I will need to let in port 222 without default SSH rule above.

- sudo ufw allow 222

We can now enable our firewall

- sudo ufw enable

Now if we check our ufw status

- sudo ufw status

# Disable root account completely on your system

To disable root account type in following

- sudo passwd -l root

If you need to re-enable your root user, you can do so by typing in following

- sudo passwd root

# Enable 2FA

You should make this a habit, it brings security of your internet exposed servers to whole new level.

We will install Google Authenticator

- sudo apt install libpam-google-authenticator

We will need to modify /etc/pam.d/sshd file to allow Google Authenticator…

- sudo nano /etc/pam.d/sshd

Add following line

- auth required pam_google_authenticator.so

Next file we will need to edit is

- sudo nano /etc/ssh/sshd_config

Change to yes following line

- ChallengeResponseAuthentication yes

Now, we will configure Google Authenticator for user zeljko

Into terminal just type in

- google-authenticator



# Link:
Security Hardening Ubuntu 20.04:
https://www.informaticar.net/security-hardening-ubuntu-20-04/

How to Install Rkhunter on Ubuntu 20.04:
https://blog.eldernode.com/install-rkhunter-on-ubuntu/
