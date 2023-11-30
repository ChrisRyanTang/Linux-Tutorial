# Tutorial
## Christopher Tang A01029033

## Open up Windows terminal
open up Windows terminal

## Connecting to Linux Terminal
copy your digital ocean `droplet_ip`
At the terminal, type `ssh -i .\.ssh\do-key root@droplet_ip`
`ssh` is a secure shell
`-i` identity file
`root` is the username

## Creating a user
At the Linux terminal, type ```sudo useradd -ms /bin/bash username```
`useradd` creates a new user
`-m` Creates a home directory if it does not exist.
`-s` The name of the user's login shell.
`/bin/bash` is the default shell for Linux

## Give a password to a user
At the Linux terminal, type ```sudo passwd username```
`passwd` password file

## Give a user sudo privileges
At the Linux terminal, type ```sudo usermod -aG sudo username```
`usermod` modify a user account
`-a` Adds the user to the supplementary group(s). Use only with -G option.
`-G` A list of supplementary groups which the user is also a member of. Each group is separated from the next by a comma, with no intervening whitespace. The groups must exist.

## Copying .ssh directory
On the Linux terminal type, `sudo cp -r /root/.ssh /home/username`
`cp` copy files and directories
`-r` copy directories recursively

## Changing ownership of .ssh directory
On the Linux terminal type, `sudo chown -R username:username /home/username/.ssh`
`chown` change file owner and group
`-R` operates on files and directories recursively

## Logging in with the New user
Exit from the Linux terminal by typing, `exit`
From the Windows terminal type, `ssh -i .\.ssh\do-key username@droplet_ip`
You should be logged in as the user.

## Configure SSH for New User
At the Linux terminal type, `sudo vim /etc/ssh/sshd_config`
Locate the line that starts with `PermitRootLogin` and set that value to `no`.
`PermitRootLogin no`
Save and exit the editor by typing in the command `:wq`
Restart the the ssh service by typing, `sudo systemctl restart ssh`
`systemctl` is a command for controlling the systemd system and service manager.

## Installing and Updating Packages
At the Linux terminal type, `sudo apt update && sudo apt upgrade`
`apt` is a command-line interface for the package management system
`update` is used to resynchronize the package index files from their sources.
`upgrade` is used to install the newest versions of all packages that are currently installed on the system.

## Installing Nginx
On the Linux terminal type, `sudo apt install nginx`
`apt` is a command-line interface for the package management system
`nginx` is a web server which can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.
Once that has finished downloading head into the www directory.

## Making a index.html
On the Linux terminal type, `cd /var/www`
Make a new directory inside the www.
On the Linux terminal type, `sudo mkdir my-site`
`mkdir` make directories
Head into the my-site directory.
On the Linux terminal type, `cd /var/www/my-site`
We can go ahead and make a vim file for the index.html.
On the Linux terminal type, `sudo vim index.html`
`vim` is a text editor
Copy and paste inside that vim file,
```<!DOCTYPE html>
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
Save and quit the editor by typing in the command `:wq`.
`:wq` is a command to write the file to disk and quit the editor.


## Creating a server block
Head into directory /etc/nginx/service-available.
On the Linux terminal type, `cd /etc/nginx/service-available`
Create a my-site.conf file inside that directory.
On the Linux terminal type, `sudo vim my-site.conf`
`vim` is a text editor
Copy and paste inside that vim file, 
```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	
	root /var/www/html;
	
	index index.html index.htm index.nginx-debian.html;
	
	server_name _;
	
	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

## Configure a server block
In the same file `my-site.conf` edit the directory.
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
Changing the `root /var/www/html` to `root /var/www/my-site`so that we can use the index.html we have configured earlier.
Save and quit the editor by typing in the command, `:wq`
`:wq` is a command to write the file to disk and quit the editor.


## Deleting the default files inside service-available and service-enable
On the Linux terminal type, `sudo rm -r /etc/nginx/service-available/default`
`rm` remove files or directories
`-r` remove directories and their contents recursively 
On the Linux terminal type, `sudo rm -r /etc/nginx/service-enable/default`

## Creating a symbolic link
On the Linux terminal type, `sudo ln -s /etc/nginx/service-available/my-site.conf /etc/nginx/service-enable/my-site.conf@`
`ln` make links between files
`-s` symbolic link

## Restarting Nginx
On the Linux terminal type, `sudo nginx -t`
`nginx` is a web server which can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.
`-t` test configuration and exit
On the Linux terminal type, `sudo systemctl restart nginx`
`systemctl` is a command for controlling the systemd system and service manager.
Check the status of the nginx service.
On the Linux terminal type, `systemctl status nginx`
`systemctl` is a command for controlling the systemd system and service manager.
You should see that the nginx service is active and running.


## Viewing the website
On your browser you can use your `droplet_ip` and see what is displayed for your html document.

