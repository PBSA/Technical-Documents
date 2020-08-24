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

## Navigating to the project directory and setting up the project root

To easily follow this document, it is recommended to set an environment variable to the project root. Navigate to the root of the project, and assign the variable:

```text
# Starting in the directory where the repository was cloned
cd peerplays-docker
PROJECT_ROOT=$(pwd)
```

## Installing Docker

{% hint style="danger" %}
It is required to have Docker installed on the system that will be performing the steps in this document. 
{% endhint %}

Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
./run.sh install_docker
```

The terminal will need to be reinitialized after installation.

{% hint style="info" %}
**Optional**: you can Look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker.
{% endhint %}

{% hint style="danger" %}
If you are having permission issues trying to run Docker use `sudo` or look at [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)
{% endhint %}

## Setting up the environment

Copy the `example.env` to `.env`  located in the root of the repository:

```text
cd $PROJECT_ROOT
cp example.env .env
```

Use the script ****provided which will automate the replacement of the `BTC_REGTEST_CONF`in `bitcoin.conf`.

```text
cd $PROJECT_ROOT
cd scripts/regtest
./replace_btc_conf.sh
```

{% hint style="info" %}
The script uses the`bitcoin.conf` file located in `peerplays-docker/bitcoin/regtest/bitcoin.conf`
{% endhint %}

**Optional:** If you want to use your own config, edit the .env file & set`BTC_REGTEST_CONF` to the full path where the `bitcoin.conf` is located. 

{% hint style="warning" %}
To enable the blockchain API to be accessible outside of its docker container, make sure to specify `PORTS` in the `.env` file
{% endhint %}

## Installing the peerplays:son-dev image

Use `run.sh` to pull the SON image:

```text
cd $PROJECT_ROOT
sudo ./run.sh install son-dev
```

## Editing the configuration

### Setting up config.ini

For a detailed overview check out: [SON Configuration](../son-configuration.md)

{% hint style="warning" %}
There are many example configuration files, make sure to copy the right one. In this case it is:

 config.ini.**son-exists**.example
{% endhint %}

Copy the correct example configuration:

```text
cd $PROJECT_ROOT
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
cd $PROJECT_ROOT
./run.sh start_son_regtest
```

The SON network will be created and the seed \(peerplaysd\) and bitcoind-node \(bitcoind\) containers will be launched. 

To check the status, inspect the logs:

```text
./run.sh logs
```

This will give an output similar to 

![\(logs showing healthy connection to GLADIATOR\)](../../../.gitbook/assets/image%20%2828%29.png)

Just in case the logs are not looking healthy, perform a replay.

```text
# replay the blockchain
./run.sh replay
```

## Using the CLI wallet

After starting the environment, the CLI wallet for the seed \(peerplaysd\) will be available..

### Connecting to the blockchain with the CLI Wallet

Open another terminal and use `docker exec` to connect to the wallet.

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

For an existing network, a lifetime account must be imported to create an account. Import the `faucet` account:

```text
# In the CLI wallet
import_key faucet 5KQHVEoqLX4fguLz8xYRLwwDjMk7sE6s5cTBC9jzBHCs5sk6cCr
```

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
create_account_with_brain_key <BRAIN-KEY> <YOUR-ACCOUNT-NAME> faucet faucet true
```

## Interacting with Bitcoin \(Regtest\)

{% hint style="info" %}
The following instructions are designed for use within a Bitcoin regtest network. Read more about it here: [https://developer.bitcoin.org/examples/testing.html](https://developer.bitcoin.org/examples/testing.html)
{% endhint %}

### Creating a Bitcoin Wallet

To create a bitcoin wallet on the existing network:

```text
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 createwallet <WALLET-NAME>
```

{% hint style="info" %}
**Tip:** Wallet Names must be unique. Name the wallet after your account
{% endhint %}

### Generating Bitcoin addresses

Create two bitcoin addresses and get their information \(run these commands twice\):

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> getnewaddress
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> getaddressinfo <BITCOIN_ADDRESS>
```

{% hint style="warning" %}
Take note of the "pubkey" value output when using `getaddressinfo`
{% endhint %}

### Mapping the Bitcoin addresses to a Peerplays account

```text
# In the CLI wallet
add_sidechain_address <ACCOUNT> bitcoin <BITCOIN_DEPOSIT_PUBLIC_KEY> <BITCOIN_WITHDRAW_PUBLIC_KEY> <BITCOIN_WITHDRAW_ADDRESS> true
```

### Depositing Bitcoin to a Peerplays account.

#### Getting the sidechain deposit address for BTC transactions

{% hint style="danger" %}
The Sidechain deposit addresses and Bitcoin deposit addresses are different.
{% endhint %}

```text
# In the CLI wallet
get_sidechain_address_by_account_and_sidechain <ACCOUNT> bitcoin
```

#### Send some Bitcoin to the Sidechain deposit address of a Peerplays account using the faucet wallet:

{% hint style="warning" %}
This is a shared faucet. Please only send small amounts \(less than 0.1 BTC\) and send it back to the faucet when finished. 
{% endhint %}

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 loadwallet faucet
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet="faucet" sendtoaddress <SIDECHAIN_DEPOSIT_ADDRESS> <AMOUNT> "" "" true
```

#### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> generatetoaddress 1 <BITCOIN_ADDRESS>
```

Proposals will be created on the Peerplays chain after block generation. Wait for a minute for some blocks to be generated on the Peerplays chain and then generate another block in the regtest network: 

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> generatetoaddress 1 <BITCOIN_ADDRESS>
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
transfer faucet <ACCOUNT> 100 TEST "" true
```
{% endhint %}

```text
# In the CLI wallet
transfer <ACCOUNT> son-account <WITHDRAW_AMOUNT> BTC "" true
```

#### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> generatetoaddress 1 <BITCOIN_ADDRESS>
```

{% hint style="info" %}
To check if the Bitcoin was successfully sent to the withdrawal address that was mapped to the Peerplays account, check the received balance in the Bitcoin node:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcconnect=96.46.49.1 -rpcport=8332 -rpcuser=1 -rpcpassword=1 -rpcwallet=<WALLET-NAME> getreceivedbyaddress <BITCOIN_WITHDRAW_ADDRESS>
```

The output will show some BTC.
{% endhint %}

## Cleaning up the environment

To remove the containers and data from the environment run:

```text
cd $PROJECT_ROOT
./run.sh clean son
```

