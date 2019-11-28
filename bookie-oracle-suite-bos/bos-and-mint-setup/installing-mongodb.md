# Installing MongoDB

MongoDB is a fully flexible index support and rich queries database. Mongodb is a NoSQL database. MongoDB provides large media storage with GridFS. Click [here](https://docs.mongodb.com/v4.2/release-notes/4.2/) for more details about this version of MongoDB.

This tutorial will help you to install MongoDB 4.2 community release on Ubuntu 18.04 LTS \(Bionic\) and 16.04 LTS \(Xenial\).

### Step 1 – Setup Apt Repository

First of all, import GPK key for the MongoDB apt repository on your system using the following command. This is required to test packages before installation

```text
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 4B7C549A058F8B6B
```

Then add MongoDB APT repository `url in /etc/apt/sources.list.d/mongodb.list.`

**Ubuntu 18.04 LTS:**

```text
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb.list
```

### Step 2 – Install MongoDB on Ubuntu

After adding required APT repositories, use the following commands to install MongoDB on your systems. It will also install all dependent packages required for MongoDB.

```text
sudo apt update
sudo apt install mongodb-org
```

If you want to install any specific version of MongoDB, define the version number as below

```text
sudo apt install mongodb-org=4.2.1 mongodb-org-server=4.2.1 mongodb-org-shell=4.2.1 mongodb-org-mongos=4.2.1 mongodb-org-tools=4.2.1
```

### Step 3 – Manage MongoDB Service

After installation, MongoDB will start automatically. To start or stop MongoDB uses init script. Below are the example commands to do.

```text
sudo systemctl enable mongod
sudo systemctl start mongod 
```

Use the following commands to stop or restart MongoDB service.

```text
sudo systemctl stop mongod
sudo systemctl restart mongod 
```

* How to Work with MongoDB – [Read this tutorial](https://tecadmin.net/tutorial/mongodb/mongodb-tutorials/)

### Step 4 – Verify MongoDB Installation

Finally, use the below command to check installed MongoDB version on your system.

```text
mongod --version 

db version v4.2.1
git version: edf6d45851c0b9ee15548f0f847df141764a317e
OpenSSL version: OpenSSL 1.1.1  11 Sep 2018
allocator: tcmalloc
modules: none
build environment:
    distmod: ubuntu1804
    distarch: x86_64
    target_arch: x86_64
```

Also, connect MongoDB using the command line and execute some test commands for checking proper working.

```text
mongo 

> use mydb;

> db.test.save( { tecadmin: 100 } )

> db.test.find()

  { "_id" : ObjectId("52b0dc8285f8a8071cbb5daf"), "tecadmin" : 100 }
```

* [How to Create and Drop database in MongoDB](https://tecadmin.net/create-and-drop-database-in-mongodb/)

SHARE IT![Share on](https://www.facebook.com/share.php?u=https://tecadmin.net/install-mongodb-on-ubuntu/&title=How+to+Install+MongoDB+4.2+on+Ubuntu+18.04+%26%23038%3B+16.04+via+PPA)

