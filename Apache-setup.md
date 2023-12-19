# How To Install Apache web server In Ubuntu

## Apache

```bash
# sudo apt update
# sudo apt install apache2
```

You can verify from this command  

```bash
# sudo ufw app list
# sudo ufw status
```

Allow Firewall For Web Traffic  

```bash
# sudo ufw allow "Apache Full"
```

Check Apache port and other info  

```bash
# sudo ufw app info "Apache Full"
```

Now Check your web server  

```bash
# sudo systemctl status apache2
```

```bash
Output
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2020-04-23 22:36:30 UTC; 20h ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 29435 (apache2)
      Tasks: 55 (limit: 1137)
     Memory: 8.0M
     CGroup: /system.slice/apache2.service
             ├─29435 /usr/sbin/apache2 -k start
             ├─29437 /usr/sbin/apache2 -k start
             └─29438 /usr/sbin/apache2 -k start
```

- Apache is configured to start automatically when the server boots. If this is not what you want  

```bash
# sudo systemctl disable apache2
# sudo systemctl enable apache2
```

- let’s go over some basic management commands using systemctl  

```bash
# sudo systemctl restart apache2
# sudo systemctl start apache2
# sudo systemctl stop apache2
# sudo systemctl reload apache2
# sudo systemctl status apache2
```

## Setup Virtual Host in Apache

- Path of your project folder

```bash
# cd /var/www/html/Your_project_folder  
```

- Then after you have to give permission to your project  

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

- Then you have to create config file for project  

```bash
# cd nano /etc/apache2/sites-available/your_domain.conf
```

**demo conf file** :-

```bash
<VirtualHost *:80>
    ServerAdmin your.email@artoon.in
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com
    <Directory /var/www/example.com>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    <IfModule mod_dir.c>
        DirectoryIndex index.php index.pl index.cgi index.html index.xhtml index.htm
    </IfModule>
</VirtualHost>
```

- Then check your configuration error  

```bash
# sudo apache2ctl configtest
```

OUTPUT = Syntax OK

- Next, enable your conf file  

```bash
# sudo a2ensite your_domain.conf
```

- If you disable any other conf file then use

```bash
# sudo a2dissite Your_conf_file
```

- If you done any change in conf file you must have to restart apache server  

```bash
# sudo systemctl restart apache2
```

### Next, Add host entry in your local system for check your website  

```bash
# cd nano /etc/hosts 
```

- Now you can check with your domain name  

  - **<http://your_domain>**
