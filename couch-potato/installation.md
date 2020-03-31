# Installation

This document explains the  Couch Potato server installation as both "The long way" and "The short way"

The document does assume some prior knowledge of simple web server installation.

## The Stack

Couch Potato front-end is an Ionic web application using the Angular framework. It interfaces with the back-end through a PHP API that provides connectivity to a MySQL database,

![](../.gitbook/assets/blank-diagram-7.png)

Although the diagram above shows an Apache server, other servers compatible with PHP and MySQL could be used, such as Nginx.

## Installing - The Long Way

These short steps show how to install all components and dependencies.

### Step 1 - Install a PHP / MySQL Server

There are several open source PHP stacks readily available that are by far the easiest way of getting set up and include MySQL as well. The most popular being WAMP and LAMP. 

These stacks, and installation instructions,  are readily available for download from many sources and won't be covered any further here.

### Step 2 - Update versions

At the time of writing the PHP and SQL versions used are:

* PHP 7.2
* MySQL 5.7

After installing PHP and MySQL if your versions aren't at least as new as these then they need to be updated.

The process for updating varies according to the operating system, but if PHP and MySQL were installed as a stack in Step 1, such as LAMP, then as long as that was the most recent version there shouldn't be an issue with old versions of PHP or MySQL.

### Step 3 - Install PHP Dependencies.

The Couch Potato API additional libraries that either aren't part of the standard installation and need to be loaded, or are in PHP but haven't been enabled.

#### mysqli

This is the PHP-&gt;MySQL library used by the API. For installation instructions see:

{% embed url="https://www.php.net/manual/en/mysqli.installation.php" %}

### Step 4 - Run MySQL Script

A script will be provided to all new Couch Potato operators that will  create the database schema and pre-populate the database with all the starting data.

{% hint style="danger" %}
**Important**: After the script is run you should have a new database schema called `couch_potato`. To avoid any issues or additional configuration changes don't change the database name.
{% endhint %}

Run the script on the MySQL database instance created in the previous steps.

### Step 5 - Add Database Connection to API

The PHP API must be updated with the correct database connection credentials that were used to create the database. To do this some environment variables need to be changed as follows:

Open the `.env` file from the root location where the PHP API was loaded.

```text
DB_HOST=" "
DB_USER=""
DB_PASS=""
DB_NAME="couch_potato"
```

Next update the `DB_HOST`, `DB_USER` and `DB_PASS` values to those of your database.

### Step 6 - Installing the User Interface

Installing the web components is very simple but does depend where you're planning on hosting the website.

So assuming you have a suitable web server/directory already set up then copy all the files from the `www` folder to the web server / webapp folder, or copy the entire `www` folder.

The `www` folder can be found here:

TBD

### Step 7 - Load User Interface Dependencies

There is one additional dependency that needs to be added to the web server to support the cryptography library that Couch Potato uses.

From the console run:

```text
npm install crypto-js
```

### Step 8 - Configure User Interface 

To configure the application to use the API and other options open the config.json file from the www/assets folder.

```text
{
    "api_url": "https://localhost/couch-potato/",
    "notifications": 
            {
                "delay": 2000,
                "start": 36,
                "end": 240
            },
    "version": "v1.0.0",
    "title1": "Couch",
    "title2": "Potato",
    "logolarge": "/assets/imgs/couch-potato-main.png",
    "logosmall": "/assets/imgs/couch-potato.png"
}

```

In this file you can change the following attributes:

| Name | Description |
| :--- | :--- |
| api\_url | The URL for the Couch Potato API, see above steps. |
| notifications: delay | The time in milliseconds at which the game notifications are refreshed. See [Notifications](help/user-guide/dashboard/#notifications) |
| notifications: start | The number of hours that the notifications report back. For example, 36 means that the notifications will report on any games that are up to three days old. |
| notifications: end | The number of hours that the notifications report ahead. For example, 240 means that the notifications will report on any games that are up to 10 days away. |
| title1 | For white-labelling purposes the title can be customized to any text |
| title2 | See `title1` |
| logolarge | For white-labelling purposes the large logo can be changed to any valid URL |
| logosmall | Same as `logolarge` but for the smaller logo. |











