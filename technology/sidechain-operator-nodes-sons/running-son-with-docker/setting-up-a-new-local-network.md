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

## Interacting with Bitcoin \(Regtest\)

{% hint style="info" %}
The following instructions are designed for use within a Bitcoin regtest network. Read more about it here: [https://developer.bitcoin.org/examples/testing.html](https://developer.bitcoin.org/examples/testing.html)
{% endhint %}

### Creating a Bitcoin Wallet

To create a bitcoin wallet on the existing network:

```text
ocker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 createwallet <WALLET-NAME>
```

{% hint style="info" %}
**Tip:** Wallet Names must be unique. Name the wallet after your account
{% endhint %}

### Generating Bitcoin addresses

{% hint style="danger" %}
Wait for 15 minutes after the setup script was run \(new local networks\) before proceeding to the next steps. This is the default review period it takes for the sonaccount's to become active sons. To check if they are active:

```text
# In the CLI wallet
list_active_sons
```

The output should **not** be an empty array.
{% endhint %}

Create two bitcoin addresses and get their information \(run these commands twice\):

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getnewaddress
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getaddressinfo <BITCOIN_ADDRESS>
```

{% hint style="warning" %}
Take note of the "pubkey" value when getting the address info
{% endhint %}

### Mapping the Bitcoin addresses to a Peerplays account

```text
# In the CLI wallet
add_sidechain_address <ACCOUNT> bitcoin <BITCOIN_DEPOSIT_PUBLIC_KEY> <BITCOIN_WITHDRAW_PUBLIC_KEY> <BITCOIN_WITHDRAW_ADDRESS> true
```

### Depositing Bitcoin to a Peerplays account

{% hint style="warning" %}
Blocks will first need to be mined on a new regtest network in order to have funds to send. Generate 101 blocks to a Bitcoin address to start:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 101 <BITCOIN_ADDRESS>
```

Generating more blocks will increase the amount of BTC that is available on the network.
{% endhint %}

#### Getting the sidechain deposit address for BTC transactions

{% hint style="danger" %}
The Sidechain deposit addresses and Bitcoin deposit addresses are different.
{% endhint %}

```text
# In the CLI wallet
get_sidechain_address_by_account_and_sidechain <ACCOUNT> bitcoin
```

#### Send some Bitcoin to the sidechain deposit address of a Peerplays account:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" sendtoaddress <SIDECHAIN_DEPOSIT_ADDRESS> <AMOUNT> "" "" true
```

#### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

Proposals will be created on the Peerplays chain after block generation. Wait for a minute for some blocks to be generated on the Peerplays chain and then generate another block in the regtest network: 

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

{% hint style="info" %}
To check if the Bitcoin was successfully transferred to the Peerplays account list its account balance:

```text
# In the CLI wallet
list_account_balances <ACCOUNT>
```

The output will show some BTC.
{% endhint %}

### Withdrawing Bitcoin from a Peerplays account

#### Transfer BTC from a Peerplays account to the `son-account`:

{% hint style="warning" %}
In order to transfer funds, the Peerplays account sending them must have some core tokens to pay the transfer fee.  
  
Fund the account with some core tokens \(TEST in this case\):

```text
# In the CLI wallet
transfer nathan <ACCOUNT> 100 TEST "" true
```
{% endhint %}

```text
# In the CLI wallet
transfer <ACCOUNT> son-account <WITHDRAW_AMOUNT> BTC "" true
```

#### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

{% hint style="info" %}
To check if the Bitcoin was successfully sent to the withdrawal address that was mapped to the Peerplays account, check the received balance in the Bitcoin node:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getreceivedbyaddress <BITCOIN_WITHDRAW_ADDRESS>
```

The output will show some BTC.
{% endhint %}

## Cleaning up the environment

To remove the containers and data from the environment run:

```text
# Starting in the project root
./run.sh clean son
```

