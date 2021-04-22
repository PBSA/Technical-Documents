---
description: Setup SONs using a pre-configured Docker container
---
# Docker Install

This document assumes that you are running Ubuntu 18.04. Other Debian based releases may also work with the provided script.

## Hardware Requirements

Please see the general SON [hardware requirements](requirements.md).

For the docker install, we'll be using a self-hosted Bitcoin node. The requirements that we'll need for this guide would be as follows:

| Full Node? | SON Node? | Bitcoin node type            | CPU     | Memory | Storage   | Bandwidth | OS           |
| :--------- | :-------- | :--------------------------- | :------ | :----- | :-------- | :-------- | :----------- |
| Yes        | Yes       | Self-Hosted, Reduced Storage | 8 Cores | 64GB   | 350GB SSD | 1Gbps     | Ubuntu 18.04 |

## Installing the required dependencies

```text
sudo apt-get update
sudo apt-get install git curl
```

## Cloning the peerplays docker repository

```text
git clone https://gitlab.com/PBSA/tools-libs/peerplays-docker.git -b release
```

## Navigating to the project directory and setting up the project root

To easily follow this document, it is recommended to set an environment variable to the project root. Navigate to the root of the project, and assign the variable:

```text
# Starting in the directory where the repository was cloned
cd peerplays-docker
PROJECT_ROOT=$(pwd)
```

## Installing Docker

> **Note:** It is required to have Docker installed on the system that will be performing the steps in this document.

Docker can be installed using the `run.sh` script inside the Peerplays Docker repository:

```text
./run.sh install_docker
```

The terminal will need to be reinitialized after installation.

> **Optional:** you can Look at [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/) to learn more on how to install Docker.

> **Note:** If you are having permission issues trying to run Docker, use `sudo` or look at [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)

## Setting up the environment

Copy the `example.env` to `.env`  located in the root of the repository:

```text
cd $PROJECT_ROOT
cp example.env .env
```

We're going to have to make some changes to the .env file so we'll open that now.

```text
vim .env
```

Here's what your .env file should look like when we're done:

```text
# In the .env file...

# Unique name to label the container created by this peerplays-docker installation
DOCKER_NAME="seed"

# Default docker image to run using ./run.sh start / restart / replay
# Also used when tagging remote image downloaded from ./run.sh install
DOCKER_IMAGE="peerplays"

# Default network for SON used with ./run.sh start_son_regtest
DOCKER_NETWORK="son"

# Default SON wallet used inside bitcoind-node with ./run.sh start_son_regtest
SON_WALLET="son-wallet"

# Default bitcoin key used inside bitcoind-node with ./run.sh start_son_regtest
# Update this with your own Bitcoin Private Key!
BTC_REGTEST_KEY="XXXXXXXXXXXXX"

# Comma separated port numbers to expose to the internet (binds to 0.0.0.0)
# PORTS=9777
# Advanced Usage Example: 
#
#   Expose 9777 to the internet, but only expose RPC ports 8090 and 8091 onto 127.0.0.1 (localhost)
#   allowing the host machine access to the container's RPC ports via 127.0.0.1:8090 and 127.0.0.1:8091
#
PORTS=9777,127.0.0.1:8090:8090,127.0.0.1:8091:8091

# Amount of time in seconds to allow the docker container to stop before killing it.
# Default: 600 seconds (10 minutes)
STOP_TIME=600

# Websocket RPC node to use by default for ./run.sh remote_wallet
REMOTE_WS=""

# Remote docker tags to pull when running ./run.sh install OR ./run.sh install_full with no arguments, respectively
DK_TAG="datasecuritynode/peerplays:latest"
DK_TAG_FULL="datasecuritynode/peerplays:latest-full"

# Git repository to use when building Peerplays - containing peerplaysd code
PEERPLAYS_SOURCE="https://github.com/peerplays-network/peerplays.git"

# LOCAL folder containing Dockerfile for ./run.sh build
DOCKER_DIR="$DIR/dkr"
# LOCAL folder to hold witness_node_data_dir
DATADIR="$DIR/data"
# LOCAL folder to store shared_memory.bin (or rocksdb files for MIRA)
SHM_DIR="/dev/shm"

# blockchain folder, used by dlblocks
BC_FOLDER="$DATADIR/witness_node_data_dir/blockchain"

# Example RocksDB configuration file, will automatically be copied to MIRA_FILE on first run.sh execution
# if MIRA_FILE doesn't exist
EXAMPLE_MIRA="$DATADIR/witness_node_data_dir/database.cfg.example"
MIRA_FILE="$DATADIR/witness_node_data_dir/database.cfg"

# Example Peerplays node configuration file, will automatically be copied to CONF_FILE on first run.sh execution
# if CONF_FILE doesn't exist
EXAMPLE_CONF="$DATADIR/witness_node_data_dir/config.ini.example"
CONF_FILE="$DATADIR/witness_node_data_dir/config.ini"

# Example Bitcoin Regtest configuration, full path must be specified
BTC_REGTEST_CONF="/home/ubuntu/.bitcoin/bitcoin.conf"

# Array of additional arguments to be passed to Docker during builds
# Generally populated using arguments passed to build/build_full
# But you can specify custom additional build parameters by setting BUILD_ARGS
# as an array in .env
# e.g.
#
#    BUILD_ARGS=('--rm' '-q' '--compress')
#
BUILD_ARGS=()

```

