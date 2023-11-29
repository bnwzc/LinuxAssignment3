# LinuxAssignment3
## Starting from a Fresh Debian 12 server on digitalocean
## Create a new regular user
- Create a new user
```
useradd <user-name>
```
- Set the password
```
sudo passwd <user-name>
```
### User can perform administrative tasks
- Add the user to the sudo group to allow them to use sudo in Debian
```
sudo usermod -aG sudo <user-name>
```
### User has bash as login shell
- Set User's Shell
```
sudo usermod -s /bin/bash <user-name>
```
### User can access the server via SSH
- Copy the .ssh directory from the root users home directory to the new users home directory (including any files in the directory).
```
sudo cp -R /root/.ssh /home/user_name/
```
- Change ownership of the directory, and files in the directory so that the copy in your new users directory is owned by the new user and the new users primary group
```
sudo chown -R <user-name>:<user-name> /home/user_name/.ssh
```
## Prevent the root user from connecting to the server via SSH
## Install nginx
## Configure nginx to serve a sample website
