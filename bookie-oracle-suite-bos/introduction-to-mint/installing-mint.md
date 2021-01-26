# Installing MINT

## Install Dependencies

{% hint style="warning" %}
**Note**: Dependencies must be installed as `root/sudo`
{% endhint %}

```text
apt-get install libffi-dev libssl-dev python-dev python3-pip
pip3 install virtualenv
```

{% hint style="warning" %}
Note: `virtualenv` is a best practice for python, but installation can also be on a `user/global` level.
{% endhint %}

## Install Databases

{% hint style="warning" %}
**Note**: Databases must be installed as `root/sudo`
{% endhint %}

MINT uses a local SQLite database which requires MySQL setup \(running a MySQL server instance is not required\). Assuming a Ubuntu 16.04. or later operating system, install:

```text
apt-get install libmysqlclient-dev
```

## Install bos-mint

{% hint style="warning" %}
**Note**: bos-mint should be installed as `user`
{% endhint %}

You can either install bos-mint via pypi / pip3 \(production installation\) or via git clone \(debug installation\). For production use install bos-auto via pip3 is recommended, but the Git master branch is always the latest release as well, making both installations equivalent.

```bash
cd ~
mkdir bos-mint
cd bos-mint
# create virtual environment
virtualenv -p python3 env
# activate environment
source env/bin/activate
# install bos-mint into virtual environment
pip3 install bos-mint
```

For debug use, checkout from GitHub \(master branch\) and install dependencies manually:

```bash
cd ~
# checkout from github
git checkout https://github.com/peerplays-network/bos-mint
cd bos-mint
# create virtual environment
virtualenv -p python3 env
# activate environment
source env/bin/activate
# install dependencies
pip3 install -r requirements.txt
```

BOS MINT is supposed to run in the virtual environment. Either activate it beforehand like shown above or run it directly in the env/bin folder.

## Upgrading bos-mint

{% hint style="warning" %}
**Note**: bos-mint should be upgraded as `user`
{% endhint %}

For production installation, upgrade to the latest version - including all dependencies run:

```bash
pip3 install --upgrade bos-mint
```

For debug installation, pull latest master branch and upgrade dependencies manually:

```bash
git pull
pip3 install -r requirements.txt --upgrade
```

## Modify Configuration

Next step is to configure bos-auto.

```bash
# basic mint configuration file
wget https://raw.githubusercontent.com/peerplays-network/bos-mint/master/config-example.yaml
mv config-example.yaml config-bos-mint.yaml
# modify config-bos-mint.yaml (add your own peerplays node and secret key)
```

Default configuration only requires the following:

```bash
# see bos_mint/config-defaults.yaml for explanation and all possible override values
secret_key: # enter some random string
allowed_assets:
    - BTF
connection:  
    use: beatrice
    beatrice:
        node:
            - # enter your node
```

Possible override values are :

```bash
debug: False
project_name: MINT
project_sub_name: The BOS Manual Intervention Module
secret_key: # enter any random string 
sql_database: "sqlite:///{cwd}/bookied-local.db"
connection:
    use: # enter your desired chain
    beatrice:
        node:
            - # enter your node
        nobroadcast: False
        num_retries: 1
notifications:
    accountLessThanCoreInfo: 1000
    accountLessThanCoreWarning: 200
allowed_assets:
    - BTF
    - BTC
    - PPY
    - BTCTEST
    - TEST
allowed_transitions:
    EventStatus:
        create:
            - upcoming
        upcoming:
            - in_progress
            - finished
            - frozen
            - canceled
        in_progress:
            - finished
            - frozen
            - canceled
        finished:
            - canceled
        frozen:
            - upcoming
            - in_progress
            - frozen
            - canceled
            - finished
    BettingMarketGroupStatus:
        create:
            - upcoming
        upcoming:
            - closed
            - canceled
            - in_play
            - frozen
        in_play:
            - frozen
            - closed
            - canceled
        frozen:
            - closed
            - canceled
            - in_play
            - upcoming
        closed:
            - graded
            - canceled
        graded:
            - re_grading
            - settled
            - canceled
        re_grading:
            - graded
            - canceled
    BettingMarketStatus:
        create:
            - unresolved
        unresolved:
            - win
            - not_win
            - canceled
            - frozen
        frozen:
            - unresolved
            - win
            - not_win
            - canceled
        win:
            - not_win
            - canceled
        not_win:
            - canceled
            - win
```

## Running bos-mint

To run MINT in debug mode use:

```bash
bos-mint start  --port 8001 --host localhost
```

The output that you see should contain:

```python
2018-05-18 11:56:04,754 INFO      :  * Running on http://localhost:8001/ (Press CTRL+C to quit)
```

The above setup is basic and for development use. Going forward, a Witness may want to deploy UWSGI with parallel workers for the endpoint.

MINT is purposely run on `localhost` to restrict outside access. Securing a Python flask application from malicious break in attempts is tedious and would be an ongoing effort.

{% hint style="danger" %}
**Important**: Recommendation is to access it via a SSH tunnel or through VPN.
{% endhint %}

Example for SSH tunnel:

Assume bos-mint is running on a remote server accessible via 1.2.3.4 and you have login credentials via SSH \(password or private key access\). On the local machine that you'll be using to access MINT via a web browser open the tunnel:

```python
ssh -f -N -L 8080:127.0.0.1:8001 yourusername@1.2.3.4
```

-f : Send process to background

-N : Do not send commands \(if you need open ssh connections only for tunnelling\)

-L : Port mapping \(8080 port on your machine, 127.0.0.1:8001 - proxy to where MINT runs\)

Now you can open mint in your browser using [http://localhost:8080](http://localhost:8080/) address.

After starting MINT use your favourite desktop browser to access it and you'll be asked to enter your Witness key that will be stored encrypted in the local Peerplays wallet.

{% hint style="warning" %}
**Note**: MINT is not optimized for mobile use yet.
{% endhint %}

## Development Use

For MINT development use checkout the latest repository from:

{% embed url="https://github.com/peerplays-network/bos-mint" caption="" %}

and then run:

```python
$ ./run_dev_server.sh    # Run MINT
```

