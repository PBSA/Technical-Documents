# Beatrice

Currently the Beatrice testnet has functionality for SON while the mainnet does not. This requires a few more packages to be installed and some extra configuration to enable Bitcoin connectivity.

## Building on Ubuntu 18.04 LTS and Installation Instructions <a id="building-on-ubuntu-18-04-lts-and-installation-instructions"></a>

The following dependencies are necessary for a clean install of Ubuntu 18.04 LTS:

```text
sudo apt-get update
sudo apt-get -y  install autoconf bash build-essential ca-certificates cmake \
      dnsutils doxygen git graphviz libbz2-dev libcurl4-openssl-dev \
      libncurses-dev libreadline-dev libssl-dev libtool libzmq3-dev \
      locales ntp pkg-config wget autotools-dev libicu-dev python-dev
```

### Build Boost 1.67.0 <a id="build-boost-1-67-0"></a>

```text
mkdir $HOME/src
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
sudo apt-get update
sudo apt-get install -y autotools-dev libicu-dev python-dev
wget -c 'http://sourceforge.net/projects/boost/files/boost/1.67.0/boost_1_67_0.tar.bz2/download' \
     -O boost_1_67_0.tar.bz2
tar xjf boost_1_67_0.tar.bz2
cd boost_1_67_0/
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install
```

## Building Peerplays <a id="building-peerplays"></a>

```text
cd $HOME/src
export BOOST_ROOT=$HOME/src/boost_1_67_0
git clone https://github.com/peerplays-network/peerplays.git
cd peerplays
git checkout beatrice
git submodule update --init --recursive
git submodule sync --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Release
make -j$(nproc)
```

## Starting A Peerplays Witness Node <a id="starting-a-peerplays-witness-node"></a>

```text
./programs/witness_node/witness_node
```

{% hint style="danger" %}
**Important**: Next stop the Witness node before continuing.
{% endhint %}

```text
vi witness_node_data_dir/config.ini
```

Configure the p2p-endpoint and rpc-endpoint:

```text
p2p-endpoint = 0.0.0.0:9777
rpc-endpoint = 127.0.0.1:8090
```

{% hint style="success" %}
Start the Witness node back up.
{% endhint %}

```text
./programs/witness_node/witness_node
```

### Starting the witness as a service <a id="starting-the-witness-as-a-service"></a>

We can add the Peerplays blockchain node as a service by placing a service file under`/etc/systemd/system` similar to:

```text
[Unit]
Description=Witness
[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/beatrice
ExecStart=/home/ubuntu/beatrice/witness_node
Restart=always
[Install]
WantedBy=mult-user.target
```

