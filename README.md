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

**Install MySQL**

    sudo apt-get install mysql-server -y

Run a MySQL security script

    sudo mysql_secure_installation

**Install phpMyAdmin and secure the installation**

    sudo apt-get install phpmyadmin php7.2-mbstring php7.2-gettext -y

(select apache2 using your space bar before hitting ENTER)

Restart Apache

    sudo systemctl restart apache2

**Test phpMyAdmin**

Just put your IP address into your browser followed by /phpmyadmin .

**Download Magento 2.3**

Important run commands as the correct user. The reason for creating a new user is for added security. The main user that I created before has the ability to run superuser commands if required, where the web user would never need such privileges.

**Create a Magento user**
This user will also be who you log in as to upload and download your files via FTP.

Add the user 

    sudo adduser magento
    
Make the web server group the primary group for the new user

    sudo usermod -g www-data magento
    
**Folder permissions**

When I installed Apache, it automatically created a web directory to store own web files. However, it will have created this under the default user known as www-data. So, we just need to update the folder permissions for the directory that we’ll be installing Magento. This will allow our new Magento user to operate correctly.

**Update the root file ownership to coincide with our new web user**

    sudo chown magento:www-data /var/www/html/

**Install Composer**
Install Composer by downloading the file 

    sudo curl -sS https://getcomposer.org/installer | php

Move composer file to the required directory

    sudo mv composer.phar /usr/local/bin/composer

**Magento Access Keys**

be sure to visit  https://marketplace.magento.com/ and create an account. Then you can generate a set of access keys that will allow you to download from the Magento repository via Composer.


**Download Magento 2.3 via Composer**

First we need to navigate to the web directory of our web server

    cd /var/www/html

Switch from out superuser to the magento user

    su magento

In order for Composer to work, it needs to be ran from within an empty directory.

Check all file and folder inside the directory

    ls -la

Delete that test Apache file

    rm index.html

Tell composer to install Magento 2.3.5 
This insignificant looking dot tells composer to install it in the same directory from where we are running the command from. Missing this dot will cause Magento to install somewhere else.

    composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition=2.3.5 .

During the setup, you will be asked for a Username and a Password. Just to be clear, **Username = Public Key** and **Password = Private Key** what we created on the Magento website.

**Set pre-installation permissions**

Make sure not to miss this step.

    find var generated vendor pub/static pub/media app/etc -type f -exec chmod g+w {} + && find var generated vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} + && chown -R :www-data . && chmod u+x bin/magento

**Create a database for installing Magento 2.3**

**Set up a user & database**

 Set up an isolated MySQL User than only has access to one specific database that it owns.
 
 - So visit  **HTTP://<your_server>/phpmyadmin**;
 - Log in with your MySQL root credentials;
 - Once logged in, click  **User Accounts**;
 - Click  **Add user account**;
 - Enter a username;
 - Change Host name to Local;
 - Generate a password and write it down safe;
 - Click the box below labelled  **“Create database with same name and
   grant all privileges.”**, which will conveniently create a database
   for us at the same time as creating the user; Hit  **Go**  at the
   bottom-right of the page;

   
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
