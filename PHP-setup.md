# **PHP Setup In Ubuntu With Multiple Version**

## PHP

```bash
# sudo apt update  
# sudo apt upgrade  
```

```bash
# sudo apt-get install software-properties-common -y
# sudo add-apt-repository ppa:ondrej/php
```

```bash
# sudo apt update 
```

install specific php version

```bash
# sudo apt install php8.1  
# sudo apt install php7.4  
# sudo apt install php5.6  
```

Installing PHP Versions 8.1 && 7.2 with PHP-FPM

```bash
# sudo apt-get install php8.1 php8.1-fpm php8.1-mysql libapache2-mod-php8.1
```

```bash
# sudo apt-get install php7.2 php7.2-fpm php7.2-mysql libapache2-mod-php7.2 
```

- php8.1-fpm provides the Fast Process Manager
- php8.1-mysql connects PHP to the MySQL database  
- libapahce2-mod-php8.1 provides the PHP module for the Apache webserver  

After install both php-fpm version check service :-

```bash
# sudo systemctl start php8.1-fpm
# sudo systemctl status php8.1-fpm
```

```bash
Output
● php8.1-fpm.service - The PHP 8.1 FastCGI Process Manager
   Loaded: loaded (/lib/systemd/system/php8.1-fpm.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2020-03-29 12:53:23 UTC; 15s ago
     Docs: man:php-fpm8.1(8)
  Process: 20961 ExecStopPost=/usr/lib/php/php-fpm-socket-helper remove /run/php/php-fpm.sock /etc/php/8.1/fpm/pool.d/www.conf 70 (code=exited,
  Process: 20979 ExecStartPost=/usr/lib/php/php-fpm-socket-helper install /run/php/php-fpm.sock /etc/php/8.1/fpm/pool.d/www.conf 70 (code=exite
 Main PID: 20963 (php-fpm8.1)
   Status: "Processes active: 0, idle: 2, Requests: 0, slow: 0, Traffic: 0req/sec"
    Tasks: 3 (limit: 1150)
   CGroup: /system.slice/php8.1-fpm.service
           ├─20963 php-fpm: master process (/etc/php/8.1/fpm/php-fpm.conf)
           ├─20977 php-fpm: pool www
           └─20978 php-fpm: pool www
```

By defaulf server use latest php version

```bash
# sudo update-alternatives --config php
```

using this command you can switch to other version

There are 4 choices for the alternative php (providing /usr/bin/php).

```bash
  Selection    Path             Priority   Status
------------------------------------------------------------
* 0            /usr/bin/php8.1   81        auto mode
  1            /usr/bin/php5.6   56        manual mode
  2            /usr/bin/php7.4   74        manual mode
  3            /usr/bin/php8.1   81        manual mode

Press  to keep the current choice[*], or type selection number: 2
```

Some file need other extension for php so add extension using this command

```bash
# sudo apt install php8.1-[extension]
# sudo apt install php8.1-mysql php8.1-mbstring php8.1-xml php8.1-curl  
```

If you add multiple extension then use command like this,

```bash
# sudo apt install php8.1-mysql php8.1-mbstring php8.1-xml php8.1-curl  
```

Main PHP configuration file location :-

```bash
# cd /etc/php/8.1
```

installed PHP modules are stored under :-

```bash
# cd /etc/php/8.1/mods-available  
```

Now,uninstall php version,

```bash
# sudo apt remove php8.1
```

if you also remove all other php modules then use this command,

```bash
# sudo apt remove php8.1*  
```