> **IMPORTANT:** You will need a Bitcoin Private Key of a wallet that you own on the Bitcoin mainnet. In the .env file above, you must replace **BTC_REGTEST_KEY="XXXXXXXXXXXXX"** with your own private key. So it may look something like **BTC_REGTEST_KEY="cSKyTeXidmj93dgbMFqgzD7yvxzA7QAYr5j9qDnY9seyhyv7gH2m"** for example.

> **Optional:** If you want to use your own config, edit the .env file & set `BTC_REGTEST_CONF` to the full path where the `bitcoin.conf` is located.

Since we'll be setting some custom config in our bitcoin.conf, we'll need to create and edit it now.

```text
touch /home/ubuntu/.bitcoin/bitcoin.conf
vim /home/ubuntu/.bitcoin/bitcoin.conf
```

The bitcoin.conf should look like this:

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

Save and quit the VIM editor.

> **Note:** The settings in the config file above are set to reduce the requirements of the server. Block pruning and setting the node to Blocks Only save network and storage resources. For more information, see <https://bitcoin.org/en/full-node#reduce-storage>.

## Installing the peerplays:son image

Use `run.sh` to pull the SON image:

```text
cd $PROJECT_ROOT
sudo ./run.sh install son
```

## Editing the configuration

### Setting up config.ini

> There are many example configuration files, make sure to copy the right one. In this case it is:
config.ini.**son-exists**.example

Copy the correct example configuration:

```text
cd $PROJECT_ROOT
cd data/witness_node_data_dir
cp config.ini.son-exists.example config.ini
```

We'll need to make an edit to the config.ini file as well.

```text
vim config.ini
```

The important parts of the config.ini file (for now!) should look like the following. But don't forget to add your own Bitcoin public and private keys!

```text

# Endpoint for P2P node to listen on
# p2p-endpoint =

# P2P nodes to connect to on startup (may specify multiple times)
# seed-node =

# JSON array of P2P nodes to connect to on startup

## Empty seed nodes means new network
# seed-nodes = []

## Connect to other SON nodes
# seed-nodes =

# Pairs of [BLOCK_NUM,BLOCK_ID] that should be enforced as checkpoints.
# checkpoint = 

# Endpoint for websocket RPC to listen on
rpc-endpoint = 0.0.0.0:8090

# Endpoint for TLS websocket RPC to listen on
# rpc-tls-endpoint = 

# The TLS certificate file for this server
# server-pem = 

# Password for this certificate
# server-pem-password = 

# File to read Genesis State from
genesis-json = /peerplays/son-genesis.json

# Block signing key to use for init witnesses, overrides genesis file
# dbg-init-key = 

# JSON file specifying API permissions
# api-access = 

# Space-separated list of plugins to activate
plugins = witness account_history market_history accounts_list affiliate_stats bookie peerplays_sidechain

# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================

# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
# son-id = ""

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
# son-ids = [""]

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
# peerplays-private-key = ["", ""]

# IP address of Bitcoin node
bitcoin-node-ip = 172.0.0.1

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
bitcoin-wallet-password = ""

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
# This should be the public and private keys of the Bitcoin wallet you'll use. You already entered the private key in the .env file above.
bitcoin-private-key = ["", ""]
```

