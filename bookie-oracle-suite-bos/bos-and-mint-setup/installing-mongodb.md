# Installing MongoDB

MongoDB is a NoSQL database that has fully flexible index support and a rich queries database. 

This document explains how to install MongoDB \(as `root/sudo`\).

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
sudo apt install -y mongodb
```

If you want to install a specific version of MongoDB, define the version number as follows:

```text
sudo apt install mongodb-org=4.2.1 mongodb-org-server=4.2.1 mongodb-org-shell=4.2.1 mongodb-org-mongos=4.2.1 mongodb-org-tools=4.2.1
```

### Step 3 – Manage MongoDB Service

After installation, MongoDB will start automatically. To start or stop MongoDB use an init script. For example:

```text
sudo systemctl enable mongod
sudo systemctl start mongod 
```

And use the following commands to stop or restart the MongoDB service.

```text
sudo systemctl stop mongod
sudo systemctl restart mongod 
```

### Step 4 – Verify MongoDB Installation

Finally, use the below command to check the installed MongoDB version on your system.

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

And check the status with:

```text
sudo service mongod status
```

{% hint style="danger" %}
**Important**:  Some versions have the service name as `mongod` and some have `mongodb.` If you get an error with the above command, use `sudo service mongodb status` instead.
{% endhint %}

### Step 5 - Test MongoDB

Also, connect MongoDB using the command line and execute some test commands for checking proper working.

```text
mongo 

> use mydb;

> db.test.save( { tecadmin: 100 } )

> db.test.find()

  { "_id" : ObjectId("52b0dc8285f8a8071cbb5daf"), "tecadmin" : 100 }
```

