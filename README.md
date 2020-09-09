<h1>Ubuntu Server (v16.04) set up and Magento 2.3.5 instalation</h1>

## About this article
This article will help you set up Ubuntu server and install magento 2.3.5 
This article will also contain commands that I use so that you can easily copy and paste them. Please note: this article for testing purpose only.

**1. Create a basic Ubuntu Server .**

Build a Web Server that runs:

Ubuntu 16.04;

Apache;

MySQL;

PHP 7.2

**Create a new user**
Create a new superuser that we can use instead of the root user to carry out our administrative tasks.

    adduser dima
Give this user ‘superuser’ privileges by adding it to the ‘sudo’ group

    usermod -aG sudo dima
Logout as the root user and reconnect with our new user.
Access config file to disable root user login (for security).

    sudo nano /etc/ssh/sshd_config
    
Find “PermitRootLogin yes” and replace the “yes” with a “no”.
Now reload the SSH Service for the changes.

    sudo systemctl reload sshd
 **Enable a basic firewall**
 Allow SSH connections to your web server
 

    sudo ufw allow ssh
Turn on the firewall

    sudo ufw enable
**Creating web server**
Convert it into a web server that is capable of installing Magento 2.3.5

**Updating repositories**

    sudo add-apt-repository ppa:ondrej/php
    sudo apt-get update
**Install Apache and configure it specifically for Magento 2.3.5**
Apache is the software that will ultimately convert our basic server into a web server.

    sudo apt-get install apache2 -y
Open Apache settings file to allow .htaccess file usage
Open Apache settings file to allow .htaccess file usage

    sudo nano /etc/apache2/sites-available/000-default.conf
Add the below to the file, save and exit.

    <Directory "/var/www/html">
     AllowOverride All
    </Directory>
Open Apache settings file to set the Global ServerName

    sudo nano /etc/apache2/apache2.conf
Add this line at the end of the file, then save and exit

    ServerName <server_IP>
Check for any errors

    sudo apache2ctl configtest
Enable Apache rewrite

    sudo a2enmod rewrite
Restart Apache for any changes to take effect

    sudo systemctl restart apache2
Enable Apache through the firewall

    sudo ufw allow 'Apache Full'
To test that Apache is working, simply visit the IP address of your server. You should see a Apache2 Ubuntu default test page.

**Install PHP and extensions for Magento 2.3**

This commands to specify 7.2

    sudo apt-get install php7.2 libapache2-mod-php7.2 php7.2-mysql php7.2-soap php7.2-bcmath php7.2-xml php7.2-mbstring php7.2-gd php7.2-common php7.2-cli php7.2-curl php7.2-intl php7.2-zip zip unzip -y

Tell the web server to prefer PHP files (move index.php to front of the list)

    sudo nano /etc/apache2/mods-enabled/dir.conf

Restart apache for changes to take effect

    sudo systemctl restart apache2
<!--
**dmitrijpotapcik/dmitrijpotapcik** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
