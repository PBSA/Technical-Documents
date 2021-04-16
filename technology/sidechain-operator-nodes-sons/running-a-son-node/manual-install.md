---
description: Setup SONs by building the source code
---
# Manual Install

## Overview

The process of manually installing a SON Node[^son-node] is similar to installing a Witness Node[^witness-node].

This is an introduction for new SONs to get up to speed on the Peerplays blockchain. It is intended for SONs planning to join a live, already deployed, blockchain.

The following repository should be used in support of this document:

<https://github.com/peerplays-network/peerplays>

Please review the Requirements[^requirements] for setting up a SON Node before continuing to run a manual install following this guide.

The following steps outline the manual installation of a SON Node.

1. Build from Source Code
1. Configure the SON Node
1. Start the SON Node
1. (Optional) Automatically Start the Node as a Service

## Build from Source Code

### Install dependencies on Ubuntu 18.04 LTS

The following dependencies are necessary for a clean install on Ubuntu 18.04 LTS:

```text
sudo apt-get update
sudo apt-get -y  install autoconf bash build-essential ca-certificates cmake \
      dnsutils doxygen git graphviz libbz2-dev libcurl4-openssl-dev \
      libncurses-dev libreadline-dev libssl-dev libtool libzmq3-dev \
      locales ntp pkg-config wget autotools-dev libicu-dev python-dev
```

#### Build Boost 1.67.0

Boost is a C++ library that handles common program functions like generating config files and basic file system i/o. Peerplays uses Boost to handle such functions. Since Boost is a dependency, we must build it here.

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

### Build peerplays

Now we build Peerplays with the official source code from GitHub.

```text
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git checkout master # --> replace with most recent tag
git submodule update --init --recursive
git submodule sync --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)

make install # this can install the executable files under /usr/local
```

> **Note**: \#master\# can be replaced with the most recent release tag.

#### Start the node to generate the config.ini file

If we have installed the blockchain following the above steps, the node can be started as follows.

> **Note**: We start the SON Node with the `witness_node` command although we are only intending to set up this node as a SON. This is because the same program is used to operate different types of nodes depending on how we configure the program.[^node-types]

```text
./programs/witness_node/witness_node 

# If you need the logs, the following can be helpful
# ./programs/witness_node/witness_node 2>&1 peerplays.log
```

Running the witness_node program will create a config.ini file with some default settings. We'll need to edit the config file so we'll stop the program for now. Stop the program with `ctrl + c`.

## Configuration

The newly generated config.ini file will be located at the following path.

```text
<home>/src/peerplays/witness_node_data_dir/config.ini
```
### Creating a SON account in Peerplays


## Start the SON Node

After setting up the config.ini file for SON operation, we'll start the node back up.

```text
./programs/witness_node/witness_node
```

Your SON configured, up and running. But there's still more to do.

### Next steps

* Automate the SON node to run when the server starts.
* Create a backup SON node to help prevent downtime.
* Get voted in as an operating SON node.

## Setting Up the SON Node as a Service



> **Note**: This part is optional, but highly recommended!

[^son-node]: ***SON Node***:
Sidechain Operator Node - An independent server operator which facilitates the transfer of off-chain assets (like Bitcoin or Ethereum tokens) between the Peerplays chain and the asset's native chain.

[^witness-node]: ***Witness Node***:
An independent server operator which produces blocks for the blockchain. These blocks contain the operations and transactions that happen on the chain.
See
[Becoming a Witness](../../../witnesses/becoming-a-witness.md) for more information on installing Witness Nodes.
[What is a Peerplays Witness](../../../witnesses/wat-is-a-witness.md) for more information on Peerplays Witnessing.

[^requirements]: ***SON Node Requirements***:
See
[Requirements](requirements.md)

[^node-types]: ***Node Types***:
There are many types of nodes in the Peerplays ecosystem. (Witness, SON, Seed, and API)
