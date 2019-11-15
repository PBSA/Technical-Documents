# EXEBlock Knowledge Base : How to setup a Private Testnet of Peerplays/5050

### Setting up private testnet <a id="HowtosetupaPrivateTestnetofPeerplays/5050-Settingupprivatetestnet"></a>

**Important Note: The 5050 DAPP must be run on port 80. Failure to do so will cause it to throw constant websocket errors, making it almost impossible to debug.**

### Step-by-step guide <a id="HowtosetupaPrivateTestnetofPeerplays/5050-Step-by-stepguide"></a>

For Ubuntu 14.04 LTS and up users, see this link first: [https://github.com/cryptonomex/graphene/wiki/build-ubuntu](https://github.com/cryptonomex/graphene/wiki/build-ubuntu)

and then proceed with:

Ubuntu 16.04 LTS ships with Boost 1.58 libraries, so no need to build from source.

```text
$ sudo apt-get install libboost-all-dev
```

1. **Getting code and compiling:**  git clone [https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org](https://bitbucketdocker:FTHEbQaMUsgwpVgv32By@bitbucket.org/exeblock/sidechain.git)[/exeblock/peerplays-chain.git](https://bitbucket.org/exeblock/peerplays-chain.git) cd peerplays git submodule update --init --recursive  _\*\* \(Optional - see "embedding genesis"\) For embedding genesis run: cmake -DBOOST\_ROOT="$BOOST\_ROOT" -DGRAPHENE\_EGENESIS\_JSON="$\(pwd\)/genesis/my-genesis.json" -DCMAKE\_BUILD\_TYPE=Release .\)_  _\*\* Run cmake command to create make rules_ cmake -DBOOST\_ROOT="$BOOST\_ROOT" -DCMAKE\_BUILD\_TYPE=Release .  _\*\*Compile the witness node:_ make witness\_node   _or_ make witness\_node -j3 \(if the VM has 8GB of RAM or more\)  _\*\*\(Optional\) Launch the witness node \(click Ctrl-C to exit\):_ ./programs/witness\_node/witness\_node  
2. **Genesis creation:**  mkdir -p genesis programs/witness\_node/witness\_node --create-genesis-json genesis/my-genesis.json  
3. **Creating data directory:**  programs/witness\_node/witness\_node --data-dir data/my-blockprod --genesis-json genesis/my-genesis.json \*\*Quit the node by Ctrl+C  __
4. **Starting block production:**  
  
   _\*\*Open data/my-blockprod/config.ini for editing:_   
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
  

5. **Obtain chain ID:**  curl --data '{"jsonrpc": "2.0", "method": "get\_chain\_properties", "params": \[\], "id": 1}' [http://127.0.0.1:8090/rpc](http://127.0.0.1:11011/rpc) && echo  
6. **Creating a wallet:**  _\*\*Compile a wallet \(Optional see **embedding genesis**: For embedding genesis run: cmake -DBOOST\_ROOT="$BOOST\_ROOT" -DGRAPHENE\_EGENESIS\_JSON="$\(pwd\)/genesis/my-genesis.json" -DCMAKE\_BUILD\_TYPE=Release .\)_ make cli\_wallet  _\*\*Update the next command. You should use the chain ID reported by your witness\_node \(5.\)_  programs/cli\_wallet/cli\_wallet --wallet-file my-wallet.json --chain-id cf307110d029cb882d126bf0488dc4864772f68d9888d86b458d16e6c36aa74b --server-rpc-endpoint [ws://127.0.0.1:8090](exeblock-knowledge-base-how-to-setup-a-private-testnet-of-peerplays-5050.md) -u '' -p ''  \*\*_Set password "supersecret"_ set\_password supersecret  
7. **Gaining access to stake:**  unlock supersecret  __import\_key nathan "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3" __ \*\*_Funds are stored in genesis balance objects. These funds can be claimed, with no fee, using the import\_balance command._ import\_balance nathan \["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\] true  _\*\*Check the new balance:_ ****list\_account\_balances nathan  ****
8. **Make 'nathan' a lifetime member** \(\*\*_Creating an account requires lifetime member \(LTM\) status\):_  upgrade\_account nathan true  
9. **Creating accounts:**  register\_account alpha PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF nathan nathan 0 true  transfer nathan alpha 100000 PPY "here is the cash" true  \*\*_We can now open a new wallet for alpha user:_ import\_key alpha 5HuCDiMeESd86xrRvTbexLjkVg2BEoKrb7BAA5RLgXizkgV3shs  upgrade\_account alpha true  create\_witness alpha "[http://www.alpha](http://www.alpha)" true __ \*\*_The get\_private\_key command allows us to obtain the public key corresponding to the block signing key:_ get\_private\_key PPY4zSJHx7D84T1j6HQ7keXWdtabBBWJxvfJw72XmEyqmgdoo1njF
10. **Creating committee members:**  


    register\_account com0 PPY7GEP546p7CsMAPVQFSRGrTU11nYrCfcwyAs3r2YXHDfXBikUeM PPY7GEP546p7CsMAPVQFSRGrTU11nYrCfcwyAs3r2YXHDfXBikUeM nathan nathan 0 true  
    transfer nathan com0 100000 PPY "here is the cash" true  
    import\_key com0 5Jp4kKVPFu1aB25Sp23cDqREe3SnheoupT6Zb9Us8ztBfMFWM6L  
    register\_account com1 PPY7dzmuNGKJ5SAAdxkg6aZH7YFkuJM4Qb12WZCKwKMsPjE5VMSUf PPY7dzmuNGKJ5SAAdxkg6aZH7YFkuJM4Qb12WZCKwKMsPjE5VMSUf nathan nathan 0 true  
    transfer nathan com1 100000 PPY "here is the cash" true  
    import\_key com1 5KAdgJdAesG5kxMWKjMJBBNZBwwAiegonzCG4TZmdjzA3Stsi6n  
    register\_account com2 PPY8TXstANsSNN9rMiUzbcwfFa5bKwKEpAbUprwePPoXyH7wTuisp PPY8TXstANsSNN9rMiUzbcwfFa5bKwKEpAbUprwePPoXyH7wTuisp nathan nathan 0 true  
    transfer nathan com2 100000 PPY "here is the cash" true  
    import\_key com2 5J2jGrREGb6j72AStZ7Wj7KTXkPe5k7NdLXv3ejBioCEnC6YyFs  
    register\_account com3 PPY7bQQGWZuXBFU2e75Cg2jh57DDsfi5MrzkY4s5qHNUv7HVWvFPw PPY7bQQGWZuXBFU2e75Cg2jh57DDsfi5MrzkY4s5qHNUv7HVWvFPw nathan nathan 0 true  
    transfer nathan com3 100000 PPY "here is the cash" true  
    import\_key com3 5K6GTG8M8Cnmw9BAr4eNJCeEfAgzQgTGRQhFDq6AKFa5b9hoXm8  
    register\_account com4 PPY8ekyFfwx6gZDKNRPceHHHR2sgTLMTDgL9xSb5Jbv9L9eTNWhfn PPY8ekyFfwx6gZDKNRPceHHHR2sgTLMTDgL9xSb5Jbv9L9eTNWhfn nathan nathan 0 true  
    transfer nathan com4 100000 PPY "here is the cash" true  
    import\_key com4 5KGt6AdP7r3HdELrU1CcvdgCWb4JpR2suDuVFcSc78PmPg3U761  
    register\_account com5 PPY6TzHLATpemLxWdHLsAFjw2LLotaT6JtmvPZBfh9cat4CMMoSyr PPY6TzHLATpemLxWdHLsAFjw2LLotaT6JtmvPZBfh9cat4CMMoSyr nathan nathan 0 true  
    transfer nathan com5 100000 PPY "here is the cash" true  
    import\_key com5 5JKENoXacAQaoZicRb5n2CsSR2TRmX1Qs64AeVCPZnmi9Jc8Q2N  
    register\_account com6 PPY63SWcduLXBGt94YwYcTrJH6cDghNYzNye5ha9fDJYueZh9mjyR PPY63SWcduLXBGt94YwYcTrJH6cDghNYzNye5ha9fDJYueZh9mjyR nathan nathan 0 true  
    transfer nathan com6 100000 PPY "here is the cash" true  
    import\_key com6 5JLiAUKjuYFg91mW3U25FZDKzqih1EGnW4Ssymbv6MaaYLawu9H  
    upgrade\_account com0 true  
    upgrade\_account com1 true  
    upgrade\_account com2 true  
    upgrade\_account com3 true  
    upgrade\_account com4 true  
    upgrade\_account com5 true  
    upgrade\_account com6 true  
    create\_committee\_member com0 "[http://www.com0](http://www.com0)" true  
    create\_committee\_member com1 "[http://www.com1](http://www.com1)" true  
    create\_committee\_member com2 "[http://www.com2](http://www.com2)" true  
    create\_committee\_member com3 "[http://www.com3](http://www.com3)" true  
    create\_committee\_member com4 "[http://www.com4](http://www.com4)" true  
    create\_committee\_member com5 "[http://www.com5](http://www.com5)" true  
    create\_committee\_member com6 "[http://www.com6](http://www.com6)" true  
    vote\_for\_committee\_member nathan com0 true true  
    vote\_for\_committee\_member nathan com1 true true  
    vote\_for\_committee\_member nathan com2 true true  
    vote\_for\_committee\_member nathan com3 true true  
    vote\_for\_committee\_member nathan com4 true true  
    vote\_for\_committee\_member nathan com5 true true  
    vote\_for\_committee\_member nathan com6 true true

11. _Propose a change of a block interval:_ propose\_parameter\_change com3 "2018-05-04T15:03:11"  {"block\_interval" : 3} true

          _\*\*\* By default a proposal review period is 14 days: "committee\_proposal\_review\_period": 1209600. When a user places a new proposal, he/she should set proposal expiration date later than 14 days from the day the proposal is placed._

        _Also it might be useful to change a maintenacel interval from 24 to 1 hour:_

                  propose\_parameter\_change com5 "2018-06-10T14:30:00" {"maintenance\_interval" : 3600} true

        _Another example of sumbiting a proposal - change a draw \(asset\) creation fee:_

              propose\_fee\_change com5 "2018-05-24T00:00:00" {"asset\_create" : {"symbol3":500000, "symbol4":3000000, "long\_symbol": 500000}} false

_\*The next step would be open ports\(8090 & 9777\) for outside connection to the Testnet._

## Embedding the Genesis block \(optional\) <a id="HowtosetupaPrivateTestnetofPeerplays/5050-EmbeddingtheGenesisblock(optional)"></a>

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
  
  


#### **The instructions for installing a faucet are outdated. Use the** [**How to install PBSA Faucet**]()  **document instead !!!** <a id="HowtosetupaPrivateTestnetofPeerplays/5050-Theinstructionsforinstallingafaucetareoutdated.Usethedocumentinstead!!!"></a>

## SETTING UP THE FAUCET  <a id="HowtosetupaPrivateTestnetofPeerplays/5050-SETTINGUPTHEFAUCET"></a>

In order to allow people that do not have funds yet to enter the system, we need to setup a faucet. Here, we will also use mina as our **deployment tool** for a productive installation.  
[https://github.com/cryptonomex/faucet](https://github.com/cryptonomex/faucet)

Start the faucet : rails server -p 3000 -b 0.0.0.0

Python: python3 manage.py runserver -h 0.0.0.0 -p 5000

Here is a workaround:

```text
sudo addgroup gph
sudo adduser --system --no-create-home --ingroup gph --disabled-password --disabled-login gph
```

### **Python Library for Graphene** <a id="HowtosetupaPrivateTestnetofPeerplays/5050-PythonLibraryforGraphene"></a>

[https://github.com/xeroc/python-graphenelib](https://github.com/xeroc/python-graphenelib)

### Related articles <a id="HowtosetupaPrivateTestnetofPeerplays/5050-Relatedarticles"></a>

[https://github.com/cryptonomex/graphene/wiki/private-testnet](https://github.com/cryptonomex/graphene/wiki/private-testnet)

[https://github.com/cryptonomex/graphene/wiki/build-ubuntu](https://github.com/cryptonomex/graphene/wiki/build-ubuntu)

[http://docs.bitshares.eu/testnet/2-genesis.html](http://docs.bitshares.eu/testnet/2-genesis.html)

[https://github.com/xeroc/python-graphenelib](https://github.com/xeroc/python-graphenelib)

[https://github.com/cryptonomex/faucet](https://github.com/cryptonomex/faucet)

*  Page: [How to set up Linux testnet environment](/wiki/spaces/EKB/pages/197460035/How+to+set+up+Linux+testnet+environment)
*  Page: [How to Setup EXEchain](/wiki/spaces/EKB/pages/197460257/How+to+Setup+EXEchain)
*  Page: [How to setup Sidechain environment](/wiki/spaces/EKB/pages/197460072/How+to+setup+Sidechain+environment)
*  Page: [EVM Smart Contract testing](/wiki/spaces/EKB/pages/197460007/EVM+Smart+Contract+testing)
*  Page: [How to Create Smart Contract](/wiki/spaces/EKB/pages/197460297/How+to+Create+Smart+Contract)

Run wallet  


programs/cli\_wallet/cli\_wallet --wallet-file my-wallet.json --chain-id 3e95d1db3caa6b9c2ec95d5ed89786c7cfc6addac3509c9606ea2060acfa5431 --server-rpc-endpoint [ws://10.20.10.44:8090](exeblock-knowledge-base-how-to-setup-a-private-testnet-of-peerplays-5050.md)

Run Bitshares Faucet

rails server -p 3000 -b 0.0.0.0

