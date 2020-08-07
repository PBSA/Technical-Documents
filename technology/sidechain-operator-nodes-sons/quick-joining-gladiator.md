---
description: Setup SONs and join GLADIATOR public TESTNET
---

# Quick joining GLADIATOR

This is a quick document which assumes that the user has experience in setting up various Graphene blockchains before. The following link can be a good refresher: [https://www.peerplays.tech/witnesses/becoming-a-witness](https://www.peerplays.tech/witnesses/becoming-a-witness)

In this document the exectables are downloaded for the Gitlab CI-CD pipeline

### 1. Prepare the server

The following dependencies are necessary for a clean Ubuntu 18.04 LTS

```text
sudo apt-get install autoconf bash build-essential ca-certificates cmake \
     doxygen git graphviz libbz2-dev libcurl4-openssl-dev libncurses-dev \
     libreadline-dev libssl-dev libtool libzmq3-dev locales ntp pkg-config \
     wget
```

### 2. Download SON executable

Go to [https://gitlab.com/PBSA/peerplays/-/jobs](https://gitlab.com/PBSA/peerplays/-/jobs) and find the job ID for the build you want to use. Build should have the following properties

* Job: features/SONs-base
* Stage: build
* Name: build

Example of such a job has ID 467332251:

{% embed url="https://gitlab.com/PBSA/peerplays/-/jobs/467332251" %}

To download executables, click Download button on the right side of the Job page, or execute the following command:  


```text
wget --content-disposition --show-progress\
 https://gitlab.com/PBSA/peerplays/-/jobs/538784707/artifacts/download
```

To unpack executables in current folder:

```text
unzip -j artifacts.zip build/programs/cli_wallet/cli_wallet -d ./
unzip -j artifacts.zip build/programs/witness_node/witness_node -d ./
```

### 

Execute the `witness_node` binary which will create the necessary files and folders under `witness_node_data_dir`

```text
./witness_node
```

Stop the node \(`CTRL + c` \)  and edit `config.ini` to configure the node.

### Connecting to PBSA's Gladiator Testnet

Inside `config.ini`, set the `seed-nodes` to:

```text
seed-nodes=["96.46.49.1:9777", "96.46.49.2:9777", "96.46.49.3:9777", "96.46.49.4:9777", "96.46.49.5:9777", "96.46.49.6:9777", "96.46.49.7:9777", "96.46.49.8:9777", "96.46.49.9:9777", "96.46.49.10:9777", "96.46.49.11:9777", "96.46.49.12:9777", "96.46.49.13:9777", "96.46.49.14:9777", "96.46.49.15:9777", "96.46.49.16:9777"]
```

{% hint style="danger" %}
In order to sync with PBSA's Gladiator Testnet, the genesis file must be exactly the same as used by the witness nodes. This file can be downloaded here: [https://drive.google.com/file/d/1YmDbwUB-5D5vGzc9vYEva8yLkTkwva8r/view?usp=sharing](https://drive.google.com/file/d/1YmDbwUB-5D5vGzc9vYEva8yLkTkwva8r/view?usp=sharing)

```text
wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=1YmDbwUB-5D5vGzc9vYEva8yLkTkwva8r' -O SONs_genesis.json
```

Move the `genesis.json` file to the root of the project directory alongside the `witness_node` binary.
{% endhint %}

Inside `config.ini`, specify the `genesis.json` 

```text
genesis-json = genesis.json
```

Start the `witness_node` and the blocks should start syncing.

```text
./witness_node --gensis-file SONs_genesis.json
```

{% hint style="warning" %}
If blocks have already been seeded during the initial startup, it may be necessary to reset the `blockchain` and `p2p` directories. Removing them is fine for this case.

```text
rm -rf witness_node_data_dir/blockchain
rm -rf witness_node_data_dir/p2p
```
{% endhint %}

At this point you will be able download a copy of the blockchain. Additional steps are required to run the cli\_wallet and also become a SON.

### SON configuration

#### Prerequisites

* Your node is up and running and synced with the network.
* You have access to a bitcoin node, with RPC and ZMQ notifications enabled
* You have access to shared wallet used by all SONs \(prerelease requirement only\)

#### Configuration files

SON plugin is controlled by following parameters

```text
peerplays_sidechain plugin. <no description>
Options:
  --son-id arg                          ID of SON controlled by this node (e.g.
                                        "1.27.5", quotes are required)
  --son-ids arg                         IDs of multiple SONs controlled by this
                                        node (e.g. ["1.27.5", "1.27.6"], quotes
                                        are required)
  --peerplays-private-key arg (=["TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"])
                                        Tuple of [PublicKey, WIF private key] 
                                        (may specify multiple times)
  --bitcoin-node-ip arg (=99.79.189.95) IP address of Bitcoin node
  --bitcoin-node-zmq-port arg (=11111)  ZMQ port of Bitcoin node
  --bitcoin-node-rpc-port arg (=22222)  RPC port of Bitcoin node
  --bitcoin-node-rpc-user arg (=1)      Bitcoin RPC user
  --bitcoin-node-rpc-password arg (=1)  Bitcoin RPC password
  --bitcoin-wallet arg                  Bitcoin wallet
  --bitcoin-wallet-password arg         Bitcoin wallet password
  --bitcoin-private-key arg (=["02d0f137e717fb3aab7aff99904001d49a0a636c5e1342f8927a4ba2eaee8e9772","cVN31uC9sTEr392DLVUEjrtMgLA8Yb3fpYmTRj7bomTm6nn2ANPr"])
                                        Tuple of [Bitcoin public key, Bitcoin 
                                        private key] (may specify multiple 
                                        times)
  
```

These parameters are available from both command line and config file:

Edit add the following to your config.ini file

```text
# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================

# ID of SON controlled by this node (e.g. "1.27.5", quotes are required)
son-id = "1.27.0"

# IDs of multiple SONs controlled by this node (e.g. ["1.27.5", "1.27.6"], quotes are required)
son-ids = []

# Tuple of [PublicKey, WIF private key]
peerplays-private-key = ["TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV", "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]

# IP address of Bitcoin node
bitcoin-node-ip = 99.79.189.95

# ZMQ port of Bitcoin node
bitcoin-node-zmq-port = 11111

# RPC port of Bitcoin node
bitcoin-node-rpc-port = 22222

# Bitcoin RPC user
bitcoin-node-rpc-user = 1

# Bitcoin RPC password
bitcoin-node-rpc-password = 1

# Bitcoin wallet
bitcoin-wallet = son-wallet

# Bitcoin wallet password
bitcoin-wallet-password = 9da115c9fa6fe7fd09390841ac91aee4

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["02d0f137e717fb3aab7aff99904001d49a0a636c5e1342f8927a4ba2eaee8e9772","cVN31uC9sTEr392DLVUEjrtMgLA8Yb3fpYmTRj7bomTm6nn2ANPr"]

```

**Edit the config file and add the RPC port**

```text
# Endpoint for websocket RPC to listen on
rpc-endpoint = 127.0.0.1:8090
```

### Becoming a SON

Becoming a SON is very similar to becoming a witness. You will need:

* Active user account, upgraded to lifetime member, which will be the owner of SON account
* Create two vesting balances \(types son and normal\) of 50 core assets, and get its IDs
* Create Bitcoin address for SON account in shared SON wallet
* Create SON account, and get its ID
* Set the signing key for a son account \(usually, its a signing key of owner account\)
* Set the bitcoin address as a sidechain address for a SON account
* Update your config file with values obtained in previous steps, and restart the witness with peerplays\_sidechain plugin enabled

Example:

```text
# ================================================================================
# Active user account, upgraded to lifetime member, which will be the owner of SON account
unlocked >>> get_account account07
get_account account07
{
  "id": "1.2.58",
  "membership_expiration_date": "2106-02-07T06:28:15",
  "registrar": "1.2.58",
  "referrer": "1.2.58",
  "lifetime_referrer": "1.2.58",
  "network_fee_percentage": 2000,
  "lifetime_referrer_fee_percentage": 8000,
  "referrer_rewards_percentage": 0,
  "name": "account07",
  "owner": {
    "weight_threshold": 1,
    "account_auths": [],
    "key_auths": [[
        "TEST86obxk1fqh8cmDkRdgyVkzHtcmqcByva7tk9DYAjCpLPkxqeCC",
        1
      ]
    ],
    "address_auths": []
  },
  "active": {
    "weight_threshold": 1,
    "account_auths": [],
    "key_auths": [[
        "TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB",
        1
      ]
    ],
    "address_auths": []
  },
...
}
# ================================================================================

# ================================================================================
# Create two vesting balances (types son and normal) of 50 core assets, and get its IDs
unlocked >>> create_vesting_balance account07 50 TEST son true
create_vesting_balance account07 50 TEST son true
{
  "ref_block_num": 3977,
  "ref_block_prefix": 145563165,
  "expiration": "2020-03-13T17:08:45",
  "operations": [[
      32,{
...
}

unlocked >>> create_vesting_balance account07 50 TEST normal true
create_vesting_balance account07 50 TEST normal true
{
  "ref_block_num": 3979,
  "ref_block_prefix": 1838302122,
  "expiration": "2020-03-13T17:08:51",
  "operations": [[
      32,{
...
}

unlocked >>> get_vesting_balances account07
get_vesting_balances account07
[{
    "id": "1.13.79",
    "owner": "1.2.58",
...
  },{
    "id": "1.13.80",
    "owner": "1.2.58",
...
  }
]
# ================================================================================

# ================================================================================
# Create Bitcoin address for SON account in shared SON wallet
bitcoin-core.cli -rpcconnect=99.79.189.95 -rpcport=22222 -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" getnewaddress
2NCGVYMNjSSwYrxGAFv53WQP6i7xu8XHEVn

bitcoin-core.cli -rpcconnect=99.79.189.95 -rpcport=22222 -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" walletpassphrase 9da115c9fa6fe7fd09390841ac91aee4 60

bitcoin-core.cli -rpcconnect=99.79.189.95 -rpcport=22222 -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" dumpprivkey 2NCGVYMNjSSwYrxGAFv53WQP6i7xu8XHEVn
cSmQ517iJaAT94SMAsLhtyicZcFggY54gg5aLCUDuwUA3ECftP24

bitcoin-core.cli -rpcconnect=99.79.189.95 -rpcport=22222 -rpcuser=1 -rpcpassword=1 -rpcwallet="son-wallet" getaddressinfo 2NCGVYMNjSSwYrxGAFv53WQP6i7xu8XHEVn
{
  "address": "2NCGVYMNjSSwYrxGAFv53WQP6i7xu8XHEVn",
  "scriptPubKey": "a914d0a7c9ec2c89c67df9c1d8dce8d786ec9a0afcac87",
  "ismine": true,
  "solvable": true,
  "desc": "sh(wpkh([c9cc7580/0'/0'/3']03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2))#4g9d62sg",
  "iswatchonly": false,
  "isscript": true,
  "iswitness": false,
  "script": "witness_v0_keyhash",
  "hex": "0014d244cd683056c918a4d8f5cb1adce7a86558e253",
  "pubkey": "03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2",
...
}
# ================================================================================

# ================================================================================
# Create SON account, and get its ID
unlocked >>> create_son account07 "http://www.account07.com" 1.13.79 1.13.80 [[bitcoin, 03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2]] true
create_son account07 "http://www.account07.com" 1.13.79 1.13.80 [[bitcoin, 03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2]] true
{
  "ref_block_num": 4171,
  "ref_block_prefix": 2156837961,
  "expiration": "2020-03-13T17:20:06",
  "operations": [[
      82,{
        "fee": {
          "amount": 0,
          "asset_id": "1.3.0"
        },
        "owner_account": "1.2.58",
        "url": "http://www.account07.com",
        "deposit": "1.13.79",
        "signing_key": "TEST71FaAsTFbKyoGM7USqzS9uG78jjRv7wJnTytxyXr7CTnrHeAHJ",
        "sidechain_public_keys": [[
            "bitcoin",
            "03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2"
          ]
        ],
        "pay_vb": "1.13.80"
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "1f03b014fadc66110bef63125967a93da577cdd598ca8a1ffebf36b7e21fdfc3b85587e7f3e5604b336c6fc0dd3ab3d109022c1996b77a13a374349ecc9e1e1fdd"
  ]
}

unlocked >>> get_son account07
get_son account07
{
  "id": "1.27.16",
  "son_account": "1.2.58",
  "vote_id": "3:54",
  "total_votes": 0,
  "url": "http://www.account07.com",
  "deposit": "1.13.79",
  "signing_key": "TEST71FaAsTFbKyoGM7USqzS9uG78jjRv7wJnTytxyXr7CTnrHeAHJ",
  "pay_vb": "1.13.80",
  "statistics": "2.24.16",
  "status": "inactive",
  "sidechain_public_keys": [[
      "bitcoin",
      "03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2"
    ]
  ]
}
# ================================================================================

# ================================================================================
# Set the signing key for a son account (usually, its a signing key of owner account)
# Set the bitcoin address as a sidechain address for a SON account

unlocked >>> get_private_key TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB
get_private_key TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB
"5JKvPJkerMNVEubsbKN8Xd8wGaU1ifhv7xAwy9gFJP6yMEoTkSd"

unlocked >>> update_son account07 "" "TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB" [[bitcoin, 03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2]] true
update_son account07 "" "TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB" [[bitcoin, 03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2]] true
{
  "ref_block_num": 4210,
  "ref_block_prefix": 4294628244,
  "expiration": "2020-03-13T17:22:21",
  "operations": [[
      83,{
        "fee": {
          "amount": 0,
          "asset_id": "1.3.0"
        },
        "son_id": "1.27.16",
        "owner_account": "1.2.58",
        "new_signing_key": "TEST7SUmjftH3jL5L1YCTdMo1hk5qpZrhbo4MW6N2wWyQpjXkT7ByB",
        "new_sidechain_public_keys": [[
            "bitcoin",
            "03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2"
          ]
        ]
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "1f43fccecb7994fbb6138fd85ccb1b462d4ffeaa696cbe0738e0a3eaf99d1eb65c4c8e995db5c39a293d8f3056757ab199eb8b99a2972b0a9b56057e875ea4c790"
  ]
}
# ================================================================================

# ================================================================================
# Update your config file with values obtained in previous steps, and restart the witness with peerplays_sidechain plugin enabled
# ================================================================================

# Open your config file, and find options plugins

# Space-separated list of plugins to activate
plugins = witness account_history market_history accounts_list affiliate_stats bookie

# Update the line, and add peerplays_sidechain to the list of enabled plugins

# Space-separated list of plugins to activate
plugins = witness account_history market_history accounts_list affiliate_stats bookie peerplays_sidechain


# Set son-id parameter to the ID of your SON
son-id = "1.27.16"

# Set son-ids parameter to empty array
son-ids = []

# Set peerplays-private-key to the key pair belonging to the SON owner account
peerplays-private-key = ["TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV", "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]

# Leave default value
bitcoin-node-ip = 99.79.189.95

# Leave default value
bitcoin-node-zmq-port = 11111

# Leave default value
bitcoin-node-rpc-port = 22222

# Leave default value
bitcoin-node-rpc-user = 1

# Leave default value
bitcoin-node-rpc-password = 1

# Leave default value
bitcoin-wallet = son-wallet

# Leave default value
bitcoin-wallet-password = 9da115c9fa6fe7fd09390841ac91aee4

# Set bitcoin-private-key to the keypair of the bitcoin address you created for a SON in shared wallet
bitcoin-private-key = ["03eac07563aa5280a6c1db50724ffc51edca1458d3ed9a61c7455fb16f35ddccd2","cSmQ517iJaAT94SMAsLhtyicZcFggY54gg5aLCUDuwUA3ECftP24"]

# Restat the node with new configuration
```

### Starting the node

```text
witness_node 
# If you need the logs, the following can be helpful
# witness_node 2>&1 peerplays.log
```

## Using the CLI wallet

In the terminal execute the CLI wallet:

```text
# In the local terminal
./cli_wallet
```

{% hint style="warning" %}
I**f an exception is thrown** and contains `Remote server gave us an unexpected chain_id`, then copy the `remote_chain_id` that is provided by it. 

Pass the chain ID to the CLI wallet:

```text
# In the local terminal
./cli_wallet --chain-id=<CHAIN-ID>
```
{% endhint %}

{% hint style="info" %}
There is optionally a flag that can be passed in to connect to a remote rpc endpoint. 

```text
./cli_wallet --server-rpc-endpoint ws://96.46.49.3:8090 -u '' -p ''
```
{% endhint %}

Enter a password for the CLI wallet:

```text
# In the CLI wallet
set_password <YOUR-WALLET-PASSWORD>
```

Unlock the CLI wallet by providing the password set earlier:

```text
# In the CLI wallet
unlock <YOUR-WALLET-PASSWORD>
```

{% hint style="info" %}
The CLI wallet will show `unlocked >>>` when successfully unlocked
{% endhint %}

The CLI wallet is now ready to be used.

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

## 

