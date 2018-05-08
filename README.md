# magento-notes
## Install LAMP
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
### Install Php
The version recommended of Php to Magento 2, it's PHP 7.0.6 - 7.0.x and PHP 7.1.x.
Add the repository
```
sudo apt-add-repository ppa:ondrej/php
sudo apt-get update
```
Install php and depedencies.
```
sudo apt install php7.1 libapache2-mod-php7.1 libapache2-mod-php7.1 php7.1-common php7.1-mbstring php7.1-xmlrpc php7.1-soap php7.1-gd php7.1-xml php7.1-intl php7.1-mysql php7.1-cli php7.1-mcrypt php7.1-ldap php7.1-zip php7.1-curl
```
