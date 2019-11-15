# EXEBlock Knowledge Base : How to get started with EOS testnet

This is a basic guide on how to create a wallet, account, contract, and send funds between accounts on a local EOS testnet.

## Step-by-step guide <a id="HowtogetstartedwithEOStestnet-Step-by-stepguide"></a>

**Anatomy of basic programs EOSIO programs**:

* _**nodeos**_ – server-side blockchain node component. The core EOSIO daemon that can be configured with plugins to run a node. Example uses are block production, dedicated API endpoints, and local development.
* _**cleos**_ – command line interface to interact with the blockchain
* _**keosd**_ – an EOSIO wallet daemon that loads wallet related plugins, such as the HTTP interface and RPC API  

**\(1\) Basic Set Up**

Note: This step can be skipped if using the exeblock testnet. This is simply a quick way to get your own local testnet to test on.

 Here are some basic Docker commands that may be useful in this guide:

        docker build -t “name\_of\_image”. : Build the image  
        docker run -i -t “name\_of\_image” : run docker image and enter shell  
        docker ps: list containers

We will be using the docker quick start available at: [https://eosio-nodeos.readme.io/docs/docker-quickstart](https://eosio-nodeos.readme.io/docs/docker-quickstart)

This image is a compiled version of the EOSIO software designed to allow developers to get quickly setup and developing on their local testnet.

**Step 1:** Pull the image from the repository  
docker pull eosio/eos-dev

  
**Step 2:** Start the EOSIO node  
docker run --name nodeos -p 8888:8888 -p 9876:9876 -t eosio/eos-dev nodeosd.sh arg1 arg2  
  
Now we have a simple single node EOS blockchain running inside of the eosio/eos-dev docker container!

       **Step 3:** Assuming the EOSIO node was started without errors, we can check our blockchains information, and check that the RPC interface is working.

       curl http://127.0.0.1:8888/v1/chain/get\_info  
       This should return a JSON message with the chain\_id

       **Step 4:** Go into your docker container

       _docker run -i -t "Name/id of image"_

**\(2\) Creating and Setting Up the EOS Wallet**

**Step 1:** Create a wallet

_cleos wallet create -n exeUser →_ feel free to use a different user name

\(a\) List the user:

     _cleos wallet_ _list_

**Step 2:** Generate EOSIO Keys Pair

_cleos create key_

_cleos wallet import_ _-n exeuser ${private\_key\_1}_ → Here private key should be the private key generated from the "cleos create key" command

_cleos wallet keys_ → If succesful, this should respond with your public key

**Creating an Account**

We want to be able to create an account so that we can perform actions on the blockchain. This requires both keosd and nodeos to run simultaneously

**Step 1:** Start keosd and nodeos simultaneously

Currently, the default port for keosd and nodeos are the same \(port 8888\). To simplify running nodeos for this part of the tutorial, we will change the port for keosd to 8899. There are two ways that we can do this:

1. Edit the keosd config file \(~/config.ini\) and change the http-server-address property to: http-server-address = 127.0.0.1:8899

OR

      2. Start keosd using the command line argument --http-server-address=localhost:8899

\(a\) Using \(2\) we kill the current keosd, and restart it on a different port

     _pkill keosd_

     _nohup keosd --http-server-address=localhost:8899 &_

\(b\) Unlock your default wallet \(it was locked when keosd was restarted\). Since keosd was started and is now listening on port 8899, you will need to use the --wallet-url command line argument to cleos.

     _cleos --wallet-url=_[_http://localhost:8899_](http://localhost:8899) _wallet unlock -n exeuser_

_\(c\) Start nodeos in the background_

     _nohup nodeos -e -p eosio --plugin eosio::chain\_api\_plugin --plugin eosio::history\_api\_plugin &_

_**Step 2: Import eosio private key**_

the key can be found in the config.ini file under the "signature-provider"

_cleos --wallet-url_ [_http://localhost:8899_](http://localhost:8899) _wallet import &lt;eosio\_account\_private\_key&gt;_

**Step 2: Create your account**

cleos --wallet-url=[http://localhost:8899](http://localhost:8899) create account eosio test123 _&lt;eosio\_account\_private\_key&gt; &lt;eosio\_account\_private\_key&gt;_

cleos --wallet-url [http://localhost:8899](http://localhost:8899) set contract tester123 contracts/eosio.token

**Step 2:** Create a currency

cleos --wallet-url [http://localhost:8899](http://localhost:8899) push action tester123 create '\[ "eosio", "1000000000.0000 EOS"\]' -p tester123

## Related articles <a id="HowtogetstartedwithEOStestnet-Relatedarticles"></a>

*  Page: [How to set up Linux testnet environment](/wiki/spaces/EKB/pages/197460035/How+to+set+up+Linux+testnet+environment)
*  Page: [How to Setup EXEchain](/wiki/spaces/EKB/pages/197460257/How+to+Setup+EXEchain)
*  Page: [How to setup Sidechain environment](/wiki/spaces/EKB/pages/197460072/How+to+setup+Sidechain+environment)
*  Page: [EVM Smart Contract testing](/wiki/spaces/EKB/pages/197460007/EVM+Smart+Contract+testing)
*  Page: [How to Create Smart Contract](/wiki/spaces/EKB/pages/197460297/How+to+Create+Smart+Contract)

