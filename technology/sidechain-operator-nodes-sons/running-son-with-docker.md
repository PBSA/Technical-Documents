# Running SON with Docker

## Prerequisites

{% hint style="danger" %}
It is required to have Docker installed on the system that will be performing the steps in this document. Look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker.
{% endhint %}

Optionally, Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
# Starting in the project root
./run.sh install_docker
```

The terminal will need to be reinitialized after installation.

## Cloning the peerplays docker repository 

```text
git clone https://gitlab.com/data-security-node/peerplays-docker.git -b release
```

##  Setting up the environment

Copy the `example.env` to `.env`  located in the root of the repository:

```text
# Starting in the project root
cp example.env .env
```

Edit the .env file & set_`BTC_REGTEST_CONF`_ to the full path where the `bitcoin.conf` is located. 

{% hint style="info" %}
There is a `bitcoin.conf` file located in `peerplays-docker/bitcoin/regtest/bitcoin.conf`
{% endhint %}

**Optional**: There is a script provided which will automate the replacement of the `BTC_REGTEST_CONF`:

```text
# Starting in the project root
cd scripts/regtest
./replace_btc_conf.sh
```

{% hint style="warning" %}
To enable the blockchain API to be accessible outside of its docker container, make sure to specify `PORTS` in the `.env` file
{% endhint %}

## Installing the peerplays:son-dev image

Use `run.sh` to pull the SON image:

```text
# Starting in the project root
./run.sh install son-dev
```

## Next steps

### New local network

For setting up a complete network with SONs and witnesses configured locally proceed to setting up a new local network:

{% page-ref page="setting-up-a-new-local-network.md" %}

### Existing network

For connecting to an existing network \(like PBSA's Gladiator nodes\) proceed to connecting to an existing network:

{% page-ref page="connecting-to-an-existing-network.md" %}



