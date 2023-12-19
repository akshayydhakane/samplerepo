# **Setup LAMP Stack On Ubuntu Server**

## Step 1 - Installing Apache

```bash
# sudo apt update
```

```bash
# sudo apt install apache2
```

```bash
#sudo ufw app list
```

```bash
# sudo ufw allow in "Apache"
```

```bash
# sudo ufw status
```

## Step 2 - Installing PHP

```bash
# sudo apt install php libapache2-mod-php php-mysql
```

- Install software-properties-common:
- The software-properties-common package provides apt-add-repository command-line utility which you will use to add the ondrej/php PPA repository.

```bash
# sudo apt-get install software-properties-common -y
```

```bash
# sudo add-apt-repository ppa:ondrej/php
```

```bash
# sudo apt-get update -y
```

```bash
# sudo apt-get install php7.2 php7.2-fpm php7.2-mysql libapache2-mod-php7.2
```

```bash
# sudo apt-get install php7.3 php7.3-fpm php7.3-mysql libapache2-mod-php7.3
```

```bash
# sudo systemctl start php7.3-fpm
```

Check PHP version

```bash
# sudo update-alternatives --config php
```

There are 4 choices for the alternative php (providing /usr/bin/php).

```bash
  Selection    Path             Priority   Status
------------------------------------------------------------
* 0            /usr/bin/php8.1   81        auto mode
  1            /usr/bin/php7.2   56        manual mode
  2            /usr/bin/php7.3   74        manual mode

Press  to keep the current choice[*], or type selection number: 1
```

## Step 3 — Installing MySQL

```bash
# sudo apt install mysql-server
```

```bash
# sudo mysql_secure_installation
```

```bash
OUTPUT:
Answer Y for yes, or anything else to continue without enabling.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:
```

```bash
OUTPUT:
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
```

```bash
Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
```

```bash
# sudo mysql
```

```bash
Output
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 22
Server version: 8.0.19-0ubuntu5 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

- Create a new database

```bash
mysql> CREATE DATABASE example_database;
```

- creates a new user named example_user, using mysql_native_password as default authentication method.

```bash
mysql> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```

- Now we need to give this user permission over the example_database database:

```bash
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
```

- connect to the MySQL console using the root account:

```bash
mysql> mysql -u example_user -p
```

```bash
mysql> SHOW DATABASES;
```

- Now exit the MySQL shell with:

```bash
mysql> exit
```

## Step 4 — Creating a Virtual Host for your Website

```bash
# cd /var/www/html/Your_project_folder  
```

```bash
# sudo chmod -R 755 /var/www/html/Your_project_folder  
```

Next,

- create demo file for run apache server  

```bash
# sudo nano /var/www/html/Your_project_folder/index.html  
```

```html
<html>
    <head>
        <title>Welcome to Your_domain!</title>
    </head>
    <body>
        <h1>Success!  The your_domain virtual host is working!</h1>
    </body>
</html>
```

### Then you have to create config file for project  

```bash
# cd nano /etc/apache2/sites-available/your_domain.conf
```

- demo conf file :-

```bash
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    ServerName your_domain
    ServerAlias www.your_domain
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### Then check your configuration error  

```bash
# sudo apache2ctl configtest
```

```bash
OUTPUT = Syntax OK
```

### Next, enable your conf file  

```bash
# sudo a2ensite your_domain.conf
```

- If you done any change in conf file you must have to restart apache server  

```bash
# sudo systemctl restart apache2
```
