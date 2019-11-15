# EXEBlock Knowledge Base : How to create Bitcoin Core daemon \(bitcoind\) in Docker

\*\*NOTE: See attached files

INSTRUCTIONS:

1. **Navigate to the directory where the file is, and make sure to also have bitcoin.conf and start-node.sh \(shown below\)**
2. **Build the Dockerfile** _docker build -t bitcoind-testnet --no-cache ._
3.  **When completed, run the docker:** _docker run -dit --name bitcoind  -p 18332:18332 -p 28332:28332 -p bitcoind-testnet /bin/bash_
4. **\(To debug and test\) Connect with a separate console:** _docker exec -it bitcoind /bin/bash_
5. **Within this container from the docker exec you can run the script** \(sh start-node.sh\)

## Dockerfile <a id="HowtocreateBitcoinCoredaemon(bitcoind)inDocker-Dockerfile"></a>

```text
FROM ubuntu:16.04

#install bitcoin from PPA

RUN  apt-get update

RUN  apt-get install --yes software-properties-common

RUN  add-apt-repository --yes ppa:bitcoin/bitcoin

RUN  apt-get update

#install bitcoind (from PPA) and make

RUN  apt-get install --yes bitcoind

#install vim

RUN apt-get install --yes vim

ENV DOCKER_HOME /home/bitcoind-testnet

RUN mkdir -p /home/bitcoind-testnet

#copy start-node.sh script
ADD start-node.sh $DOCKER_HOME 
RUN chmod 755 $DOCKER_HOME/start-node.sh

#copy bitcoin.conf
RUN mkdir -p /root/.bitcoin
ADD bitcoin.conf /root/.bitcoin

#EXPOSE rpcallowip

EXPOSE 0

#EXPOSE Bitcoin JSON-RPC testnet server
EXPOSE 18332

#EXPOSE Bitcoin p2p testnet port
EXPOSE 18333

EXPOSE 28332

#WORKDIR sets the work directory

WORKDIR $DOCKER_HOME

CMD ["/home/bitcoind-testnet/start-node.sh"]
```

## start-node.sh <a id="HowtocreateBitcoinCoredaemon(bitcoind)inDocker-start-node.sh"></a>

```text
#!/bin/bash

bitcoind -testnet -server=1 -listen=1 -rpcallowip=0.0.0.0/0 -rest -rpcuser=123 -rpcpassword=123 -zmqpubhashblock=tcp://127.0.0.1:28332 -onlynet=ipv4 -rpcport=18332 -prune=550 -daemon

/bin/bash
```

## bitcoin.conf <a id="HowtocreateBitcoinCoredaemon(bitcoind)inDocker-bitcoin.conf"></a>

```text
testnet=3 
server=1 
listen=1 
rpcallowip=0.0.0.0/0 
rpcuser=123 
rpcpassword=123 
zmqpubhashblock=tcp://127.0.0.1:28332 
rpcport=18332
prune=550
```

