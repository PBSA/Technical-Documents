---
description: Set up a Sidechain Operator Node (SON) by building the source code
---

# Manual Install

The process of manually installing a SON is similar to installing a Witness Node. This is an introduction for new SONs to get up to speed on the Peerplays blockchain. It is intended for SONs planning to join a live, already deployed, blockchain.

{% hint style="info" %}
This tutorial will take you through the steps required to have an operating SON. Since SONs serve the purpose of facilitating transfers of assets between the Peerplays blockchain and other blockchains, we'll need to connect to another chain to be of any use...

Let's use **Bitcoin**! 😎 
{% endhint %}

Please review the Requirements for setting up a SON before continuing to run a manual install following this guide.

The following steps outline the manual installation of a \(Bitcoin enabled\) SON.

1. Preparing the Environment
2. Build Peerplays
3. Connect to the Bitcoin Network and Generate an Address
4. Create a SON Account
5. Configure the SON
6. Start the SON
7. \(Optional\) Automatically Start the Node as a Service

## 1. Preparing the Environment

### 1.1. Hardware Requirements

Please see the general SON [hardware requirements](../../the-basics/hardware-requirements.md).

For the manual install, we'll be using a self-hosted Bitcoin node. The requirements that we'll need for this guide would be as follows \(as per the hardware requirements doc\):

| Bitcoin node type | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Self-Hosted, Reduced Storage | 2 Cores | 16GB ⚠  | 150GB SSD | 1Gbps | Ubuntu 18.04 |

