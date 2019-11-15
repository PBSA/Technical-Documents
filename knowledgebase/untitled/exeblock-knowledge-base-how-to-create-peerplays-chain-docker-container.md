# EXEBlock Knowledge Base : How to create peerplays-chain docker container

1. Download the attachments
2. Check if the downloaded files are up to date. Compare with those in the testnet-docker repository: [https://bitbucket.org/exeblock/testnet-docker/src/master/peerplays-chain-docker/](https://bitbucket.org/exeblock/testnet-docker/src/master/peerplays-chain-docker/)
3. _chmod +x build.sh_   and _chmod +x build\_run.sh_
4. Remove old container and image: _sudo docker ps -a \| awk '{ print $1,$2 }' \| grep peerplays-chain \| awk '{print $1 }' \| xargs -I {} sudo docker rm -f {}_
5. Either just build _./build.sh_ or build and run the docker container:   _./build\_run.sh_
6. Run the curl command to test a node and to get  a chain id \(assuming the node's rpc port is 8090\): _curl --data '{"jsonrpc": "2.0", "method": "get\_chain\_properties", "params": \[\], "id": 1}'_ [_http://127.0.0.1:8090/rpc_](http://127.0.0.1:11011/rpc)
7. Done! 

       \*\*\* The script is using distcc farm in the local network. The default configuration is:  
            _"localhost/3,lzo,cpp\n10.20.10.49/2,lzo,cpp\n10.20.10.41/2,lzo,cpp\n10.20.10.47/3,lzo,cpp_  
             Which means 3 jobs will be running on the localhost, 2 jobs on 10.20.10.49 server and so on.  
             If the distcc server settings have been changed, update this line with the new configuration.  
             Also, if you get the 'Cannot Allocate Memory' error, try to reduce a total number of running jobs by replacing   
             RUN distcc-pump make -j8 with RUN distcc-pump make -j6 or RUN distcc-pump make -j4.

**Dockerfile**

FROM ubuntu:16.04

\# default is 8090 unless a --build-args SIDE\_PORT=1337 is added on docker build.  
ENV SIDE\_PORT 8090  
EXPOSE $SIDE\_PORT

\#distcc port  
EXPOSE 3692

ENV P2P\_PORT 9777  
EXPOSE $P2P\_PORT

\# Home folder  
ARG HOME\_DIR=/home/peerplays

\# Create 'peerplays' folder  
RUN mkdir $HOME\_DIR  
WORKDIR $HOME\_DIR

FROM ubuntu:16.04

RUN apt-get update && \  
apt-get install -y libncurses5-dev && \  
apt-get install -y build-essential && \  
apt-get install -y cmake && \  
apt-get install -y curl && \  
apt-get install -y dh-autoreconf && \  
apt-get install -y expect && \  
apt-get install -y git && \  
apt-get install -y libssl-dev && \  
apt-get install -y doxygen && \  
apt-get install -y libreadline6 && \  
apt-get install -y libreadline6-dev && \  
apt-get install -y libboost-all-dev && \  
apt-get install -y libzmq3-dev && \  
apt-get install -y distcc&& \  
apt-get install -y distcc-pump

RUN git clone [https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/peerplays-chain.git](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/peerplays-chain.git) $HOME\_DIR/peerplays-chain  
  
WORKDIR $HOME\_DIR/peerplays-chain

RUN git submodule update --init --recursive

\#RUN cmake -DCMAKE\_BUILD\_TYPE=Release -DECC\_IMPL=secp256k1 .  
RUN cmake -DBOOST\_ROOT="$BOOST\_ROOT" -DCMAKE\_BUILD\_TYPE=Release .

\#distcc settings  
RUN export PATH="/usr/lib/distcc:$PATH" && \  
mkdir ~/.distcc && \  
echo -e "localhost/3,lzo,cpp\n10.20.10.49/2,lzo,cpp\n10.20.10.41/2,lzo,cpp\n10.20.10.47/3,lzo,cpp" &gt;&gt; ~/.distcc/hosts

\#compile using distcc  
RUN distcc-pump make -j8  
\#RUN make CC="distcc gcc -std=gnu99"  
\#RUN make -j$\(distcc -j\)

\#compile without distcc  
\#RUN make

ADD config\_wallet.sh $WORKDIR  
RUN chmod +x config\_wallet.sh

ADD genesis.sh $WORKDIR  
RUN chmod +x genesis.sh

ADD wallet\_node.sh $WORKDIR  
RUN chmod +x wallet\_node.sh

RUN mkdir -p genesis && \  
programs/witness\_node/witness\_node --create-genesis-json genesis/my-genesis.json && \  
sh genesis.sh

\#change the lines in a file  
RUN \  
sed -i "s\|\# p2p-endpoint =\|p2p-endpoint = 0.0.0.0:9777\|" ./data/my-blockprod/config.ini && \  
sed -i "s\|\# rpc-endpoint =\|rpc-endpoint = 0.0.0.0:8090\|" ./data/my-blockprod/config.ini && \  
sed -i "s\|\# genesis-json =\|genesis-json = genesis\/my-genesis.json\|" ./data/my-blockprod/config.ini && \  
sed -i "s\|enable-stale-production = false\|enable-stale-production = true\|" ./data/my-blockprod/config.ini && \  
sed -i "s\|\# private-key = \|private-key = \[\"PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV\",\"5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3\"\]\|" ./data/my-blockprod/config.ini && \  
sed -i "s\|\# witness-ids =\|witness-ids = \[\"1.6.1\", \"1.6.2\", \"1.6.3\", \"1.6.4\", \"1.6.5\", \"1.6.6\", \"1.6.7\", \"1.6.8\", \"1.6.9\", \"1.6.10\", \"1.6.11\"\]\|" ./data/my-blockprod/config.ini

\# run the witness as service and curl chain\_id and run cli\_wallet  
ENTRYPOINT \["/bin/bash", "wallet\_node.sh"\]

CMD \["/bin/bash"\]

