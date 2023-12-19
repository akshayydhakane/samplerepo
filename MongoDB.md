# **MongoDB Setup On Ubuntu**

- Install gnupg and its required libraries using the following command:

```bash
# sudo apt-get install gnupg
```

- Once installed, retry importing the key:

```bash
# wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```

- Create the /etc/apt/sources.list.d/mongodb-org-6.0.list file for Ubuntu 20.04 (Focal):

```bash
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

```bash
# sudo apt-get update
```

- Install the latest stable version

```bash
# sudo apt-get install -y mongodb-org
```

- After installation the data directory /var/lib/mongodb and the log directory /var/log/mongodb are created  

- Mongo DB configuration file path

```bash
# /etc/mongo.conf
```

- Start MongoDB

```bash
# sudo systemctl start mongod

# sudo systemctl stop mongod

# sudo systemctl status mongod

# sudo systemctl restart mongod
```

- If you receive an error similar to the following when starting mongod

```bash
# sudo systemctl daemon-reload
```

- Using this command you dont have to start mongod service everytime when system reboot

```bash
# sudo systemctl enable mongod
```

- Begin using MongoDB

```bash
# mongo
```

- Create An admin user before we enable admin authentication

## Create Admin User

```bash
# mongo
```

```bash
# use admin
```

```bash
db.createUser(
  {
    user: "admin",
    pwd: "abc123",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```

```bash
# db.auth('admin','passwd')
```

## Enable MongoDb Authentication

```bash
# nano /etc/mongod.conf
```

```bash
net:
    bindIp: 0.0.0.0   //for Out Side access
    port: 27017
security:
    authorization: "enabled"     //for admin authentication
```

- Create New DB or Switch to Existing DB

```bash
> use local
switched to local
```

- Create New User for Current db

```bash
db.createUser({user:"local",pwd:"password",roles:["readWrite","dbAdmin","dbOwner"]})
```

```bash
db.auth('local','password')
```

- Check the DB currently in use

```bash
> db
local
```

- Drop Database : The given command helps the user to drop the required database.

```bash
> use local
switched to db local

> db.dropDatabase()
  { "dropped" : "local", "ok" : 1 }
```

- Create Mongodb Backup

```bash
# mongodump --db databasename --host 127.0.0.1 --port 27017 --username admin --password userpasswd --authenticationDatabase admin
```

- Mongodb Restore command

```bash
# mongorestore --host 127.0.0.1 -d database_name --username username   /path/to/backupfile/
```

- Create mongodb Table Dump

```bash
# mongodump --db dbname --collection=table_name --out=data/ --host 127.0.0.1 --port 27017 --username admin --password password --authenticationDatabase admin
```