{% hint style="danger" %}
The memory requirements shown in the table above are adequate to operate the node. Building and installing the node from source code \(as with this manual installation guide\) will require more memory. You may run into errors during the build and install process if the system memory is too low. See [Installing vs Operating](../../the-basics/hardware-requirements.md#4-2-installing-vs-operating) for more details.
{% endhint %}

### 1.2. Installing the required dependencies

The following dependencies are necessary for a clean install on Ubuntu 18.04 LTS:

```text
sudo apt-get update
sudo apt-get -y  install vim autoconf bash build-essential ca-certificates cmake \
      dnsutils doxygen git graphviz libbz2-dev libcurl4-openssl-dev \
      libncurses-dev libreadline-dev libssl-dev libtool libzmq3-dev \
      locales ntp pkg-config wget autotools-dev libicu-dev python-dev
```

### 1.3. Build Boost 1.67.0

Boost is a C++ library that handles common program functions like generating config files and basic file system i/o. Peerplays uses Boost to handle such functions. Since Boost is a dependency, we must build it here.

```text
mkdir $HOME/src
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
sudo apt-get update
sudo apt-get install -y autotools-dev build-essential  libbz2-dev libicu-dev python-dev
wget -c 'http://sourceforge.net/projects/boost/files/boost/1.67.0/boost_1_67_0.tar.bz2/download'\
     -O boost_1_67_0.tar.bz2
tar xjf boost_1_67_0.tar.bz2
cd boost_1_67_0/
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install
```

## 2. Build Peerplays

Now we build Peerplays with the official source code from GitHub.

```text
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git checkout master # --> replace with most recent tag
git submodule update --init --recursive
git submodule sync --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release
# the following command will install the executable files under /usr/local
make install

# the following isn't required if you ran the "make install" command above.
# If you prefer, the "make -j$(nproc)" command will install the programs under /home/ubuntu/src/peerplays/programs
make -j$(nproc)
```

{% hint style="warning" %}
**Note**: "master" can be replaced with the most recent release tag. For example: `git checkout 1.5.11` where 1.5.11 is the latest production release tag as of June 2021. The list of releases is [located here](https://github.com/peerplays-network/peerplays/releases).
{% endhint %}

### 2.1. Start the node to generate the config.ini file

{% hint style="info" %}
We start the SON Node with the `witness_node` command although we are only intending to set up this node as a SON. This is because the same program is used to operate different types of nodes depending on how we configure the program. For more information on this, see [Peerplays Node Types](../../the-basics/peerplays-node-types.md).
{% endhint %}

If we have installed the blockchain following the above steps, the node can be started as follows:

```text
witness_node
```

Running the witness\_node program will create a `config.ini` file with some default settings. We'll need to edit the config file so we'll stop the program for now. Stop the program with `ctrl + c`.

## 3. Connect to Bitcoin Network and Generate an Address

There are two options available to connect to the Bitcoin network.

1. Run a Bitcoin node yourself
2. Find an open Bitcoin node to connect to

For the purposes of this guide, I'll discuss how to run a node yourself as that will be a more reliable connection for now. Either way you go, you'll need to collect the following information to use in the `config.ini` file:

* The IP address of a Bitcoin node you can connect to \(127.0.0.1 if self-hosting\)
* ZMQ port of the Bitcoin node \(default is 1111\)
* RPC port of the Bitcoin node \(default is 8332\)
* Bitcoin RPC connection username \(default is 1\)
* Bitcoin RPC connection password \(default is 1\)
* Bitcoin wallet label \(default is son-wallet\)
* Bitcoin wallet password
* A new Bitcoin address
* The Public key of the Bitcoin address
* The Private key of the Bitcoin address

### 3.1. Install Bitcoin-core

First we'll download and install one of the official Bitcoin Core binaries:

```text
cd ~/src
wget https://bitcoincore.org/bin/bitcoin-core-0.21.0/bitcoin-0.21.0-x86_64-linux-gnu.tar.gz
# Or if you're using ARM architecture...
# wget https://bitcoincore.org/bin/bitcoin-core-0.21.0/bitcoin-0.21.0-aarch64-linux-gnu.tar.gz

tar xzf bitcoin-0.21.0-x86_64-linux-gnu.tar.gz
sudo install -m 0755 -o root -g root -t /usr/local/bin bitcoin-0.21.0/bin/*
```

Then we make a config file to manage the settings of our new Bitcoin node.

```text
cd ~
mkdir .bitcoin
cd .bitcoin
vim bitcoin.conf
```

in the Vim text editor we'll set the following:

```text
# This config should be placed in following path:
# /home/ubuntu/.bitcoin/bitcoin.conf

# [core]
# Only download and relay blocks - ignore unconfirmed transaction
blocksonly=1
# Run in the background as a daemon and accept commands.
daemon=1
# Set database cache size in megabytes; machines sync faster with a larger cache. Recommend setting as high as possible based upon machine's available RAM.
dbcache=1024
# Reduce storage requirements by only storing most recent N MiB of block. This mode is incompatible with -txindex and -rescan. WARNING: Reverting this setting requires re-downloading the entire blockchain. (default: 0 = disable pruning blocks, 1 = allow manual pruning via RPC, greater than 550 = automatically prune blocks to stay under target size in MiB).
prune=10000

# [network]
# Bind to given address and always listen on it. (default: 0.0.0.0). Use [host]:port notation for IPv6. Append =onion to tag any incoming connections to that address and port as incoming Tor connections
bind=0.0.0.0
# Listen for incoming connections on non-default port.
port=8333

# [rpc]
# Accept command line and JSON-RPC commands.
server=1
# Accept public REST requests.
rest=1
# Bind to given address to listen for JSON-RPC connections. This option is ignored unless -rpcallowip is also passed. Port is optional and overrides -rpcport. Use [host]:port notation for IPv6. This option can be specified multiple times. (default: 127.0.0.1 and ::1 i.e., localhost, or if -rpcallowip has been specified, 0.0.0.0 and :: i.e., all addresses)
rpcbind=0.0.0.0
# Listen for JSON-RPC connections on this port
rpcport=8332
# Allow JSON-RPC connections from specified source. Valid for <ip> are a single IP (e.g. 1.2.3.4), a network/netmask (e.g. 1.2.3.4/255.255.255.0) or a network/CIDR (e.g. 1.2.3.4/24). This option can be specified multiple times.
rpcallowip=0.0.0.0/32
rpcuser=1
rpcpassword=1

# [zeromq]
# Enable publishing of block hashes to <address>.
zmqpubhashblock=tcp://0.0.0.0:11111
# Enable publishing of transaction hashes to <address>.
zmqpubhashtx=tcp://0.0.0.0:11111
# Enable publishing of raw block hex to <address>.
zmqpubrawblock=tcp://0.0.0.0:11111
# Enable publishing of raw transaction hex to <address>.
zmqpubrawtx=tcp://0.0.0.0:11111


# [Sections]
# Most options automatically apply to mainnet, testnet, and regtest networks.
# If you want to confine an option to just one network, you should add it in the relevant section.
# EXCEPTIONS: The options addnode, connect, port, bind, rpcport, rpcbind and wallet
# only apply to mainnet unless they appear in the appropriate section below.

# Options only for mainnet
[main]

# Options only for testnet
[test]

# Options only for regtest
[regtest]
```

Save and quit the Vim editor.

{% hint style="info" %}
The settings in the config file above are set to reduce the requirements of the server. Block pruning and setting the node to Blocks Only save network and storage resources. For more information, see [https://bitcoin.org/en/full-node\#reduce-storage](https://bitcoin.org/en/full-node#reduce-storage).
{% endhint %}

Lastly we'll set a Cron job to ensure the Bitcoin node starts up every time the server starts.

```text
cd ~
crontab -e
```

At the bottom of the crontab file, add the following:

```text
@reboot bitcoind -daemon
```

Save and quit the crontab file. Now we're ready to fire up the Bitcoin node!

```text
bitcoind -daemon
```

{% hint style="info" %}
If successful, you'll see `Bitcoin Core starting`. As an extra check to see if everything is working, try the `bitcoin-cli -version` or `bitcoin-cli getblockchaininfo` commands.
{% endhint %}

Your Bitcoin node should now be downloading the Bitcoin blockchain data from other nodes. This might take a few hours to complete even though we cut down the requirements with block pruning. It's a lot of data after all.

### 3.2. Use the bitcoin-cli program to make a new wallet and Bitcoin address

We'll need a wallet to store the new Bitcoin address.

```text
bitcoin-cli createwallet "son-wallet"

# To create a wallet with a password, you'd do it like this:
# bitcoin-cli createwallet "son-wallet" false false "mycoolpassword"
# Note the two false parameters in the line above.
# For more information on this command, see https://developer.bitcoin.org/reference/rpc/createwallet.html.
```

Now we will create a Bitcoin address.

```text
bitcoin-cli -rpcwallet="son-wallet" getnewaddress

# This will return something like:
# bc1qsx7as3r9d92tjvxrgwue7z66f2r3pw04j67lht
```

Then we'll use this address to get its keys.

```text
bitcoin-cli -rpcwallet="son-wallet" getaddressinfo bc1qsx7as3r9d92tjvxrgwue7z66f2r3pw04j67lht

# This returns a lot of info but what we're looking for is the "pubkey".
# In this case, it's "pubkey": "023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9"
```

Now we get the private key.

```text
bitcoin-cli -rpcwallet="son-wallet" dumpprivkey bc1qsx7as3r9d92tjvxrgwue7z66f2r3pw04j67lht

# This returns the private key like this:
# KzD2WHeG49aYhYVcxBwfknm58YqDc7WEg7aWWU8P8BJ8gp1g3AuD
```

### 3.3. A quick recap

That was a lot to go over. Let's collect our data.

```text
Bitcoin Address for the SON Account = bc1qsx7as3r9d92tjvxrgwue7z66f2r3pw04j67lht
The public key = 023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9
the private key = KzD2WHeG49aYhYVcxBwfknm58YqDc7WEg7aWWU8P8BJ8gp1g3AuD

And we'll convert this info into a tuple ["Bitcoin public key", "Bitcoin private key"]
["023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9","KzD2WHeG49aYhYVcxBwfknm58YqDc7WEg7aWWU8P8BJ8gp1g3AuD"]
```

Keep this tuple handy. We'll need it in the Peerplays config file.

## 4. Use the CLI Wallet to Create a Peerplays SON Account

### 4.1. Becoming a SON

Becoming a SON is very similar to becoming a witness. You will need:

* An active user account, upgraded to lifetime member, which will be the owner of the SON account
* Create two vesting balances \(types "son" and "normal"\) of 50 PPY, and get their IDs
* The Bitcoin address created for the SON account
* Create the SON account, and get its ID
* Set the signing key for the SON account \(usually, its a signing key of the owner account\)
* Set the Bitcoin address as a sidechain address for the SON account

### 4.2. Running the cli\_wallet program

We can run the Peerplays cli wallet connecting to the Peerplays node we have set up so far. Before we can do that we'll need to make a quick edit to the config.ini file.

```text
cd /home/ubuntu/witness_node_data_dir
vim config.ini
```

in the first section of the config.ini file is the rpc-endpoint setting. We have to open our rpc-endpoint so we can use the Peerplays cli wallet. We'll enter the following:

```text
# Endpoint for websocket RPC to listen on
rpc-endpoint = 127.0.0.1:8090
```

Save the file and quit.

Our Peerplays node will have to be completely in sync with the blockchain before we can use the cli wallet so we'll start the node and wait for it to download all the data.

```text
witness_node

# If there is an error, try adding the --resync-blockchain or --replay-blockchain flags...
# witness_node --replay-blockchain
```

{% hint style="info" %}
downloading all the transaction and block data will take hours. Unfortunately this is unavoidable the first time the node syncs with the blockchain. You might want to let this run overnight.
{% endhint %}

If you just can't wait for your node to sync, you can run the cli\_wallet program on someone else's node. Simply pass the IP address of the other node like so. \(In another command line window\)

```text
cli_wallet --server-rpc-endpoint=wss://api.eifos.org
```

{% hint style="info" %}
A good resource for server-rpc-endpoints is [https://beta.eifos.org/status](https://beta.eifos.org/status). They will be listed as API nodes and use the wss:// protocol.
{% endhint %}

### 4.3. Using the cli\_wallet

Now that we have the cli\_wallet running, you'll notice a new prompt.

```text
new >>>
```

This means we're in a cli\_wallet session. First we'll make a new wallet and unlock it.

```text
set_password password
unlock password
```

{% hint style="info" %}
A list of CLI wallet commands is available here: [https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls](https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls)
{% endhint %}

Assuming we're starting without any account, it's easiest to create an account with the Peerplays GUI Wallet. The latest release is located here [https://github.com/peerplays-network/peerplays-core-gui/releases/latest](https://github.com/peerplays-network/peerplays-core-gui/releases/latest). When you create an account with the GUI wallet, you should have a username and password. We'll need those for the next steps. First we'll get the private key for the new account.

```text
# In the cli_wallet...

get_private_key_from_password <put your username here> active <put your password here>

# For example:
# get_private_key_from_password mynew-son active LExu4QtSapqzdEaly2RwMugul3GhedTf234IiF2zzzfU4nuKXow8

# The program will return a tuple of the public and private keys for your account. 
# That will look something like this:
# [
#   "PPY...random.numbers.and.letters...",
#   "...random.numbers.and.letters..."
# ]
```

{% hint style="info" %}
The key beginning with "PPY" is the public key. The other key is the private key. We'll need to import this private key into the cli\_wallet.
{% endhint %}

```text
# In the cli_wallet...

import_key "mynew-son" ...random.numbers.and.letters...

# If this is successful, the return is simply
# true
```

Next we'll upgrade the account to a lifetime membership.

{% hint style="info" %}
At the time of writing this guide, this costs 5 PPY to perform this operation. You'll need that in your account first! To this end, see [Obtaining Your First Tokens](../../the-basics/obtaining-your-first-tokens.md).
{% endhint %}

```text
# In the cli_wallet...

upgrade_account mynew-son true
```

Next we'll create the vesting balances.

```text
# In the cli_wallet...

create_vesting_balance mynew-son 50 PPY son true
create_vesting_balance mynew-son 50 PPY normal true
get_vesting_balances mynew-son

# The return here will show us the IDs of the vesting balances.
# For example:
# [{
#     "id": "1.13.79",
#     "owner": "1.2.58",
# ...
#   },{
#     "id": "1.13.80",
#     "owner": "1.2.58",
# ...
#   }
# ]
```

Now we have all the info we need to create a SON account.

```text
# In the cli_wallet...

create_son mynew-son "https://www.mynew-son.com" 1.13.79 1.13.80 [[bitcoin, 023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9]] true

# The above command is structured like this:
# create_son <username> "<SON proposal url>" <son vesting balance ID> <normal vesting balance ID> [[bitcoin, <bitcoin public key>]] true
```

To get the SON ID:

```text
# In the cli_wallet...

get_son mynew-son

# Which returns:
# {
#   "id": "1.27.16",
#   ...
# }
```

We'll set the signing key using the active key from the owning account:

```text
# In the cli_wallet...

# First we'll get the active key of the owning account.
get_account mynew-son

# In the return, we're looking for the 
# "active": 
# ...
# { ... "key_auths": 
# [[ "PPY7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB",
# ...

# Using the active public key we just found:
get_private_key PPY7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB

# This will return the private key.
# "5JKvPJkerMNVEubsbKN8Xd8wGaU1ifhv7xAwy9gFJP6yMEoTkSd"

# Then we update the signing key using the public key.
update_son mynew-son "" "PPY7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB" [[bitcoin, 023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9]] true
```

Now we have our SON account ID and the public and private keys for the SON account. We'll need this for the `config.ini` file.

## 5. Peerplays SON Configuration

The generated `config.ini` file will be located at `/home/ubuntu/witness_node_data_dir/config.ini`. We'll begin by editing this config file.

```text
cd /home/ubuntu/witness_node_data_dir
vim config.ini
```

This file will be rather large so let's focus on the important part for configuring a SON node:

```text
# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================
```

This section contains all the SON related configuration. Ensure the following config settings are in the `config.ini` file under the peerplays\_sidechain plugin options.

```text
# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.16"

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
# son-ids = 

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
peerplays-private-key = ["PPY7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB", "5JKvPJkerMNVEubsbKN8Xd8wGaU1ifhv7xAwy9gFJP6yMEoTkSd"]

# IP address of Bitcoin node
bitcoin-node-ip = 127.0.0.1

# ZMQ port of Bitcoin node
bitcoin-node-zmq-port = 11111

# RPC port of Bitcoin node
bitcoin-node-rpc-port = 8332

# Bitcoin RPC user
bitcoin-node-rpc-user = 1

# Bitcoin RPC password
bitcoin-node-rpc-password = 1

# Bitcoin wallet
bitcoin-wallet = son-wallet

# Bitcoin wallet password
bitcoin-wallet-password = 

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["023b907586045625367ecd62c5d889591586c87e57fa49be21614209489f00f1b9","KzD2WHeG49aYhYVcxBwfknm58YqDc7WEg7aWWU8P8BJ8gp1g3AuD"]
```

We're almost done, we also have to make sure the peerplays\_sidechain plugin is listed in the plugins. Find the `plugins` setting in the first section of the `config.ini` file. If it's not already there, add the `peerplays_sidechain` plugin to the list. Like so:

```text
# Space-separated list of plugins to activate
plugins = witness account_history market_history accounts_list affiliate_stats bookie peerplays_sidechain
```

Save the file and quit. Configuration of the Peerplays SON node is complete! 😅 

## 6. Start the SON

After setting up the `config.ini` file for SON operation, we'll start the node back up.

```text
witness_node
```

{% hint style="success" %}
Your SON is born! \(pun intended\)

But seriously, that was no small feat. Congratulations on this accomplishment! 🎊 
{% endhint %}

## 7. What's Next?

### 7.1. Auto-starting your node

Up until this point we have been running the node in the foreground which is fragile and inconvenient. So let's start the node as a service when the system boots up instead.

{% page-ref page="../../the-basics/auto-starting-a-node.md" %}

### 7.2. Creating a backup node

After that, it would be smart to create a backup server to enable you to make software updates, troubleshoot issues with the node, and otherwise take your node offline without causing service outages.

{% page-ref page="../../the-basics/backup-servers.md" %}

### 7.3. Join the Testnet \(Beatrice\)

As we all know, testing should never be done in production. This is why all node operators must also participate in the public testnet, Beatrice.

{% page-ref page="../../the-basics/joining-the-public-testnet.md" %}

### 7.4. Configure more sidechains

Why stop at Bitcoin?

{% page-ref page="../enabling-components.md" %}

### 7.5. Fire up another node 🔥 

Now you have a SON, but have you thought about becoming a Witness? It will be a piece of cake for you since you've already set up a SON.

{% hint style="warning" %}
When you make any node, don't forget [Beatrice](../../the-basics/joining-the-public-testnet.md)!
{% endhint %}

### 7.6. Enable SSL to encrypt your node traffic

If you have a node that is accessible from the internet \(for example, an API or Seed node\) it would be wise to enable SSL connections to your node.

{% page-ref page="../../advanced-topics/reverse-proxy-for-enabling-ssl.md" %}

## 8. Glossary

**SON**: Sidechain Operator Node - An independent server operator which facilitates the transfer of off-chain assets \(like Bitcoin or Ethereum tokens\) between the Peerplays chain and the asset's native chain.

**Witness:** An independent server operator which validates network transactions.

**Witness Node:** Nodes with a closed RPC port. They don't allow external connections. Instead these nodes focus on processing transactions into blocks.

**Vim**: A text editing program available for Ubuntu 18.04. See [vim.org](https://www.vim.org/)

