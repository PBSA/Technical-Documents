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

### Install Bitcoin testnet network <a id="Install-Bitcoin-testnet-network"></a>

To install Bitcoin testnet network you should use script install-bitcoin-testnet-network.sh  
Script execution might last 10-20 minutes. Full chain sync needs more than few hour

```text

# Go to scripts
cd scripts/

# Kill all eventually running bitcoin services on all nodes
export CMD="killall bitcoind -9; sudo service bitcoin stop;"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Run the script
./install-bitcoin-testnet-network.sh

# The script will execute numerous commands on all nodes.
# Total execution time should not be longer than 20 minutes
# Script will create and start bitcoin services, create default and son wallets,
# import default addresses, public and private keys to the appropriate wallets.

# Once the script finishes, you will have:
# - Testnet BTC node on 16 nodes
# - 16 default wallets with no funds
# - 16 encrypted wallets named "son-wallet" with no funds

# To veryfing that the network is up and running, we will use getblockcount command
# Block count returned must be the same on all nodes. If that is the case, it means that new network 


export CMD="/snap/bitcoin-core/63/bin/bitcoin-cli -rpcuser=1 -rpcpassword=1 getblockcount"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Response:
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.1 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.2 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.3 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.4 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.5 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.6 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.7 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.8 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.9 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.10 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.11 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.12 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.13 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.14 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.15 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1722792
Connection to 96.46.49.16 closed.



# You can also read the balances of all default wallets.
# All wallets will have 910 or more BTC

export CMD="/snap/bitcoin-core/63/bin/bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet= getbalance"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Response:
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.1 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.2 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.3 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.4 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.5 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.6 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.7 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.8 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.9 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.10 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.11 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.12 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.13 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.14 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.15 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
0.00000000
Connection to 96.46.49.16 closed.
```



### Install Bitcoin regtest network <a id="Install-Bitcoin-regtest-network"></a>

To install Bitcoin regtest network you should use script install-bitcoin-regtest-network.sh  
Script execution might last 10-20 minutes

```text
# Go to scripts
cd scripts/

# Kill all eventually running bitcoin services on all nodes
export CMD="killall bitcoind -9; sudo service bitcoin stop;"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Run the script
./install-bitcoin-regtest-network.sh

# The script will execute numerous commands on all nodes.
# Total execution time should not be longer than 20 minutes
# Script will create and start bitcoin services, create default and son wallets,
# import default addresses, public and private keys to the appropriate wallets.
# It will also mine some bitcoins, and distribute them to default wallets.
# It will also create a cron task, that will simulate mining.
# One block will be produced by different node each 10 minutes.

# Once the script finishes, you will have:
# - Regtest network of 16 nodes
# - 16 default wallets with 910 or more BTC each, assigned to each user withdraw addresses
# - 16 encrypted wallets named "son-wallet", each containing just a single SON private key, and no funds



# To veryfing that the network is up and running, we will use getblockcount command
# Block count returned must be the same on all nodes. If that is the case, it means that new network 


export CMD="/snap/bitcoin-core/63/bin/bitcoin-cli -rpcuser=1 -rpcpassword=1 getblockcount"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Response:
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.1 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.2 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.3 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.4 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.5 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.6 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.7 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.8 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.9 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.10 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.11 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.12 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.13 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.14 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.15 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
960
Connection to 96.46.49.16 closed.



# You can also read the balances of all default wallets.
# All wallets will have 910 or more BTC

export CMD="/snap/bitcoin-core/63/bin/bitcoin-cli -rpcuser=1 -rpcpassword=1 -rpcwallet= getbalance"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD

# Response:
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
1004.67244960
Connection to 96.46.49.1 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.2 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.3 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.4 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.5 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.6 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.7 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.8 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.9 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.10 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.11 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.12 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.13 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.14 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.15 closed.
///////////////////////////////////////////////////////////////////////////////
// Use of this system is restricted. Unauthorized access is prohibited and   //
// subject to the penalties of the law !! All information and communications //
// about this system are subject to review, monitoring and recording         //
// at any time without notice or authorization !!                            //
///////////////////////////////////////////////////////////////////////////////
910.00000000
Connection to 96.46.49.16 closed.
 
```



### Install Peerplays witness/SON network <a id="Install-Peerplays-witness/SON-network"></a>

To install Peerplays witness/SON network you should use several scripts:

* create-setup-package.sh to create setup package that will be installed on all nodes
* upload-son-config-to-nodes.sh to upload and unpack setup package to all nodes
* install-son-config-services.sh to install and run systemd services for witnesses on all nodes
* init-network.sh to initialize network to default state, ready for testing
* upload-wallet-to-nodes.sh to upload initialized wallet file to all nodes

