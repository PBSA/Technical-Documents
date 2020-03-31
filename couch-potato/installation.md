# Installation

This document explains the  Couch Potato server installation as both "The long way" and "The short way"

The document does assume some prior knowledge of simple web server installation.

## The Stack

Couch Potato front-end is an Ionic web application using the Angular framework. It interfaces with the back-end through a PHP API that provides connectivity to a MySQL database,

![](../.gitbook/assets/blank-diagram-7.png)

Although the diagram above shows an Apache server, other servers compatible with PHP and MySQL could be used, such as Nginx.

## Installing - The Long Way

These short steps show how to install all the back-end components and dependencies.

### Step 1 - Install a PHP / MySQL Server

There are several open source PHP stacks readily available that are by far the easiest way of getting set up and include MySQL as well. The most popular being WAMP and LAMP. 

These stacks, and installation instructions,  are readily available for download from many sources and won't be covered any further here.

Step 2 - Install PHP Dependencies.

The Couch Potato API uses a couple of libraries that aren't part of the standard installation and need to be loaded separately.







