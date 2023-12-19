# **Deploy ReactJS/Angular Project**

## Clone Git Project To Server

```bash
Project path - /var/www/html/
# git clone <Git Url>
```

## Install Node Modules

- Go To Project path

```bash
- select specific node version & apply 
```

```bash
# npm install
```

- Create ReactJs Project Build

```bash
# npm run build
# mv build build_live
```

- Create Angular Project Build

```bash
# npm run build
# mv dist build_live
```

- Create htaccess file in project path

```bash
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /subdirectory
RewriteRule ^index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-l
RewriteRule . /index.html [L]
</IfModule>
```

## Create VirtualHost File

- Go to virtual host path

```bash
# /etc/apache/sites-available/
```

```bash
Create .conf file  
```

- Sample Virtual host :-

```bash
<VirtualHost *:80>
    ServerAdmin jay.zalavadiya@artoon.in
    ServerName cscallbreakcms.artoon.in
    DocumentRoot /var/www/html/dev/callbreak-react-cms/build_live
    RedirectPermanent / https://cscallbreakcms.artoon.in
    ErrorLog ${APACHE_LOG_DIR}/callbreak-react-cms-error.log
    CustomLog ${APACHE_LOG_DIR}/callbreak-react-cms-access.log combined
</VirtualHost>

<VirtualHost *:443 >
    ServerAdmin jay.zalavadiya@artoon.in
    DocumentRoot /var/www/html/dev/callbreak-react-cms/build_live
    ServerName cscallbreakcms.artoon.in
SSLEngine on
SSLCertificateFile /etc/ssl/final.crt
SSLCertificateKeyFile /etc/ssl/server.key
SSLCertificateChainFile /etc/ssl/intermediate.crt


    <Directory /var/www/html/dev/callbreak-react-cms/build_live >
                Options FollowSymLinks MultiViews
                AllowOverride All
                Order allow,deny
                allow from all
        </Directory>
    ErrorLog ${APACHE_LOG_DIR}/callbreak-react-cms-error.log
    CustomLog ${APACHE_LOG_DIR}/callbreak-react-cms-access.log common
</VirtualHost>
```

Then

```bash
# apache2ctl configtest
# a2ensite (project configfile name)
```

```bash
# systemctl apache2 restart  
```

## Create Reverse Proxy Virtual Host For Project in 172.20.12.5 server

```bash
<VirtualHost :443>
    ServerName ldf.artoon.in


    <Proxy *>
        Order deny,allow
        Allow from all
    </Proxy>


SSLEngine on
SSLProxyEngine on
SSLProxyVerify none
SSLProxyCheckPeerCN off
SSLProxyCheckPeerName off
SSLProxyCheckPeerExpire off


#    SSLCertificateFile /etc/ssl/artoon.crt
#    SSLCertificateKeyFile /etc/ssl/artoon.key
SSLCertificateFile /etc/ssl/final.crt
SSLCertificateKeyFile /etc/ssl/server.key
SSLCertificateChainFile /etc/ssl/intermediate.crt


    ProxyRequests Off
    ProxyPreserveHost On
    ProxyPass / https://ldf.artoon.in/
    ProxyPassReverse / https://ldf.artoon.in/


    ErrorLog ${APACHE_LOG_DIR}/ldf-error.log
    CustomLog ${APACHE_LOG_DIR}/ldf-access.log combined 
```

```bash
# systemctl apache2 restart 
# systemctl apache2 reload 
```

```bash
# Point DNS Record With That Sub-Domain
```

## React Project CI/CD  

- Create gitlab-runner for CI/CD

```bash
# sudo gitlab-runner register
```

```yml
deploy_dev:
  stage: deploy
  tags: 
    - new-dev
  environment:
    name: development
    url: https://3gamescms.artoon.in/
  before_script:
    - eval export PATH=/root/.nvm/versions/node/v14.18.0/bin:${PATH}
  script:
    - npm install --cache
    - cp /var/www/html/dev/mgp-3games-cms/.env.dev .
    - CI= npm run-script build:dev
    - cp -r build/* /var/www/html/dev/mgp-3games-cms/build_live/
    - cp /var/www/html/htaccess /var/www/html/dev/mgp-3games-cms/build_live/.htaccess
  when: manual
  only:
    - development
```
