# Setting up a new local network

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

## Editing the configuration

### Setting up config.ini

For a detailed overview check out: [SON Configuration](../son-configuration.md)

{% hint style="warning" %}
There are many example configuration files, make sure to copy the right one. In this case it is:

 config.ini.**son-new**.example
{% endhint %}

Copy the `peerplays-docker/data/witness_node_data_dir/config.ini.son-new.example` to `peerplays-docker/data/witness_node_data_dir/config/config.ini`

```text
# Starting in the project root
cd data/witness_node_data_dir
cp config.ini.son-new.example config.ini
```

Any custom changes to `SON_WALLET` or `BTC_REGTEST_KEY` should also be made in config.ini to the variables `bitcoin-wallet` and `bitcoin-private-key`.

{% hint style="warning" %}
By default, the config specifies a new local network \(seed nodes in the config are empty\). 
{% endhint %}

## Installing the peerplays:son-dev image

Use `run.sh` to pull the SON image:

```text
# Starting in the project root
./run.sh install son-dev
```

## Starting the environment

Once the configuration is setup, use `run.sh` to start the peerplaysd and bitcond containers:

```text
# Starting in the project root
./run.sh start_son_regtest
```

The SON network will be created and the seed \(peerplaysd\) and bitcoind-node \(bitcoind\) containers will be launched. 

## Using the CLI wallet

After starting the environment, the CLI wallet for the seed \(peerplaysd\) will be available..

### Setting up the wallet and accounts

When using a new network \(seed nodes are not defined in the config by default\) run the setup script which will create a wallet file \(wallet.json\) locked with a given password within the container and fund the `nathan` and `init0 - init11` witness accounts. 

{% hint style="danger" %}
**Before continuing** with a new network and using the setup script, make sure to import the default bitcoin keys. There is a script provided which does this:

```text
# Starting in the project root
./scripts/regtest/import_btc_keys.sh
```
{% endhint %}

{% hint style="danger" %}
**Make sure** to run the setup script and run it after importing the bitcoin keys using the script above. The setup script can be viewed [here](https://gitlab.com/data-security-node/peerplays-docker/-/blob/release/data/setup_blockchain.sh) if curious. It essentially runs many CLI wallet commands in sequence to setup the accounts on the blockchain.
{% endhint %}

**Run the setup script** by using the following command with a password of choice. This will create a wallet locked by it.

```text
# In the local terminal
docker exec seed /peerplays/setup_blockchain.sh <YOUR-WALLET-PASSWORD>
```

{% hint style="info" %}
The setup script will take a few seconds to complete
{% endhint %}

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

Since the setup script was run, unlock the CLI wallet by providing the password set earlier:

```text
# In the CLI wallet
unlock <YOUR-WALLET-PASSWORD>
```

{% hint style="info" %}
The CLI wallet will show `unlocked >>>` when successfully unlocked
{% endhint %}

The CLI wallet is now ready to be used.

For information on end-to-end BTC transactions with SON [proceed to the next steps](../bitcoin-transactions.md). 

## Cleaning up the environment

To remove the containers and data from the environment run:

```text
# Starting in the project root
./run.sh clean son
```

