# **How to set Reverse Proxy in Server**

## What is Reverse Proxy ?

- A reverse proxy server is an intermediate connection point positioned at a network’s edge. It receives initial HTTP connection requests, acting like the actual endpoint.

- The reverse proxy serves as a gateway between users and your application origin server. In so doing it handles all policy management and traffic routing.

- A reverse proxy operates by :
  - Receiving a user connection request
  - Connecting with the origin server and forwarding the original request

![ReverseProxy](./image/reverse-proxy-02-1.jpg)

- Revere Proxy benefits
  - Load Balancing
  - Protection from attacks
  - caching
  - SSL encryption

## Implement Reverse Proxy for project

If you have to run proxy for project then you have to create .conf file in server.

sample Reverse proxy file :-

```bash
<VirtualHost *:443>
    ServerName www.abc.com = Your project subdomain name


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
    ProxyPass / www.abc.com/ = Your project subdomain name 
    ProxyPassReverse / www.abc.com/ = Your project subdomain name


    ErrorLog ${APACHE_LOG_DIR}/ldf-error.log
    CustomLog ${APACHE_LOG_DIR}/ldf-access.log combined
```

Then, You have to enable conf file and restart apache server

```bash
# sudo apache2ctl configtest
```

```bash
# sudo a2ensite your_domain.conf
```

If you disable any other conf file then use

```bash
# sudo a2dissite Your_conf_file
```

If you done any change in conf file you must have to restart apache server  

```bash
# sudo systemctl restart apache2
```
