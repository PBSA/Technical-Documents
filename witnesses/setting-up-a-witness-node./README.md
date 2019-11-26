# Setting up a Witness node.

This is an introduction to get new Witnesses up to speed on the Peerplays blockchain. It is intended for Witnesses planning to join a live, already deployed, blockchain.

The following repository should be used in support of this document:

[https://github.com/peerplays-network/peerplays](https://github.com/peerplays-network/peerplays)

## Building on Ubuntu 18.04 LTS and Installation Instructions

The following dependencies are necessary for a clean install of Ubuntu 18.04 LTS:

```text
sudo apt-get install gcc-5 g++-5 cmake make libbz2-dev\
    libdb++-dev libdb-dev libssl-dev openssl libreadline-dev\
     autoconf libtool git
```

### Build Boost 1.67.0

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

## Building Peerplays

```text
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git submodule update --init --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)

make install # this can install the executable files under /usr/local
```

## Install Docker Image

```text
# Install docker
sudo apt install docker.io


# Add current user to docker group
sudo usermod -a -G docker $USER
# You need to restart your shell session, to apply group membership
# Type 'groups' to verify that you are a member of a docker group


# Build docker image (from the project root, must be a docker group member)
docker build -t peerplays .


# Start docker image
docker start peerplays

# Exposed ports
# # rpc service:
# EXPOSE 8090
# # p2p service:
# EXPOSE 1776
```

## Build Graphene

```text
cd ..
git clone https://github.com/cryptonomex/graphene.git
cd graphene
git submodule update --init --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Debug .
make 
```

## Starting A Peerplays Node

For Ubuntu 14.04 LTS and up users, see [this](https://github.com/cryptonomex/graphene/wiki/build-ubuntu) and then proceed with:

```text
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git submodule update --init --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release .
make
./programs/witness_node/witness_node
```

Launching the Witness creates required directories. 

Next, **stop the Witness** **node** and continue.

```text
$ vi witness_node_data_dir/config.ini
p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 127.0.0.1:8090
seed-node = 213.184.225.234:59500
```

Start the Witness Node back up

```text
./programs/witness_node/witness_node
```

### Upgrading A Peerplays Node

To minimize downtime of your peerplays node when upgrading, one upgrade idea was written in [this steemit article](https://steemit.com/peerplays/@joseph/peerplays-update-setting-a-backup-witness-server-switching-servers).

### BOS and MINT Setup

The Bookie Oracle System \(BOS\) and Manual Intervention Tool \(MINT\) need to be installed as part of a witness setup. They are both prerequisites for the operation of the BookiePro app.

BOS / MINT repositories: [https://github.com/search?q=topic%3Abookie-oracle-software+org%3Apeerplays-network&type=Repositories](https://github.com/search?q=topic%3Abookie-oracle-software+org%3Apeerplays-network&type=Repositories)

BOS set-up and installation guide: [https://bos-auto.readthedocs.io/en/develop/installation.html](https://bos-auto.readthedocs.io/en/develop/installation.html)

### Wallet Setup

Then, in a separate terminal window, start the command-line wallet `cli_wallet`:

```text
./programs/cli_wallet/cli_wallet
```

To set your initial password to 'password' use:

```text
>>> set_password password
>>> unlock password
```

A list of CLI wallet commands is available [here](https://github.com/PBSA/peerplays/blob/master/libraries/wallet/include/graphene/wallet/wallet.hpp).

### Testnet - "Beatrice"

* chain-id - T.B.D.

### Use the `get_private_key_from_password` command

You will to generate owner and active keys

```text
get_private_key_from_password your_witness_username active the_key_you_received_from_the_faucet
```

This will reveal an array for your active key `["PPYxxx", "xxxx"]`

### import\_keys into your cli\_wallet

* use the second value in the array returned from the previous step for the private key
* be sure to wrap your username in quotes
* import the key with this command

```text
import_key "your_witness_username" xxxx
```

### Upgrade your account to lifetime membership

```text
upgrade_account your_witness_username true
```

### Create your witness \(substitute the url for your witness information\)

* place quotes around url

```text
create_witness your_witness_username "url" true
```

**Be sure to take note of the block\_signing\_key**

IMPORTANT \(issue below command using block\_signing\_key just obtained\)

```text
get_private_key block_signing_key
```

Compare this result to

```text
dump_private_keys
```

You should see 3 pairs of keys. One of the pairs should match your block\_signing\_key and this is the one you will use in the next step!

### Get your witness id

```text
get_witness username (note the "id" for your config)
```

### Modify your witness\_node config.ini to include **your** witness id and private key pair.

Comment out the existing private-key before adding yours

```text
vim witness_node_data_dir/config.ini

witness-id = "1.6.x"
private-key = ["block_signing_key","private_key_for_your_block_signing_key"]
```

### start your witness back up

```text
./programs/witness_node/witness_node
```

If it fails to start, try with these flags \(not for permanent use\)

```text
./programs/witness_node/witness_node --resync --replay
```

### Vote for yourself

```text
vote_for_witness your_witness_account your_witness_account true true
```

### Ask to be voted in!

Join @Peerplays Telegram group to find information about the witness group. [http://t.me/@peerplayswitness](http://t.me/@peerplayswitness)

You will get logs that look like this:

```text
2070264ms th_a       application.cpp:506           handle_block         ] Got block: #87913 time: 2017-05-27T16:34:30 latency: 264 ms from: bhuz-witness  irreversible: 87903 (-10)
```

Assuming you've received votes, you will start producing as a witness at the next maintenance interval \(once per hour\). You can check your votes with.

```text
get_witness your_witness_account
```

### systemd

It's important for your witness to start when your system boots up. The filepaths here assume that you installed your witness into `/home/ubuntu/peerplays`

Create a logfile to hold your stdout/err logging

```text
sudo touch /var/log/peerplays.log
```

Save this file in your peerplays directory. `vi /home/ubuntu/peerplays/start.sh`

```text
#!/bin/bash

cd /home/ubuntu/peerplays
./programs/witness_node/witness_node &> /var/log/peerplays.log
```

Make it executable

```text
chmod 744 /home/ubuntu/peerplays/start.sh
```

Create this file: `sudo vi /etc/systemd/system/peerplays.service` Note the path for start.sh. Change it to match where your start.sh file is if necessary.

```text
[Unit]
Description=Peerplays Witness
After=network.target

[Service]
ExecStart=/home/ubuntu/peerplays/start.sh

[Install]
WantedBy = multi-user.target
```

Enable the service

```text
sudo systemctl enable peerplays.service
```

Make sure you don't get any errors

```text
sudo systemctl status peerplays.service
```

Stop your witness if it is currently running from previous steps, then start it with the service.

```text
sudo systemctl start peerplays.service
```

Check your logfile for entries

```text
tail -f /var/log/peerplays.log
```

### BOS and MINT Setup

### 

### Running specific tests

* `tests/chain_tests -t block_tests/name_of_test`

