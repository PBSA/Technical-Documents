# BOS Installation

## Installation of bos-auto

In this first step, we'll install everything we'll need going forward.

### Install Dependencies

{% hint style="warning" %}
**Note**: Dependencies must be installed as `root/sudo`
{% endhint %}

```text
apt-get install libffi-dev libssl-dev python-dev python3-dev python3-pip libsecp256k1-dev
pip3 install virtualenv
```

{% hint style="info" %}
**Tip**: `virtualenv` is a best practice for python, but installation can also be on a user/global level.
{% endhint %}

### Install MongoDB

MongoDB is used for persistent storage within BOS.

{% page-ref page="installing-mongodb.md" %}

For  additional information on how to use MongoDB refer to tutorials on your distribution.

{% hint style="danger" %}
**Important**: Make sure that the MongoDB is running reliably with automatic restart on failure.
{% endhint %}

### **Install Redis**

Redis is used as an asynchronous queue for the python processes in BOS.

{% page-ref page="installing-redis.md" %}

For additional information on how to install Redisdb refer to  your Linux distribution.

{% hint style="danger" %}
**Important**: Make sure that RedisDB is running reliably with automatic restart on failure, and that it's run without any disk persistence.
{% endhint %}

### Starting MongoDB and Redis Daemons

It is highly recommended that both daemons are started on start-up.

```text
systemctl enable mongod
systemctl enable redis
```

To start the deamons, execute

```text
systemctl start mongod
systemctl start redis
```

{% hint style="danger" %}
**Important: Common Issues:** 

**Exception**: _Can’t save in background: fork or MISCONF Redis is configured to save RDB snapshots._

This indicates that either your queue is very full and the RAM is insufficient, or that your disk is full and the snapshot can’t be persisted. 