Save the file and quit.

## Starting the environment

Once the configuration is setup, use `run.sh` to start the peerplaysd and bitcoind containers:

```text
cd $PROJECT_ROOT

# You'll also want to set the shared memory size (use sudo if not logged in as root). 
# Adjust 64G to whatever size is needed for your type of server and make sure to leave growth room.
# Please be aware that the shared memory size changes constantly. Ask in a witness chatroom if you're unsure.
./run.sh shm_size 64G

# It's recommended to set vm.swappiness to 1, which tells the system to avoid using swap 
# unless absolutely necessary. To persist on reboot, place in /etc/sysctl.conf
sysctl -w vm.swappiness=1

# Start the SON environment
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
./run.sh replay_son
```

## Using the CLI wallet

After starting the environment, the CLI wallet for the seed \(peerplaysd\) will be available.

### Connecting to the blockchain with the CLI Wallet

Open another terminal and use `docker exec` to connect to the wallet.

```text
# In the local terminal
docker exec -it seed cli_wallet
```

> **NOTE:** If an exception is thrown and contains `Remote server gave us an unexpected chain_id`, then copy the `remote_chain_id` that is provided by it.
> Pass the chain ID to the CLI wallet:
>
> ```text
> # In the local terminal
> docker exec -it seed cli_wallet --chain-id=<CHAIN-ID>
> ```

Set a password for the wallet and then unlock it:

```text
# In the CLI wallet
set_password <YOUR-WALLET-PASSWORD>
unlock <YOUR-WALLET-PASSWORD>
```

The CLI wallet will show `unlocked >>>` when successfully unlocked

> **Note:** A list of CLI wallet commands is available here: <https://www.peerplays.tech/api/peerplays-wallet-api/wallet-calls>

Assuming we're starting without any account, it's easiest to create an account with the Peerplays GUI Wallet. The latest release is located here <https://github.com/peerplays-network/peerplays-core-gui/releases/latest>. When you create an account with the GUI wallet, you should have a username and password. We'll need those for the next steps. First we'll get the private key for the new account.

```text
# In the cli_wallet...

get_private_key_from_password <put your username here> active <put your password here>

# For example:
# get_private_key_from_password mynew-son active LExu4QtSapqzdEaly2RwMugul3GhedTf234IiF2zzzfU4nuKXow8

# The program will return a tuple of the public and private keys for your account. 
# That will look something like this:
# [
#   "PPY...random.numbers.and.letters...",
#   "5...random.numbers.and.letters..."
# ]
```

The key beginning with "PPY" is the public key. The key beginning with "5" is the private key. We'll need to import this private key into the cli_wallet.

```text
# In the cli_wallet...

import_key "mynew-son" 5...random.numbers.and.letters...

# If this is successful, the return is simply
# true
```

Next we'll upgrade the account to a lifetime membership.

> **Note:** At the time of writing this guide, this costs 5 PPY to perform this operation. You'll need that in your account first!

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

Now we have our SON account ID and the public and private keys for the SON account. We'll need this for the config.ini file.

## Update config.ini with SON Account Info

Lets stop the node for now so we can finish up the config.ini.

```text
cd $PROJECT_ROOT
./run.sh stop
```

Ensure the following config settings are in the config.ini file under the peerplays_sidechain plugin options.

```text
cd data/witness_node_data_dir
vim config.ini
```

```text
# In the config.ini file...

# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.16"

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
peerplays-private-key = ["PPY7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB", "5JKvPJkerMNVEubsbKN8Xd8wGaU1ifhv7xAwy9gFJP6yMEoTkSd"]
```

Then it's just a matter of starting the node back up!

```text
# replay the blockchain
./run.sh replay_son
```

## Related Documents

* [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
* [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/)
