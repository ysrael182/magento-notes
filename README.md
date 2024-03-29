# magento-notes
## Install LAMP (UBUNTU)
### Install APACHE
First of all, we need to Apache, the version recommended to Magento 2.x it's the apache 2.4
```
    sudo apt -get update
    sudo apt -get install apache2
```
Add the ServerName, into the file /etc/apache2/apache2.conf , the server name can be the ip or the localhost.
For example:
```
ServerName localhost
```
Test if everything is ok,
```
sudo apache2ctl configtest
```
And finally restart the apache server.
```
sudo systemctl restart apache2
```
test with the URL http://localhost in the browser.

### Install Mysql
The version recommended to Magento 2, it's 5.6 or 5.7
```
apt-get -y install mysql-server mysql-client
```
it's highly recommended add a password to the root user.

If in case the installation did not ask for password and a you can not enter to mysql use:
```
sudo mysqld_safe --skip-grant-tables&
sudo mysql --user=root mysql
mysql> update user set authentication_string=PASSWORD('new-password') where user='root';
flush privileges;
quit
sudo service mysql restart
sudo mysql -u root -p
exit
#reset the default password of root
```

Then, whether no solutions have not worked to change the password, you can use the following (mysql version > 8)

```
sudo mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';
sudo service mysql stop
sudo service mysql start
```
### Install Php
The version recommended of Php to Magento 2, it's PHP 7.0.6 - 7.0.x and PHP 7.1.x.
Add the repository
```
sudo apt-add-repository ppa:ondrej/php
sudo apt-get update
```
Install php and depedencies.
```
sudo apt install php7.1 libapache2-mod-php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-ldap php7.1-zip php7.1-curl php7.1-bcmath
```
### Install PhpMyadmin
```
sudo apt-get install phpmyadmin php-mbstring php-gettext
```
Enable the extensions
```
    sudo phpenmod mcrypt
    sudo phpenmod mbstring
```

And Restart Apache
```
sudo systemctl restart apache2
```
Note: If the phpmyadmin's url is not available, you must to create a symlink:
```
sudo ln -s /usr/share/phpmyadmin /var/www/html
```
### Enable .htaccess Apache2
Firt, enable the rewrite with the following command:
```
sudo a2enmod rewrite
```
Then, restart the apache:
```
sudo service apache2 restart
```
Finally, you must edit the file: /etc/apache2/apache2.conf, with the next information:

```
AllowOverride none to AllowOverride All
```

### Enable vhost 
First step copy the default 
```
sudo cp /etc/apache2/sites-available/example.com.conf /etc/apache2/sites-available/test.com.conf
```
After that, add the serverName "test.com.conf" into the host file
```
127.0.0.1              test.com.conf
```
Enable the vhost
```
sudo a2ensite test.com.conf
```
vhost for multiple websites
```
<VirtualHost *:80>
    ServerName example.com
    ServerAlias foo.example.com bar.example.com others.example.com
    DocumentRoot /www/redirect_folder 
</virtualhost>
```

### Swicth Php version
If you want to have multiple versions of php and swicth between versions (apache2), run:
```
sudo a2dismod php7.2
sudo a2enmod php7.0
sudo service restart apache2
```
Change the version of php cli:
```
sudo update-alternatives --set php /usr/bin/php7.0
```
### Permissions Magento
```
sudo usermod -g www-data name_of_user
sudo find var vendor pub/static pub/media app/etc -type f -exec chmod g+w {} \;
sudo find var vendor pub/static pub/media app/etc -type d -exec chmod g+ws {} \;
```
