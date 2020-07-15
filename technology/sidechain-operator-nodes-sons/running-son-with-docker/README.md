# Running SON with Docker

## Clone the peerplays-docker repository 

```text
git clone https://gitlab.com/data-security-node/peerplays-docker.git -b release
```

## Edit the configuration

###  Setting up the environment

Copy the `example.env` to `.env`  located in the root of the repository

```text
cp example.env .env
```

Edit the `BTC_REGTEST_CONF` to the full path where the `bitcoin.conf` is located. There is a `bitcoin.conf` file located in `peerplays-docker/bitcoin/regtest/bitcoin.conf`

### Setting up config.ini

For a detailed overview: check out: [SON Configuration](../son-configuration.md)

Copy the `/peerplays-docker/data/witness_node_data_dir/config.ini.son.example` to `/peerplays-docker/data/witness_node_data_dir/config/config.ini`

```text
cd data/witness_node_data_dir
cp config.ini.son.example config.ini
```

Any custom changes to `SON_WALLET` or `BTC_REGTEST_KEY` should also be made in this file. `bitcoin-wallet` and `bitcoin-private-key`

{% hint style="info" %}
By default, the config specifies seed nodes to connect to. If a new network is required, change to`seed-nodes=[]`
{% endhint %}

## Install the peerplays:son image

Use `run.sh` to pull the SON image

```text
./run.sh install son
```

## Start the environment

Once the configuration is setup, use `run.sh` to start the peerplaysd and bitcond containers.

```text
./run.sh start_son_regtest
```

The SON network will be created and the seed \(peerplaysd\) and bitcoind-node \(bitcoind\) containers will be launched. 



### CLI\_Wallet

Now, run the below docker exec command on terminal:

```text
docker exec -it seed cli_wallet
```

Copy the remote\_chain\_id and mapped it in the wallet with below command:

```text
./cli_wallet --chain-id=<CHAIN_ID>
```

Set the wallet password:

```text
set_password <password>
```

Unlock the Cli\_wallet:

```text
unlock <password>
```

#### Now, you are under the cli\_wallet and you successfully unlock your wallet. And wallet is ready to run the cli\_commands and for transactions <a id="Installing-and-starting-witness-services"></a>



