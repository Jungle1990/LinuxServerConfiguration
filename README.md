# Linux Server Configuration  
- IP: 35.176.52.14  
- SSH Port: 2200  
- App URL: http://35.176.52.14  

# Installed software and configuration changes  
Secure the server  
- Update all currently installed packages  
`sudo apt-get update`  
`sudo apt-get upgrade`  
- Change the SSH port from 22 to 2200  
`sudo vi /etc/ssh/sshd_config`  
change `Port 22` to `Port 2200`  
`sudo service sshd restart`  
- Configure UFW  
`sudo ufw default deny incoming`  
`sudo ufw default allow outgoing`  
`sudo ufw allow 80`  
`sudo ufw allow 123`  
`sudo ufw allow 2200`  
`sudo ufw enable`  
Notice: it seems that the above cmds do not work for Amazon Lightsail server, I had to add the firewall rule using firewall control under network tab  
- Disable remote login of root user and enfore key-based SSH authentication  
`vi /etc/ssh/sshd_config`  
change `PermitRootLogin yes` to `PermitRootLogin no`  
change `PasswordAuthentication no` to `PasswordAuthentication no`  
`sudo service sshd restart`  

Give grader access  
- Create user named grader and grant sudo permission to it  
`sudo adduser grader`  
`sudo usermod -aG sudo grader`  
- Create ssh key for grader  
`su grader`  
`cd ~`  
`ssh-keygen -t rsa`  

Prepare to deploy project  
- Configure the local timezone to UTC  
`sudo dpkg-reconfigure tzdata`  
- Install and configure Apache to serve a Python mod_wsgi application  
`sudo apt-get install apache2`  
`sudo apt-get install libapache2-mod-wsgi`  
`sudo vi /etc/apache2/sites-enabled/000-default.conf`  
add `WSGIScriptAlias / /var/www/html/myapp.wsgi` between `<VirtualHost *:80>` and `</VirtualHost>`  
`sudo apache2ctl restart`  
- Install pip, git, sqlite3  
`sudo apt-get install python-pip`  
`sudo apt-get install git`  
`sudo apt-get install sqlite3`  
- Install python packages  
`pip install sqlalchemy flask oauth2client requests httplib2`  

Deploy the Item Catalog project  
- Clone and setup the Item Catalog project  
`cd ~`  
`git clone https://github.com/Jungle1990/ItemCatalogApplication.git`  
- Fllow the README to start item catalog application  
`cd ~/ItemCatalogApplication/vagrant/catalog`  
`chmod 777 *.py`  
`./setup_db.py`  
`sudo ./application.py`  
