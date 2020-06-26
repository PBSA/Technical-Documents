# Setting up the chain and using the CLI wallet through downloadable binaries

It is recommended to use Ubuntu 18.04 and 8GB ram to run this witness node.

## Downloading the Binaries

The quickest way to get started with the binaries is to look at the [GitLab CI/CD pipelines for Beatrice](https://gitlab.com/PBSA/peerplays/-/pipelines?page=1&scope=all&ref=beatrice) and download the artifacts for the [latest build](https://gitlab.com/PBSA/peerplays/-/jobs/599439620).

![](../../.gitbook/assets/image%20%2827%29.png)

## Running the Binaries

There are two binaries of interest: **witness\_node** and **cli\_wallet.** These are found in `./artifacts/programs/witness_node` and `./artifacts/programs/cli_wallet` respectively.

### Witness Node

1. Extract the witness\_node binary to some location with write permissions.
2. Run the witness node binary: `./witness_node`
3. The blockchain will now start to sync with the public TESTNET \(Beatrice\).

{% hint style="warning" %}
Consider creating a service to run this binary.
{% endhint %}

By default, running the witness\_node binary downloaded from the Beatrice branch build will configure the `witness_node_data_dir` and start synching with the Testnet. If configuration changes need to be made \(e.g, setting up your own network, specifying custom RPC ports to listen on, etc...\), the `witness_node_data_dir/config.ini` should be modified.

```text
### Default config.ini for Beatrice

# Endpoint for P2P node to listen on
# p2p-endpoint = 

# P2P nodes to connect to on startup (may specify multiple times)
# seed-node = 

# JSON array of P2P nodes to connect to on startup
# seed-nodes = 

# Pairs of [BLOCK_NUM,BLOCK_ID] that should be enforced as checkpoints.
# checkpoint = 

# Endpoint for websocket RPC to listen on
rpc-endpoint = 127.0.0.1:8090

# Endpoint for TLS websocket RPC to listen on
# rpc-tls-endpoint = 

# The TLS certificate file for this server
# server-pem = 

# Password for this certificate
# server-pem-password = 

# File to read Genesis State from
# genesis-json = 

# Block signing key to use for init witnesses, overrides genesis file
# dbg-init-key = 

# JSON file specifying API permissions
# api-access = 

# Whether to enable tracking of votes of standby witnesses and committee members. Set it to true to provide accurate data to API clients, set to false for slightly better performance.
# enable-standby-votes-tracking = 

# Space-separated list of plugins to activate
# plugins = 

# Enable block production, even if the chain is stale.
enable-stale-production = false

# Percent of witnesses (0-99) that must be participating in order to produce blocks
required-participation = false

# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
# witness-id = 

# IDs of multiple witnesses controlled by this node (e.g. ["1.6.5", "1.6.6"], quotes are required)
# witness-ids = 

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["TEST6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"]

# Account ID to track history for (may specify multiple times)
# track-account = 

# Keep only those operations in memory that are related to account history tracking
partial-operations = 1

# Maximum number of operations per account will be kept in memory
max-ops-per-account = 100

# Elastic Search database node url(http://localhost:9200/)
# elasticsearch-node-url = 

# Number of bulk documents to index on replay(10000)
# elasticsearch-bulk-replay = 

# Number of bulk documents to index on a syncronied chain(100)
# elasticsearch-bulk-sync = 

# Use visitor to index additional data(slows down the replay(false))
# elasticsearch-visitor = 

# Pass basic auth to elasticsearch database('')
# elasticsearch-basic-auth = 

# Add a prefix to the index(peerplays-)
# elasticsearch-index-prefix = 

# Save operation as object(false)
# elasticsearch-operation-object = 

# Start doing ES job after block(0)
# elasticsearch-start-es-after-block = 

# Save operation as string. Needed to serve history api calls(true)
# elasticsearch-operation-string = 

# Mode of operation: only_save(0), only_query(1), all(2) - Default: 0
# elasticsearch-mode = 

# Elasticsearch node url(http://localhost:9200/)
# es-objects-elasticsearch-url = 

# Basic auth username:password('')
# es-objects-auth = 

# Number of bulk documents to index on replay(10000)
# es-objects-bulk-replay = 

# Number of bulk documents to index on a synchronized chain(100)
# es-objects-bulk-sync = 

# Store proposal objects(true)
# es-objects-proposals = 

# Store account objects(true)
# es-objects-accounts = 

# Store asset objects(true)
# es-objects-assets = 

# Store balances objects(true)
# es-objects-balances = 

# Store limit order objects(true)
# es-objects-limit-orders = 

# Store feed data(true)
# es-objects-asset-bitasset = 

# Add a prefix to the index(ppobjects-)
# es-objects-index-prefix = 

# Keep only current state of the objects(true)
# es-objects-keep-only-current = 

# Start doing ES job after block(0)
# es-objects-start-es-after-block = 

# Track market history by grouping orders into buckets of equal size measured in seconds specified as a JSON array of numbers
bucket-size = [15,60,300,3600,86400]

# How far back in time to track history for each bucket size, measured in the number of buckets (default: 1000)
history-per-size = 1000

# Block number after which to do a snapshot
# snapshot-at-block = 

# Block time (ISO format) after which to do a snapshot
# snapshot-at-time = 

# Pathname of JSON file where to store the snapshot
# snapshot-to = 


# ==============================================================================
# logging options
# ==============================================================================
#
# Logging configuration is loaded from logging.ini by default.
# If logging.ini exists, logging configuration added in this file will be ignored.
```

### CLI Wallet

Once your local chain instance is in sync with the network, you can start using the CLI wallet on it. 

{% hint style="info" %}
If you want to get started with the CLI Wallet right away, you can specify the `server-rpc-endpoint` to be that of an active witness node. A great place to find active endpoints is to check out: [https://beta.eifos.org/status](https://beta.eifos.org/status) under `Testnet.`
{% endhint %}

1. Extract the cli\_wallet binary to some location with write permissions.
2. Run the cli\_wallet binary: `./cli_wallet --wallet-file my-wallet.json --chain-id b3f7fe1e5ad0d2deca40a626a4404524f78e65c3a48137551c33ea4e7c365672 --server-rpc-endpoint ws://127.0.0.1:8090 -u '' -p ''`
3. You will then be asked to initialize your wallet. Set a password by using: `set_password mypassword` 
4. Unlock the wallet: `unlock mypassword` 
5. You can now execute the various functions of the wallet. Use `help` to see a list of commands with parameters.

Some Useful commands to start with:

* Get information about the entire chain: `get_global_properties`
* Get information about the core asset \(TEST\): `get_asset 1.3.0`
* List the asset balance of an account: `list_account_balances faucet`
* Importing an account into the CLI Wallet that was created with the GUI wallet or faucet with a password: 
  * `get_private_key_from_password(string account, string role, string password)` There are three roles: `active, owner, and memo`
  * `import_key(string account_name_or_id, string wif_key)` for each WIF from the different roles obtained from `get_private_key_from_password`

{% hint style="info" %}
The chain-ID for Beatrice is: b3f7fe1e5ad0d2deca40a626a4404524f78e65c3a48137551c33ea4e7c365672

To find the chain-ID for your local instance \(it will be different if you've set up your own network\) run: 

| `sudo curl --data '{"jsonrpc": "2.0", "method": "get_chain_properties", "params": [], "id": 1}'` `http://127.0.0.1:8090/rpc && echo` |
| :--- |
{% endhint %}

