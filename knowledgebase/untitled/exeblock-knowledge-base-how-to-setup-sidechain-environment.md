# EXEBlock Knowledge Base : How to setup Sidechain environment

## /\*&lt;!\[CDATA\[\*/ div.rbtoc1573758218677 {padding: 0px;} div.rbtoc1573758218677 ul {list-style: disc;margin-left: 0px;} div.rbtoc1573758218677 li {margin-left: 0px;padding-left: 0px;}  /\*\]\]&gt;\*/[Setting up SideChain:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-SettingupSideChain:)[Create Genesis for SideChain:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-CreateGenesisforSideChain:)[To Run the SideChain:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-ToRuntheSideChain:)[To Run the wallet:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-ToRunthewallet:)[Using the bitcoin-cli](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-Usingthebitcoin-cli)[Setting up multiple nodes](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-Settingupmultiplenodes)[Embedding the Genesis block \(optional\)](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-EmbeddingtheGenesisblock%28optional%29)[Setting up the Faucet:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-SettinguptheFaucet:)[Build issues:](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-Buildissues:)[Related articles](exeblock-knowledge-base-how-to-setup-sidechain-environment.md#HowtosetupSidechainenvironment-Relatedarticles)Setting up SideChain: <a id="HowtosetupSidechainenvironment-SettingupSideChain:"></a>

