# Setting up a Witness node.

This is an introduction to get new Witnesses up to speed on the Peerplays blockchain. It is intended for Witnesses planning to join a live, already deployed, blockchain.

The following repository should be used in support of this document:

{% embed url="https://github.com/peerplays-network/peerplays" %}

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

```text
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git submodule update --init --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release .
make
./programs/witness_node/witness_node
```

Launching the Witness creates the required directories. 

{% hint style="danger" %}
Next stop the Witness node before continuing.
{% endhint %}

```text
$ vi witness_node_data_dir/config.ini
p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 127.0.0.1:8090
seed-node = 213.184.225.234:59500
```

{% hint style="success" %}
Start the Witness node back up.
{% endhint %}

```text
./programs/witness_node/witness_node
```

### Upgrading A Peerplays Node

To minimize downtime of your Peerplays node when upgrading, it's recommended to create a backup Witness server.

{% page-ref page="creating-a-backup-server.md" %}

## CLI Wallet Setup

In a separate terminal window, start the command-line wallet `cli_wallet`:

```text
./programs/cli_wallet/cli_wallet
```

To set your initial password to `password` use:

```text
>>> set_password password
>>> unlock password
```

{% hint style="info" %}
A list of CLI wallet commands is available here:[ ](https://github.com/PBSA/peerplays/blob/master/libraries/wallet/include/graphene/wallet/api_documentation.hpp)[CLI Wallet commands](https://github.com/PBSA/peerplays/blob/master/libraries/wallet/include/graphene/wallet/api_documentation.hpp)
{% endhint %}

### Get Owner and Active Keys

To generate your owner and active keys use the `get_private_key_from_password` command:

```text
get_private_key_from_password your_witness_username active the_key_you_received_from_the_faucet
```

This will return an array, with your active key, in the form `["PPYxxx", "xxxx"].`

### Import Keys into your CLI Wallet

To import your keys, generated in the previous step, into your CLI wallet do the following:

1. Use the second value in the array for the private key.
2. Make sure your username is in quotes. 
3. Import the key using the following command 

{% hint style="warning" %}
Substitute `your-witness-username` with your own username.
{% endhint %}

```text
import_key "your_witness_username" xxxx
```

### Upgrade Your Account to Lifetime Membership

```text
upgrade_account your_witness_username true
```

### Create Yourself as a Witness

Make sure your URL is in quotes. 

{% hint style="warning" %}
Substitute `"url"` for your witness URL.
{% endhint %}

```text
create_witness your_witness_username "url" true
```

This will return your `block_signing_key`

{% hint style="danger" %}
Be sure to take note of  your `block_signing_key`
{% endhint %}

Run the following command using your `block_signing_key` from the previous step:

```text
get_private_key block_signing_key
```

Compare this result to the three pairs of keys generated when you run the following command:

```text
dump_private_keys
```

One of the pairs should match your `block_signing_key` , this is the one you'll use for other operations.

### Get Your Witness ID

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

### 

### BOS and MINT Setup

{% page-ref page="bos-and-mint-setup.md" %}



### 

