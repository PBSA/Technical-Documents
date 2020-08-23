# Connecting to an existing network

This document assumes that you are running Ubuntu 18.04. Other Debian based releases may also work with the provided script.

## Installing the required dependencies

```text
sudo apt-get update
sudo apt-get install git curl
```

## Cloning the peerplays docker repository 

```text
git clone https://gitlab.com/PBSA/PeerplaysIO/tools-libs/peerplays-docker.git -b release
```

{% hint style="danger" %}
It is required to have Docker installed on the system that will be performing the steps in this document. Look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker.
{% endhint %}

**Optionally:** Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
# Starting in the project root
./run.sh install_docker
```

The terminal will need to be reinitialized after installation.

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

## Editing the configuration

### Setting up config.ini

For a detailed overview check out: [SON Configuration](../son-configuration.md)

{% hint style="warning" %}
There are many example configuration files, make sure to copy the right one. In this case it is:

 config.ini.**son-exists**.example
{% endhint %}

Copy the `peerplays-docker/data/witness_node_data_dir/config.ini.son-exists.example` to   
`peerplays-docker/data/witness_node_data_dir/config/config.ini`:

```text
# Starting in the project root
cd data/witness_node_data_dir
cp config.ini.son-exists.example config.ini
```

Any custom changes to `SON_WALLET` or `BTC_REGTEST_KEY` should also be made in this file to the variables `bitcoin-wallet` and `bitcoin-private-key`.

{% hint style="warning" %}
To connect to other nodes on a public SON network change `seed-nodes` to specify a seed from that network.

To connect to PBSA's Gladiator set the seed nodes to their endpoints:

```text
seed-nodes=["96.46.49.1:9777"]
```

To see the full list of endpoints: [click here](../pbsas-gladiator-endpoints.md)
{% endhint %}

## Starting the environment

Once the configuration is setup, use `run.sh` to start the peerplaysd and bitcond containers:

```text
# Starting in the project root
./run.sh start_son_regtest
```

The SON network will be created and the seed \(peerplaysd\) and bitcoind-node \(bitcoind\) containers will be launched. 

## Using the CLI wallet

After starting the environment, the CLI wallet for the seed \(peerplaysd\) will be available..

### Connecting to the blockchain with the CLI Wallet

In the terminal use `docker exec` to connect to the wallet.

```text
# In the local terminal
docker exec -it seed cli_wallet
```

{% hint style="warning" %}
I**f an exception is thrown** and contains `Remote server gave us an unexpected chain_id`, then copy the `remote_chain_id` that is provided by it. 

Pass the chain ID to the CLI wallet:

```text
# In the local terminal
docker exec -it seed cli_wallet --chain-id=<CHAIN-ID>
```
{% endhint %}

Set a password for the wallet and then unlock it:

```text
# In the CLI wallet
set_password <YOUR-WALLET-PASSWORD>
unlock <YOUR-WALLET-PASSWORD>
```

{% hint style="success" %}
The CLI wallet will show `unlocked >>>` when successfully unlocked
{% endhint %}

### Creating a Peerplays account

Use the CLI wallet to suggest a brain key:

```text
# In the CLI wallet
suggest_brain_key
```

{% hint style="warning" %}
Make sure to backup the information that is output
{% endhint %}

Create an account using the brain key generated:

```text
# In the CLI wallet
create_account_with_brain_key <BRAIN-KEY> <YOUR-ACCOUNT-NAME> nathan nathan true
```

## Interacting with Bitcoin

For information on end-to-end BTC transactions with SON [proceed to the next steps](../bitcoin-transactions.md). 

## Cleaning up the environment

To remove the containers and data from the environment run:

```text
# Starting in the project root
./run.sh clean son
```

