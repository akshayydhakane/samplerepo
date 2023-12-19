# Node.Js Deployment Setup

Node.js is a JavaScript runtime for server-side programming. It allows developers to create scalable backend functionality using JavaScript, a language many are already familiar with from browser-based web development

## Installing Node.js

```text
# sudo apt update  
# sudo apt install nodejs  
# sudo apt install npm
```

- checking node version

```text
# node -v
```

## Installing Node Version Manager NVM

```text
# curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh
# curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
# source ~/.bashrc
```

- list all node version

```text
nvm list-remote
```

- Now you can install any of this node version using this command  

```text
nvm install v16.13.1
```

![NODE](./image/nvm-install.jpg)

- You can see all installed node version using  

```text
nvm list
```

![NODE](./image/nvm-ls.jpg)

- Now you installed so many version of node then you have to switch to different version then  

```text
# nvm use v14.10.0
```

## Configure MongoDB for database connection

- Create Admin User

```text
# mongo
```

```text
# use DBname 
```

```text
# db.createUser({user:"username",pwd:"password",roles:["readWrite","dbAdmin","dbOwner"]})
```

```text
# db.auth('username','passwd')
```

## Deploying Node Project

- ### Clone The Project To Your Working Path

```text
# git clone 
```

```text
# cd /home/node
```

- ### Node Project Need .env file or need config.json file to add credential

- ### Create All Credential And Add In config.json file or add .env file Depends On Project

```text
# 1. mongodb  : DataBase, User, Password,
# 2. redis    : DB=NO. 
# 3. rebbitMQ : User, Password, VirtualHost
```

- ### Install Node_modules

```text
# npm install
```

```text
# node start app.js/index.js
```

- ### Using Pm2 :-

- If you have to use different node version then use this command,

```text
# npm install pm2@latest -g
```

```text
# pm2 install pm2-logrotate
```
- Start Pm2 for project

```text
# pm2 start index.js
```

- List all pm2 :-

```text
# pm2 list/ls
```

- ### Managing Pm2 :-

```text
# pm2 restart app_name/id
# pm2 reload app_name/id
# pm2 stop app_name/id
# pm2 delete app_name/id
```

- Display pm2 Logs :-

```text
# pm2 logs
```

## CI/CD For Node Deployment

```yml
deploy_dev:
  stage: deploy
  tags:
    - dev - gitlab-runner tag
  environment:
    name: development
    url: https://dev.artoon.com:3000/ = Node-port
  before_script:
     - eval export PATH=/root/.nvm/versions/node/v15.10.0/bin:${PATH}
  script:
    - cd /home/node/project - project path
    - git pull http://$usr:$pwd@gitlab.artoon.in/project/project-demo.git -f development
    - npm install --cache
    - pm2 restart index = pm2 app name
  when: manual
  only:
    - development
```
