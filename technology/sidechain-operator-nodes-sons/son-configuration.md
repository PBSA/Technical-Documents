# SON Configuration

## SON configuration file <a id="SON-configuration-file"></a>

Example Config following SON config file is used as a template for SON test network.

Each node will require fine tuning to this config file, to set the parameters specific to that node.

```text
# Endpoint for P2P node to listen on
p2p-endpoint = 0.0.0.0:9777

# P2P nodes to connect to on startup (may specify multiple times)
# seed-node =

# JSON array of P2P nodes to connect to on startup
seed-nodes = ["96.46.49.1:9777", "96.46.49.5:9777", "96.46.49.9:9777", "96.46.49.13:9777"]

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
# witness plugin options
# ==============================================================================

# Enable block production, even if the chain is stale.
enable-stale-production = true

# Percent of witnesses (0-99) that must be participating in order to produce blocks
required-participation = false

# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
# witness-id = 

# IDs of multiple witnesses controlled by this node (e.g. ["1.6.5", "1.6.6"], quotes are required)
# witness-ids =

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["TEST8eF6P2mvHssWGPN5VzEVC22RQkmTyrKskbHWhKbCZ45uwe2xzD", "5JzRHf68T8NT5T9fpDzRtYzxpafVUPPjCwUuxnwJ1L7hgwDBeNY"]


# ==============================================================================
# account_history plugin options
# ==============================================================================

# Account ID to track history for (may specify multiple times)
# track-account = 

# Keep only those operations in memory that are related to account history tracking
partial-operations = 1

# Maximum number of operations per account will be kept in memory
max-ops-per-account = 100


# ==============================================================================
# market_history plugin options
# ==============================================================================

# Track market history by grouping orders into buckets of equal size measured in seconds specified as a JSON array of numbers
bucket-size = [15,60,300,3600,86400]

# How far back in time to track history for each bucket size, measured in the number of buckets (default: 1000)
history-per-size = 1000


# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================

# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.0"

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
# son-ids = 

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
peerplays-private-key = ["TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL", "5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF"]

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
bitcoin-wallet-password = ""

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["03456772301e221026269d3095ab5cb623fc239835b583ae4632f99a15107ef275", "cSKyTeXidmj93dgbMFqgzD7yvxzA7QAYr5j9qDnY9seyhyv7gH2m"]


# ==============================================================================
# logging options
# ==============================================================================
#
# Logging configuration is loaded from logging.ini by default.
# If logging.ini exists, logging configuration added in this file will be ignored.
```

### Config file options that need to be changed

```text
# P2P nodes to connect to on startup (may specify multiple times)
# seed-node = 

# JSON array of P2P nodes to connect to on startup
seed-nodes = []

# Set the list of seed nodes you want to connect to at startup
# Examples:

seed-node = "1.2.3.4:9777"
seed-nodes = ["1.2.3.4:9777", "5.6.7.8:9777"]

# seed-node = 
seed-nodes = ["1.2.3.4:9777", "5.6.7.8:9777"]
# Enable block production, even if the chain is stale.
enable-stale-production = false

# Set this parameter to true on one node only, all other nodes should have false
# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
# witness-id = 

# IDs of multiple witnesses controlled by this node (e.g. ["1.6.5", "1.6.6"], quotes are required)
witness-ids = ["1.6.1", "1.6.2", "1.6.3", "1.6.4", "1.6.5", "1.6.6", "1.6.7", "1.6.8", "1.6.9", "1.6.10", "1.6.11"]"

# Set the list of witness ids belonging to your node
# Examples:

witness-id = "1.6.1"
witness-ids = []

# witness-id = 
witness-ids = ["1.6.1", "1.6.2"]
# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]

# Set your witness public/private key pair.
# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.0"

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
son-ids = ["1.27.0", "1.27.1", "1.27.2", "1.27.3", "1.27.4", "1.27.5", "1.27.6", "1.27.7", "1.27.8", "1.27.9", "1.27.10", "1.27.11", "1.27.12", "1.27.13", "1.27.14", "1.27.15"]

# Set the SON id belonging to your node
# Examples:

son-id = "1.27.1"
son-ids = []

# son-id = 
son-ids = ["1.27.1"]
# Tuple of [PublicKey, WIF private key]
peerplays-private-key = ["TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL", "5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF"]
peerplays-private-key = ...

# Config file contains all default public/private key pairs for SON owner accounts
# No changes are required here, but unused key pairs may be removed
# List of keypairs is ordered, so first belongs to sonaccount01, last to sonaccount16
# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["03456772301e221026269d3095ab5cb623fc239835b583ae4632f99a15107ef275", "cSKyTeXidmj93dgbMFqgzD7yvxzA7QAYr5j9qDnY9seyhyv7gH2m"]
bitcoin-private-key = ...

# Config file contains all default public/private key pairs for SON
# No changes are required here, but unused key pairs may be removed
# List of keypairs is ordered, so first belongs to sonaccount01, last to sonaccount16
```

### Manually configuring single SON

```text
# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================

# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.0"

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
# son-ids = 

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
peerplays-private-key = ["TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL", "5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF"]

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
bitcoin-wallet-password = 9da115c9fa6fe7fd09390841ac91aee4

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["03456772301e221026269d3095ab5cb623fc239835b583ae4632f99a15107ef275", "cSKyTeXidmj93dgbMFqgzD7yvxzA7QAYr5j9qDnY9seyhyv7gH2m"]
```