Create your own Redis configuration file \([https://redis.io/topics/config](https://redis.io/topics/config)\) and use it to deactivate caching and activate overcommit memory:

[https://redis.io/topics/faq\#background-saving-fails-with-a-fork-error-under-linux-even-if-i-have-a-lot-of-free-ram](https://redis.io/topics/faq#background-saving-fails-with-a-fork-error-under-linux-even-if-i-have-a-lot-of-free-ram) or [https://stackoverflow.com/questions/19581059/misconf-redis-is-configured-to-save-rdb-snapshots/49839193\#49839193](https://stackoverflow.com/questions/19581059/misconf-redis-is-configured-to-save-rdb-snapshots/49839193#49839193)

[https://gist.github.com/kapkaev/4619127](https://gist.github.com/kapkaev/4619127)

**Exception**: _IncidentStorageLostException: localhost:27017: \[Errno 111\] Connection refused or similar._ 

This indicates that your MondoDB is not running properly. Check your MongoDB installation.
{% endhint %}

### Installing bos-auto

{% hint style="warning" %}
**Note**: bos-auto must be installed as `user`
{% endhint %}

You can either install bos-auto via `pypi / pip3` \(production installation\) or via git clone \(debug installation\). 

For production using install bos-auto via pip3 is recommended, but the git master branch is always the latest release as well, making both installations equivalent. Recommended is a separate user.

```text
cd ~
mkdir bos-auto
cd bos-auto
# create virtual environment
virtualenv -p python3 env
# activate environment
source env/bin/activate
# install bos-auto into virtual environment
pip3 install bos-auto
```

For debug use, checkout from Github \(master branch\) and install dependencies manually.

{% embed url="https://github.com/peerplays-network/bos-auto" %}

```text
cd ~
# checkout from github
git clone https://github.com/peerplays-network/bos-auto
cd bos-auto
# create virtual environment
virtualenv -p python3 env
# activate environment
source env/bin/activate
# install dependencies
pip3 install -r requirements.txt
```

BOS auto is supposed to run in the virtual environment. Either activate it beforehand, as above, or run it directly in the `env/bin` folder.

{% hint style="danger" %}
**Important**: If bos-auto is installed as `root` and not `user` then you'll likely get errors similar to the following:
{% endhint %}

![](../../.gitbook/assets/photo_2019-12-10-13.31.47.jpeg)

### Upgrading bos-auto

For production installation, upgrade to the latest version - including all dependencies using:

```text
pip3 install --upgrade --upgrade-strategy eager bos-auto
```

For debug installation, pull latest master branch and upgrade dependencies manually

```text
git pull
pip3 install -r requirements.txt --upgrade --upgrade-strategy eager
```

## Configuration of bos-auto

Next we need to go through the  steps required to setup bos-auto properly.

{% page-ref page="configuration-of-bos-auto.md" %}

## Spinning Up bos-auto

After bos-auto configuration we need to spin-up bos-auto to see if it works properly. 

{% page-ref page="spinning-up-bos-auto.md" %}

## **Manual Intervention \(MINT\)**

Bos-mint is a web-based manual intervention module that allows you to work with all sorts of manual interactions with the blockchain. 

For more information see:

{% page-ref page="../introduction-to-mint/" %}

## Monitoring bos-auto

The isalive call should be used for monitoring. The scheduler must be running, and the default queue a low count \(&lt; 10\).

Here is an example of a positive `isalive` check:

```text
 {
   "background": {
      "scheduler": True
   },
   "queue": {
      "status": {
         "default": {
            "count": 0
         },
         ...
      }
   },
   ...
}
```

## Configuration

The default configuration looks like the following and is \(by default\) stored in `config.yaml`:

```text
# Please see bos_auto/config-defaults.yaml for description

node: ws://localhost:8090

network: beatrice

redis_password: <your redis password>

passphrase: <your python peerplays wallet password>

BOOKIE_PROPOSER: <your witness account name>

BOOKIE_APPROVER: <your witness account name>
```

Both, the API and the worker make use of the same configuration file. 

We need to provide the wallet pass phrase in order for the worker to be able to propose changes to the blockchain objects according to the messages received from the data feed.

## Command Line Tool

## Schema

The messages sent to the API need to follow a particular message schema which is defined in `endpointschema.py`

```text
#: Default incident schema as sent from the data proxy
schema = {
    "$schema": "http://json-schema.org/draft-06/schema#",
    "title": "Bookie dataproxy Json",
    "description": "Bookie dataproxy exchange event trigger format",
    "type": "object",
    "properties": {
        "id": {
            "type": "object",
            "properties": {
                "sport": {
                    "description": "The unique name of the sport, in english",
                    "type": "string",
                },
                "event_group_name": {
                    "description": "The unique name of the event group, in english (e.g. the league)",
                    "type": "string",
                },
                "start_time": {
                    "description": "The start time of the event in UTC, ISO format",
                    "type": "string",
                    "format": "date-time",
                },
                "home": {
                    "description": "The unique name of the home team, in english",
                    "type": "string",
                },
                "away": {
                    "description": "The unique name of the away team, in english",
                    "type": "string",
                },
            },
            "required": ["sport", "event_group_name", "start_time", "home", "away"],
        },
        "call": {
            "description": "The trigger that was called",
            "type": "string",
            "enum": ["create", "in_progress", "finish", "result", "unknown", "settle"],
        },
        "arguments": {
            "type": "object",
            "properties": {
                "season": {
                    "description": "The unique season of the sport",
                    "type": "string",
                },
                "whistle_start_time": {
                    "description": "The time the start was whistled on in UTC, ISO format",
                    "type": ["null", "string"],
                    "format": "date-time",
                },
                "whistle_end_time": {
                    "description": "The time the end was whistled off in UTC, ISO format",
                    "type": ["null", "string"],
                    "format": "date-time",
                },
                "home_score": {
                    "description": "The score of the home team",
                    "type": "string",
                },
                "away_score": {
                    "description": "The score of the away team",
                    "type": "string",
                },
            },
        },
    },
    "required": ["id", "call", "arguments"],
}

```



