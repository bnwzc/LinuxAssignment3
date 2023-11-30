# LinuxAssignment3
## Starting from a Fresh Debian 12 server on digitalocean
### Create an SSH key pair
First create a new .ssh directory, run this command in your terminal.
```
# It doesn't matter if it exsisted.
mkdir .ssh
```
Then create your new ssh key pair, run this command in your terminal.
```
ssh-keygen -t ed25519 -f .ssh/do-key -C "your-email-address"
```
### Create a DO account. 
You will need a credit card to do that.
### Add your SSH key to your DigitalOcean account
1. Click **"Setting"** button on the left navbar.
2. Choose **"Security"** in the menu.
3. Click the **"Add SSH Key"** button and it will need your public SSH key.
4. Run this command in your terminal to ger your public key(replace the "user-name")
```
# Windows
Get-Content C:\Users\user-name\.ssh\do-key.pub | Set-Clipboard
```
```
# MacOS
pbcopy < ~/.ssh/your-key.pub
```
5. Paste the content into textarea in DO and set the name
6. Click the **"Add SSH Key"** button
### Create a droplet and connect it with you SSH key
1. Click the **"Droplets"** button on the left navbar.
2. Click the **"Create Droplet"** button.
3. Select the server you want.(For example, Deibian and version 12) and click the **"Create Droplet"** button.
### Login with your SSH key
In your terminal, run this command. The ip address is on your Droplet in DO. (You should use the private key to login)
```
ssh -i path-to-your-key root@your-ip
```
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
Set User's shell to bash
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
Exit the root and test that you can connect to your server with your new regular user
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
