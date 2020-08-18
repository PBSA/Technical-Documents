# Connecting to an existing network

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

Any custom changes to `SON_WALLET` or `BTC_REGTEST_KEY` should also be made in this file. `bitcoin-wallet` and `bitcoin-private-key`.

{% hint style="warning" %}
To connect to other nodes on a public SON network change `seed-nodes` to specify a seed from that network.

To connect to PBSA's Gladiator set the seed nodes to their endpoints:

```text
seed-nodes=["96.46.49.3:9777", "96.46.49.4:9777"]
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