On config file recreation, default values will be assigned to some of the properties, but some of them need to be changed or added manually for more complex testing environment.

To find values you need to put into config file, for particular SON, you need to get full SON Account info and its private key, using cli\_wallet. Execute these commands with initialized wallet.

```text
# get SON info for SON owned by sonaccount01
get_son sonaccount01

# response will be similar to this
# get_son sonaccount01
# {
#   "id": "1.27.0",
#   "son_account": "1.2.36",
#   "vote_id": "3:38",
#   "total_votes": 0,
#   "url": "http://sonaddreess01.com",
#   "deposit": "1.13.1",
#   "signing_key": "TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL",
#   "pay_vb": "1.13.2",
#   "statistics": "2.24.0",
#   "status": "active",
#   "sidechain_public_keys": [[
#       "bitcoin",
#       "03456772301e221026269d3095ab5cb623fc239835b583ae4632f99a15107ef275"
#     ]
#   ]
# }

# Get the private key for signing_key of sonaccount01
unlocked >>> get_private_key TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL

# response will be similar to this
# get_private_key TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL
# "5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF"

# We have all we need
# son-id (1.27.0)
# public key ("TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL")
# private keys ("5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF")
```

Our config params should look like this now

```text
son-id = "1.27.0"
peerplays-private-key = ["TEST8TCQFzyYDp3DPgWZ24261fMPSCzXxVyoF3miWeTj6JTi2DZdrL","5K7tjTnFn2zasimVmebUiH1yawgwK9YXm2chUwcurcHgqms9JaF"]
```

Now, we need to create a new bitcoin address and get its public and private key, and add them to config file

```text
# create new address in a wallet
docker exec bitcoind-node bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" getnewaddress
# response will be similar to this
# 2N8vYSFg9MAghj5nNC2ZURYcZTKcck8M1PU

# get info about this address
docker exec bitcoind-node bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" getaddressinfo 2N8vYSFg9MAghj5nNC2ZURYcZTKcck8M1PU

# response will be similar to this
# {
#   "address": "2N8vYSFg9MAghj5nNC2ZURYcZTKcck8M1PU",
#   "scriptPubKey": "a914abf98238ba69bacc38a613405caaeee484f6810e87",
#   "ismine": true,
#   "solvable": true,
#   "desc": "sh(wpkh([153472fd/0'/0'/31']02a6d0cd88e927afc34da3fb364a681d42889547af70b1589069364febd3e0ac29))#k8z2zlkg",
#   "iswatchonly": false,
#   "isscript": true,
#   "iswitness": false,
#   "script": "witness_v0_keyhash",
#   "hex": "001432a344b250250dd279b1658b201181ccab890141",
#   "pubkey": "02a6d0cd88e927afc34da3fb364a681d42889547af70b1589069364febd3e0ac29",
#   "embedded": {
#     "isscript": false,
#     "iswitness": true,
#     "witness_version": 0,
#     "witness_program": "32a344b250250dd279b1658b201181ccab890141",
#     "pubkey": "02a6d0cd88e927afc34da3fb364a681d42889547af70b1589069364febd3e0ac29",
#     "address": "bcrt1qx235fvjsy5xay7d3vk9jqyvpej4cjq2plrqjk2",
#     "scriptPubKey": "001432a344b250250dd279b1658b201181ccab890141"
#   },
#   "label": "",
#   "ischange": false,
#   "timestamp": 1571845292,
#   "hdkeypath": "m/0'/0'/31'",
#   "hdseedid": "7fc94a298c785719434c63458bb41bc1995a270d",
#   "hdmasterfingerprint": "153472fd",
#   "labels": [
#     {
#       "name": "",
#       "purpose": "receive"
#     }
#   ]
# }

# pubkey is what we will set as bitcoin-private-key in config file

# get private key for this address
docker exec bitcoind-node bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet="" dumpprivkey 2N8vYSFg9MAghj5nNC2ZURYcZTKcck8M1PU

# response will be similar to this
# cT9CBy22gmyyXfZgRvVaddZodjFgeuN7R18tGJe9UZeAjizgY2z4

# this value is what we will set as bitcoin-private-key in config file

# We have all we need
# bitcoin public key ("02a6d0cd88e927afc34da3fb364a681d42889547af70b1589069364febd3e0ac29")
# bitcoin private key ("cT9CBy22gmyyXfZgRvVaddZodjFgeuN7R18tGJe9UZeAjizgY2z4")
```

Our config param should look like this now

```text
bitcoin-private-key = ["02a6d0cd88e927afc34da3fb364a681d42889547af70b1589069364febd3e0ac29", "cT9CBy22gmyyXfZgRvVaddZodjFgeuN7R18tGJe9UZeAjizgY2z4"]
```

## Configuring witness <a id="Configuring-witness"></a>

For more realistic test scenario, witnesses also needs to be configured with their own IDs, public and private keys.

[https://github.com/bitshares/bitshares-core/wiki/How-To-become-an-active-witness-in-BitShares-2.0\#becoming-a-witness](https://github.com/bitshares/bitshares-core/wiki/How-To-become-an-active-witness-in-BitShares-2.0#becoming-a-witness)