You will also need Gitlab job id, of the software version you want to deploy.  
Go to [https://gitlab.com/PBSA/peerplays/-/jobs](https://gitlab.com/PBSA/peerplays/-/jobs) and find the id of the job you want to deploy. Only jobs with stage/name “build” are deployable.

Scripts execution might last 10-20 minutes.



#### **Creating setup package**

```text
# Visit gitlab jobs page, and find appropriate job id
# Eg https://gitlab.com/PBSA/peerplays/-/jobs/492479590
# Job id is 492479590


# Go to scripts
cd scripts/

# Create setup package for job id 492479590
./create-setup-package.sh 492479590

# Script will
# - Download artifacts from the given job,
# - Unpack cli_wallet and witness_node,
# - Create default genesis and config files
# - Create finetuned config files for 16 nodes
# - Create helper scripts
# - Pack executables and config files in archive named son-config-492479590.tar.gz
#   This package is uploaded to nodes using script upload-son-config-to-nodes.sh

# Script is done when you see the following message
Setup package created!
- Copy son-config-492479590.tar.gz to all 16 nodes
- Unpack son-config-492479590.tar.gz in /var/opt/
- Executables and configuration files will be located in /var/opt/492479590
- Run install-service.sh to install witness service
- Run uninstall-service.sh to uninstall witness service
- Run delete-database.sh to delete blockchain database
```



#### Uploading setup package to nodes <a id="Uploading-setup-package-to-nodes"></a>

```text
# Uploading setup package for job id 492479590 to nodes
./upload-son-config-to-nodes.sh 492479590

# Script will
# - Upload son-config-492479590.tar.gz to all nodes
# - Unpack son-config-492479590.tar.gz on all nodes

# To verify that package is uploaded and unpacked
export CMD="ls -al /var/opt/492479590"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD;

# As each setup package is unpacked into separate folder, with job id as name,
# we are able to have several versions deployed at the same time, and we are able to
# switch between the versions in a matter of minutes.
# Although more version may be deployed, they cant be active at the same time
# Only one can run
```

####  <a id="Installing-and-starting-witness-services"></a>

#### Installing and starting witness services <a id="Installing-and-starting-witness-services"></a>

```text
# Installing and running systemd services for witnesses
./install-son-config-services.sh 492479590

# Script will
# - Run helper script uninstall-service.sh that will stop and uninstall any
#   previously installed witness service.
# - Run helper script install-service.sh that will install systemd service for
#   witness node. This script will identify computer it is running on by IP address
#   and install systemd service with appropriate configuration on it
#   (son01 on gladiator1, son02 on gladiator2, ..., son16 on gladiator16)
# - Start witness service

# If we have multiple software versions deployed, executing this script with
# different job id is enough to switch between deployed versions
```

#### Network initialization <a id="Network-initialization"></a>

```text
# Initializing network to defautl state, ready for testing
# Script execution will last up to a minute
# Make sure that you have enough time to finish execution before maintenance block

# Pickup the chain ID
curl --silent --data '{"jsonrpc": "2.0", "method": "get_chain_properties", "params": [], "id": 1}' http://127.0.0.1:8090/rpc | jq -r ".result.chain_id"

cd /var/opt
ln -s /home/machine_name/scripts/init-network.sh
cd /var/opt/492479590
../init-network.sh CHAIN-ID

# Script will
# - Set default password and import nathan private key
# - Create accounts for new witnesses, sons and users
# - Transfer some core funds from nathan to all other accounts
# - Creat ethree example assets for testing purposes
# - Distribute newly created assets to user accounts
# - Upgrade accounts for witnesses and sons to lifetime members
# - Add sidechain addresses for user accounts (for deposit and withdrawal)
# - Create new witnesses, and update their signing keys
# - Create vesting balances for SONs
# - Create SONs

# Once the script is finish executing, wait for the maintenance block
```

#### Uploading initialized wallet to all nodes <a id="Uploading-initialized-wallet-to-all-nodes"></a>

```text
# Once the network initialization is done, the wallet on gladiator1 contains
# all the keys for all the users
# To avoid importing all keys to wallets on other 15 nodes, we simply copy
# wallet file from gladiator1 to all other nodes

cd /home/machine_name/scripts
./upload-wallet-to-nodes.sh 492479590
```

### Restarting clean chain on existing setup <a id="Restarting-clean-chain-on-existing-setup"></a>

Restarting clean chain is done by stopping witness services on all nodes, deleting existsing databases on all nodes, and restarting the witness services on all nodes.

```text
# Make sure you know the job id of the setup you want to restart
# Update the command with wanted job id

# Kill nodes, delete databases
export CMD="killall witness_node -9; sudo service witness stop; cd /var/opt/492479590; ./delete-database.sh;"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD;

# Restart services
export CMD="sudo systemctl daemon-reload; sudo service witness start;"; ssh -t gladiator01 $CMD; ssh -t gladiator02 $CMD; ssh -t gladiator03 $CMD; ssh -t gladiator04 $CMD; ssh -t gladiator05 $CMD; ssh -t gladiator06 $CMD; ssh -t gladiator07 $CMD; ssh -t gladiator08 $CMD; ssh -t gladiator09 $CMD; ssh -t gladiator10 $CMD; ssh -t gladiator11 $CMD; ssh -t gladiator12 $CMD; ssh -t gladiator13 $CMD; ssh -t gladiator14 $CMD; ssh -t gladiator15 $CMD; ssh -t gladiator16 $CMD;
```