1. **Git clone the sidechain:**        git clone [git@bitbucket.org](mailto:git@bitbucket.org):exeblock/sidechain.git    cd sidechain  
2. **Install the dependencies for the sidechain:**  **MacOS:**     brew doctor    brew update     brew install/    autoconf \    automake \    cmake \    git \    boost160 \    libtool \    openssl \    snappy \    zlib \    python3    brew unlink readline    brew link --force readline  **Linux:**    _Ubuntu 16.04 LTS ships with Boost 1.58 libraries, so no need to build from source._    sudo apt-get install libboost-all-dev      
3. **Install CMake and other missing functions:**  **MacOS:**    ****brew install cmake    brew install libyaml    brew install zmq    git clone [https://github.com/zeromq/libzmq](https://github.com/zeromq/libzmq)    cd libzmq    mkdir cmake-build && cd cmake-build && cmake .. && make -j 4 && make test && sudo make install && sudo ldconfig    sudo update\_dyld\_shared\_cache    cd ../..  **Linux:**    git clone [https://github.com/zeromq/libzmq](https://github.com/zeromq/libzmq)    cd libzmq    mkdir cmake-build    cd cmake-build    cmake .. && make -j 4 && make test && sudo make install    sudo update\_dyld\_shared\_cache    cd ../..  
4. **MacOS:** _Adding "/usr/local/include/zmq.hpp" lets the SideChain build properly since brew only installed the zmq.h_    vi /usr/local/include/zmq.hpp       _paste_ [_this file_](https://github.com/zeromq/cppzmq/edit/master/zmq.hpp) _in there_     :exit  __
5. **Adding the location of the ENV is important in MacOS since all the dependencies are obtained with "brew" instead of "apt-get":**  **MacOS:**    export OPENSSL\_ROOT\_DIR=$\(brew --prefix\)/Cellar/openssl/1.0.2o\_2/    export OPENSSL\_CRYPTO\_LIBRARY=$\(brew --prefix\)/Cellar/openssl/1.0.2o\_2/lib    export OPENSSL\_INCLUDE\_DIR=$\(brew --prefix\)/Cellar/openssl/1.0.2o\_2/include    export BOOST\_ROOT=$\(brew --prefix\)/Cellar/boost@1.60/1.60.0/    export SNAPPY\_LIBRARIES=$\(brew --prefix\)/Cellar/snappy/1.1.7\_1/lib/    export SNAPPY\_INCLUDE\_DIR=$\(brew --prefix\)/Cellar/snappy/1.1.7\_1/include/    export ZLIB\_LIBRARIES=$\(brew --prefix\)/Cellar/zlib/1.2.11/lib/    export ZMQ\_ROOT\_DIR=$\(brew --prefix\)/Cellar/zeromq/4.2.5/  
6. **Update the submodules:**        git checkout stable    git submodule update --init --recursive     mkdir build && cd build    cmake -DBOOST\_ROOT="$BOOST\_ROOT" -DCMAKE\_BUILD\_TYPE=Release -DECC\_IMPL=secp256k1 ..    cd ..    make    ./programs/witness\_node/witness\_node --enable-stale-production  
7. **Stop the witness node to continue:**        vi witness\_node\_data\_dir/config.ini _Now add the following below to the config.ini file_    p2p-endpoint = 0.0.0.0:9777    rpc-endpoint = 127.0.0.1:8090 _Exit and run the witness node again_

## Create Genesis for SideChain: <a id="HowtosetupSidechainenvironment-CreateGenesisforSideChain:"></a>

1. **Genesis creation:**  mkdir -p genesis programs/witness\_node/witness\_node --create-genesis-json genesis/my-genesis.json
2. **Creating data directory:**  programs/witness\_node/witness\_node --data-dir data/my-blockprod --genesis-json genesis/my-genesis.json _\*\*Quit the node by Ctrl+C_
3. _**Starting block production:**  
  
   \*\*Open data/my-blockprod/config.ini for editing:_   
   vi data/my-blockprod/config.ini  


   p2p-endpoint = 127.0.0.1:9777  
   rpc-endpoint = 127.0.0.1:8090   
   \*\*for outside connections - 0.0.0.0:8090

   genesis-json = genesis/my-genesis.json

   private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]

   witness-id = "1.6.1"  
   witness-id = "1.6.2"  
   witness-id = "1.6.3"  
   witness-id = "1.6.4"  
   witness-id = "1.6.5"  
   witness-id = "1.6.6"  
   witness-id = "1.6.7"  
   witness-id = "1.6.8"  
   witness-id = "1.6.9"  
   witness-id = "1.6.10"  
   witness-id = "1.6.11"  
  
   _\*\*Save and exit the file:_  
   :exit  
  
   _\*\*Run witness again:_  
   programs/witness\_node/witness\_node --data-dir data/my-blockprod --enable-stale-production  
  
   _\*\*To start it as a service:_  
   nohup programs/witness\_node/witness\_node --data-dir data/my-blockprod --enable-stale-production &

## To Run the SideChain: <a id="HowtosetupSidechainenvironment-ToRuntheSideChain:"></a>

1. **Running the witness back up:**        ./programs/witness\_node/witness\_node --data-dir data/my-blockprod --genesis/my-genesis.json

## To Run the wallet: <a id="HowtosetupSidechainenvironment-ToRunthewallet:"></a>

**Then, in a separate terminal window, start the command-line wallet:**

1. **First you need to find the chain ID:**    sudo curl --data '{"jsonrpc": "2.0", "method": "get\_chain\_properties", "params": \[\], "id": 1}' [http://127.0.0.1:8090/rpc](http://127.0.0.1:11011/rpc) && echo    or view the SideChain terminal window to see the initialized chain ID  
2. **Create the wallet:**    make cli\_wallet    ./programs/cli\_wallet/cli\_wallet --wallet-file my-wallet.json --chain-id &lt;enter your chain-ID here&gt; --server-rpc-endpoint [ws://127.0.0.1:8090](exeblock-knowledge-base-how-to-setup-sidechain-environment.md) -u '' -p ''    set\_password supersecret  
3. **Unlock the wallet:**    unlock supersecret    import\_key nathan "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"    \*\*_Funds are stored in genesis balance objects. These funds can be claimed, with no fee, using the import\_balance command._    import\_balance nathan \["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\] true  
4. **Make 'nathan' a lifetime member to create an account:**    upgrade\_account nathan true  
5. **Create an account:**    register\_account alpha PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF nathan nathan 0 true    transfer nathan alpha 100000 PPY "here is the cash" true    \*\*_We can now open a new wallet for alpha user:_    import\_key alpha 5HuCDiMeESd86xrRvTbexLjkVg2BEoKrb7BAA5RLgXizkgV3shs    upgrade\_account alpha true    create\_witness alpha "[http://www.alpha](http://www.alpha)" true    \*\*_The get\_private\_key command allows us to obtain the public key corresponding to the block signing key:    get\_private\_key PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF_ 
6. **Fund the witnesses**  


   transfer nathan init0 100000 PPY "here is the cash" true  
   transfer nathan init1 100000 PPY "here is the cash" true  
   transfer nathan init2 100000 PPY "here is the cash" true  
   transfer nathan init3 100000 PPY "here is the cash" true  
   transfer nathan init4 100000 PPY "here is the cash" true  
   transfer nathan init5 100000 PPY "here is the cash" true  
   transfer nathan init6 100000 PPY "here is the cash" true  
   transfer nathan init7 100000 PPY "here is the cash" true  
   transfer nathan init8 100000 PPY "here is the cash" true  
   transfer nathan init9 100000 PPY "here is the cash" true  
   transfer nathan init10 100000 PPY "here is the cash" true

## Using the bitcoin-cli <a id="HowtosetupSidechainenvironment-Usingthebitcoin-cli"></a>

Before you begin:  
  
Make sure the btcnode ports and addresses match with the sidechain config.ini parameters. In the examples case we are using:

sidechain-bitcoin-ip = 127.0.0.1 &lt;- use ip of bitcoid \(bitcoin node\)

sidechain-bitcoin-zmq-port = 28332 &lt;- use port set in -zmqpubhashblock

sidechain-bitcoin-rpc-port = 8332 &lt;- use port set in -rpcport

sidechain-bitcoin-rpc-user = 123

sidechain-bitcoin-rpc-password = 123 &lt;- your username and password set in rpc bitcoind  
  
Make sure to run the bitcoin daemon process on your machine with these parameters:

```text
bitcoind -regtest -server=1 -listen=1 -rpcallowip=0.0.0.0/0 -rest -rpcuser=123 -rpcpassword=123 -zmqpubhashblock=tcp://127.0.0.1:28332 -rpcport=8332 -txindex=1 -daemon
```

You can also enter these configurations in your bitcoin.conf file in your bitcoin data directory.

Note: We are running in the regtest mode meaning we are not connected to peers that can mine. Instead, we can use a command which acts as mining called generate. More information can be found at: [https://bitcoin.org/en/developer-examples\#regtest-mode](https://bitcoin.org/en/developer-examples#regtest-mode)

Once this is running, you can then start using bitcoin-cli to interact with the client.

Note: if using a new bitcoin client, you must generate 101 blocks.

```text
bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 generate 101
```

Using cURL

```text
curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"generate","params":[101]}' -H 'content-type:text/plain;' http://123:123@localhost:8332/
```

1. -Now, you should have bitcoin generated in the wallet. To check the balance:

   ```text
   bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 getbalance
   ```

   Using cURL

   ```text
   curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getbalance","params":[]}' -H 'content-type:text/plain;' http://123:123@localhost:8332/
   ```

2. You should now be able to send btc to an address that is connected to the btc node. Create an account using the sidechain wallet and generate a bitcoin address for it by clicking the deposit BTC button at the top and showing the address \(currentyl, can be accessed at http://10.20.10.47:8082\). Now, use bitcoin-cli to send bitcoin to this address.

   ```text
   bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 sendtoaddress 2q0HFbWCgCWxNGXtCKwlIeOfGbrOn8xEO150hVnfmQuBPqI4QTrq 11
   ```

   Using cURL

   ```text
   curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"sendtoaddress","params":["2q0HFbWCgCWxNGXtCKwlIeOfGbrOn8xEO150hVnfmQuBPqI4QTrq", 11]}' -H 'content-type:text/plain;' http://123:123@localhost:8332/
   ```

  
   This will send 11 btc to the address 2q0HFbWCgCWxNGXtCKwlIeOfGbrOn8xEO150hVnfmQuBPqI4QTrq  
  

3. Now we must generate blocks for this transaction to proceed.
   1. Generate 1 block and then wait for the sidechain blockchain to give 6 confirmations \(This can be viewed in the network tab in the wallet gui\)

      ```text
      bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 generate 6
      ```

   2. After this, generate 6 blocks for the btc node to give 6 confirmations.

      ```text
      bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 generate 6
      ```

   3. The amount deposited \(minus the fees\) will be shown in the sidechain wallet after 6 more blocks of the sidechain blockchain are produced.
4. To withdraw the bitcoin from sidechain, we first need to determine an address to send to. With bitcoin-cli we can create an address for the wallet which we can use.

   ```text
   bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 getnewaddress "bob"
   ```

   Using cURL

   ```text
   curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getnewaddress","params":["bob"]}' -H 'content-type:text/plain;' http://123:123@localhost:8332/
   ```

   This will generate a public address for the label entered. In this case, the generated output for the address given is 2NEQwEcpjkX9mVQ2135QAaLJx1CxrKWouwM

5. Now on sidechain we can withdraw the btc we have to this address. After doing so, generate some blocks using the bitcoin-cli.

   ```text
   bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 generate 6
   ```

6. Now we can see a successful deposit by checking if the address we generated received the amount \(minus the fees\) sent.

   ```text
   bitcoin-cli -rpcuser=123 -rpcpassword=123 -rpcport=8332 getreceivedbyaddress 2NEQwEcpjkX9mVQ2135QAaLJx1CxrKWouwM
   ```

   Using cURL:

   ```text
   curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getreceivedbyaddress","params":["2NEQwEcpjkX9mVQ2135QAaLJx1CxrKWouwM"]}' -H 'content-type:text/plain;' http://123:123@localhost:8332/
   ```

7. Send BTC to another account. On the sidechain gui, click send at the top of the dashboard and send pBTC to another account. Check if the amount if pBTC is taken away from the sender, and added to the receiver.
8. RESET BITCOIN:

   ```text
   rm -rf ~/.bitcoin/regtest
   ```

## Setting up multiple nodes <a id="HowtosetupSidechainenvironment-Settingupmultiplenodes"></a>

Now that we have a blockchain established, we can sync other nodes to it.  
  


1. **Creating data directory:** programs/witness\_node/witness\_node --data-dir data/chain2 _\*\*Quit the node by Ctrl+C_  
2. **Edit the config.ini**  p2p-endpoint = &lt;some endpoint to listen on&gt; \(example 0.0.0.0:9781\) seed-node = &lt;p2p endpoint of the established blockchain&gt; \(example: 127.0.0.1:9780\) rpc-endpoint = &lt;some endpoint to listen on&gt; \(example: 0.0.0.0:8191\) genesis-json = &lt;location of genesis file&gt; sidechain-bitcoin-ip = 127.0.0.1 &lt;- use ip of bitcoid \(bitcoin node\) sidechain-bitcoin-zmq-port = 28332 &lt;- use port set in -zmqpubhashblock sidechain-bitcoin-rpc-port = 8332 &lt;- use port set in -rpcport sidechain-bitcoin-rpc-user = 123 sidechain-bitcoin-rpc-password = 123 &lt;- your username and password set in rpc bitcoind enable-stale-production = true  
3. **Start the chain** _programs/witness\_node/witness\_node --data-dir data/chain2  \*\*To start it as a service:_ nohup programs/witness\_node/witness\_node --data-dir data/chain2  & 

This can be repeated for multiple different nodes.

## Embedding the Genesis block \(optional\) <a id="HowtosetupSidechainenvironment-EmbeddingtheGenesisblock(optional)"></a>

Now that we have the blockchain established and the used correct genesis block, we can have it embedded into the binaries directly. For that reasons we have moved it into the root directory and called it genesis.json for the default compile toolchain to catch it automatically. We recompile to include the genesis block with:  
  


```text
make clean
find . -name "CMakeCache.txt" | xargs rm -f
find . -name "CMakeFiles" | xargs rm -Rf
cmake -DCMAKE_BUILD_TYPE=Release .
```

Deleting caches will reset all cmake variables, so if you have used instructions like ../installation/Build which tells you to set other cmake variables, you will have to add those variables to the cmake line above.

Embedding the genesis copies the entire content of genesis block into the witness\_node binary, and additionally copies the chain ID into the cli\_wallet binary. Embedded genesis allows the following simplifications to the subsequent instructions:

• You need not specify the genesis file on the witness node command line, or in the witness node configuration file.

• You need not specify the chain ID on the cli\_wallet command line when starting a new wallet.

## Setting up the Faucet: <a id="HowtosetupSidechainenvironment-SettinguptheFaucet:"></a>

**In order to allow people that do not have funds yet to enter the system, we need to setup a faucet. Here, we will also use mina as our deployment tool** for a productive installation:

1. **First git pull the taping for the faucet:**    [https://bitbucket.org/exeblock/tapin/](https://bitbucket.org/exeblock/tapin/src/development/)
2. **Start the faucet:**    _Ruby:_    rails server -p 3000 -b 0.0.0.0    _Python:_    python3 manage.py runserver -h 0.0.0.0 -p 5000  
3. **Here is a workaround:**    sudo addgroup gph    sudo adduser --system --no-create-home --ingroup gph --disabled-password --disabled-login gph

## Build issues: <a id="HowtosetupSidechainenvironment-Buildissues:"></a>

1. _CXX linkin - "cd .." out of the build directory, then "rm -rf build/" and  start again with step 6._
2. _For Ubuntu 14.04 LTS and up users, see this link first:_ [_https://github.com/cryptonomex/graphene/wiki/build-ubuntu_](https://github.com/cryptonomex/graphene/wiki/build-ubuntu) _and then proceed with:_

References: [https://steemit.com/steem/@chaimyu/steem-mac-steem](https://steemit.com/steem/@chaimyu/steem-mac-steem)

## Related articles <a id="HowtosetupSidechainenvironment-Relatedarticles"></a>

*  Page: [How to set up Linux testnet environment](/wiki/spaces/EKB/pages/197460035/How+to+set+up+Linux+testnet+environment)
*  Page: [How to Setup EXEchain](/wiki/spaces/EKB/pages/197460257/How+to+Setup+EXEchain)
*  Page: [How to setup Sidechain environment](/wiki/spaces/EKB/pages/197460072/How+to+setup+Sidechain+environment)
*  Page: [EVM Smart Contract testing](/wiki/spaces/EKB/pages/197460007/EVM+Smart+Contract+testing)
*  Page: [How to Create Smart Contract](/wiki/spaces/EKB/pages/197460297/How+to+Create+Smart+Contract)

