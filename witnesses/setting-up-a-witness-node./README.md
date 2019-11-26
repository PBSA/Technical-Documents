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

The next step is to set up the CLI Wallet.

{% page-ref page="../../peerplays-wallet/cli-wallet-setup.md" %}

### 

## BOS and MINT Setup

{% page-ref page="bos-and-mint-setup.md" %}



### 

