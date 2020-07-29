# Running SON with Docker

## Cloning the peerplays docker repository 

```text
git clone https://gitlab.com/data-security-node/peerplays-docker.git -b release
```

## Editing the configuration

###  Setting up the environment

Copy the `example.env` to `.env`  located in the root of the repository:

```text
# Starting in the project root
cp example.env .env
```

Edit the `BTC_REGTEST_CONF` to the full path where the `bitcoin.conf` is located. 

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

### Setting up config.ini

For a detailed overview: check out: [SON Configuration](../son-configuration.md)

Copy the `/peerplays-docker/data/witness_node_data_dir/config.ini.son.example` to `/peerplays-docker/data/witness_node_data_dir/config/config.ini`

```text
# Starting in the project root
cd data/witness_node_data_dir
cp config.ini.son.example config.ini
```

Any custom changes to `SON_WALLET` or `BTC_REGTEST_KEY` should also be made in this file. `bitcoin-wallet` and `bitcoin-private-key`

{% hint style="warning" %}
By default, the config specifies a new local network \(seed nodes in the config are empty\). To connect to other nodes on a public SON network change `seed-nodes` to specify a seed from that network.
{% endhint %}

## Installing the peerplays:son image

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

If using a new network \(default\) run the setup script which will create a wallet file \(wallet.json\) locked with a given password within the container and fund the `nathan` and `init0 - init11` witness accounts. 

{% hint style="danger" %}
Before continuing with a new network and using the setup script, make sure to import the default bitcoin keys. There is a script provided which does this:

```text
# Starting in the project root
./scripts/regtest/import_btc_keys.sh
```
{% endhint %}

```text
# In the local terminal
docker exec seed /peerplays/setup_blockchain.sh <YOUR-WALLET-PASSWORD>
```

In the terminal use `docker exec` to connect to the wallet.

```text
# In the local terminal
docker exec -it seed cli_wallet
```

If an exception is thrown and contains `Remote server gave us an unexpected chain_id`, then copy the `remote_chain_id` that is provided by it. 

Pass the chain ID to the CLI wallet:

```text
# In the local terminal
docker exec -it seed cli_wallet --chain-id=<CHAIN-ID>
```

Unlock the CLI wallet by providing the password set earlier:

```text
# In the CLI wallet
unlock <YOUR-WALLET-PASSWORD>
```

The CLI wallet is now ready to be used.

For information on end-to-end BTC transactions with SON [proceed to the next steps](bitcoin-transactions.md). 

## Cleaning up the environment

To remove the containers and data from the environment run:

```text
# Starting in the project root
./run.sh clean son
```

