# LinuxAssignment3
## Starting from a Fresh Debian 12 server on digitalocean
### Create an SSH key pair
1. First create a new .ssh directory, run this command in your terminal.It doesn't matter if it exsisted.
```
mkdir .ssh
```
2. Then create your new ssh key pair, run this command in your terminal.
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
For Windows
```
Get-Content C:\Users\user-name\.ssh\do-key.pub | Set-Clipboard
```
For MacOS
```
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
1. Create a new user, the user has bash as login shell and has a home directory.
```
useradd -ms /bin/bash <user-name>
```
2. Set the password
```
sudo passwd <user-name>
```
### User can perform administrative tasks
Add the user to the sudo group to allow them to use sudo in Debian
```
sudo usermod -aG sudo <user-name>
```

### User can access the server via SSH
1. Copy the .ssh directory from the root users home directory to the new users home directory (including any files in the directory).
```
sudo cp -r /root/.ssh /home/<user-name>/
```
2. Change ownership of the directory, and files in the directory so that the copy in your new users directory is owned by the new user and the new users primary group
```
sudo chown -R <user-name>:<user-name> /home/<user-name>/.ssh/
```
3. Exit the root and test that you can connect to your server with your new regular user
```
ssh -i path-to-your-key <user-name>@your_ip
```
## Prevent the root user from connecting to the server via SSH
1. Edit ssh configuration so that the root user can no longer connect to the server via ssh. The file we are looking for is the /etc/ssh/sshd_config file
```
sudo vim /etc/ssh/sshd_config
```
2. Find the line that
```
PermitRootLogin yes
```
and change it to
```
PermitRootLogin no
```
4. Save the file and restart the ssh service. We will look at services and systemctl in another class. You can do this with the following command:
```
sudo systemctl restart ssh.service
```
5. Test that you can no longer connect to your server as root
```
ssh -i path-to-your-key root@root_ip
```
You should get a permission denied error
## Install nginx
1. Update the packages to make sure it is the latest.
```
sudo apt update
```
2. Install the Nginx
```
sudo apt install nginx
```
3. Start the Nginx service
```
sudo systemctl start nginx
```
4. Check the status of Nginx service
```
sudo systemctl status nginx
```
## Configure nginx to serve a sample website
1. Create a new directory which is called my-site in /var/www
```
sudo mkdir -p /var/www/my-site
```
2. Create the sample webpage index.html document in the my-site directory.
```
sudo vim /var/www/my-site/index.html
```
```
# Sample Page
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2420</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        h1 {
            text-align: center;
        }
    </style>
</head>
<body>
    <h1>Hello, World</h1>
</body>
</html>
```
3. Configuring a server block
Create a new file my-file in /etc/nginx/sites-available and enter the nginx config below into it.
```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	
	root /var/www/my-site;
	
	index index.html index.htm index.nginx-debian.html;
	
	server_name _;
	
	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```
4. Create a symbolic link
create a symbolic link to your new config file in /etc/nginx/sites-enabled
```
sudo ln -s /etc/nginx/sites-available/my-file /etc/nginx/sites-enabled/
```
5. Delete the duplicated link
Delete the duplicated link in /etc/nginx/sites-enabled
```
sudo unlink /etc/nginx/sites-enabled/default
```
6. Test your nginx configurations
```
sudo nginx -t
```
7. Restart your nginx server
```
sudo systemctl restart nginx
```
8. Send an HTTP GET request (replace your-ip-address)
```
curl <your-ip-address>
```
Then you can go to your ip address to see the page.
