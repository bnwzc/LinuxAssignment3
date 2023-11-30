# LinuxAssignment3
## Starting from a Fresh Debian 12 server on digitalocean
Create an SSH key pair
Create a DO account and your first droplet
Addi your SSH key to your DigitalOcean account
Connect to your new droplet using your ssh key
## Create a new regular user
Create a new user
```
useradd <user-name>
```
Set the password
```
sudo passwd <user-name>
```
### User can perform administrative tasks
Add the user to the sudo group to allow them to use sudo in Debian
```
sudo usermod -aG sudo <user-name>
```
### User has bash as login shell
Set User's Shell
```
sudo usermod -s /bin/bash <user-name>
```
### User can access the server via SSH
Copy the .ssh directory from the root users home directory to the new users home directory (including any files in the directory).
```
sudo cp -R /root/.ssh /home/user-name/
```
Change ownership of the directory, and files in the directory so that the copy in your new users directory is owned by the new user and the new users primary group
```
sudo chown -R <user-name>:<user-name> /home/user-name/.ssh
```
Test that you can connect to your server with your new regular user
```
ssh -i path-to-your-key user-name@your_ip
```
## Prevent the root user from connecting to the server via SSH
Edit ssh configuration so that the root user can no longer connect to the server via ssh
The file we are looking for is the /etc/ssh/sshd_config file
```
sudo vim /etc/ssh/sshd_config
```
Find the line that
```
PermitRootLogin yes
```
and change it to
```
PermitRootLogin no
```
Test that you can no longer connect to your server as root
```
ssh -i path-to-your-key root@root_ip
```
## Install nginx
## Configure nginx to serve a sample website
