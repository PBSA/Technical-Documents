# EXEBlock Knowledge Base : How to setup a Public Testnet

  
1. Create a new data folder:

programs/witness\_node/witness\_node --data-dir data/my-blockprod --genesis-json genesis.json

  
2. Open and edit the data/my-blockprod/config.ini file:

\# Endpoint for P2P node to listen on  
p2p-endpoint = 0.0.0.0:8092

\# P2P nodes to connect to on startup \(may specify multiple times\)  
seed-node = 10.20.10.48:8090

\# Endpoint for websocket RPC to listen on  
rpc-endpoint = 0.0.0.0:8093

\# File to read Genesis State from  
genesis-json = genesis.json

\# Enable block production, even if the chain is stale.  
enable-stale-production = true

\# ID of witness controlled by this node \(e.g. "1.6.5", quotes are required, may specify multiple times\)  
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

\# Tuple of \[PublicKey, WIF private key\] \(may specify multiple times\)  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]  
private-key = \["PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV","5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\]

3. Replace genesys.json with genesys-public-testnet.json \(mv genesys-public-testnet.json genesis.json\)

4. Compile a wallet: make cli\_wallet

5. Get the chain id:

##        _curl --data '{"jsonrpc": "2.0", "method": "get\_chain\_properties", "params": \[\], "id": 1}'_ [_http://127.0.0.1:8090/rpc_](http://127.0.0.1:8090/rpc) _&& echo_ <a id="HowtosetupaPublicTestnet-curl--data&apos;{&quot;jsonrpc&quot;:&quot;2.0&quot;,&quot;method&quot;:&quot;get_chain_properties&quot;,&quot;params&quot;:[],&quot;id&quot;:1}&apos;http://127.0.0.1:8090/rpc&amp;&amp;echo"></a>

6. Login into the wallet with the seed node's chain id:

##        _programs/cli\_wallet/cli\_wallet --wallet-file my-wallet.json --chain-id 8c2986035e5aa67de24e01546c6f652e82acd63283b43d13d7a955a04275704f --server-rpc-endpoint_ [_ws://127.0.0.1:8090_](exeblock-knowledge-base-how-to-setup-a-public-testnet.md) _-u '' -p ''_ <a id="HowtosetupaPublicTestnet-programs/cli_wallet/cli_wallet--wallet-filemy-wallet.json--chain-id8c2986035e5aa67de24e01546c6f652e82acd63283b43d13d7a955a04275704f--server-rpc-endpointws://127.0.0.1:8090-u&apos;&apos;-p&apos;&apos;"></a>

       \*\*\* Replace the “[ws://127.0.0.1:8090](exeblock-knowledge-base-how-to-setup-a-public-testnet.md)” with the rpc endpoint of the witness node.

## 7. Set a wallet password:        _set\_password supersecret_ <a id="HowtosetupaPublicTestnet-7.Setawalletpassword:set_passwordsupersecret"></a>

## 8. Unlock the wallet:        _unlock supersecret_ <a id="HowtosetupaPublicTestnet-8.Unlockthewallet:unlocksupersecret"></a>

9. Import the account name \(owner key\):

##        _import\_key nathan "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"_ <a id="HowtosetupaPublicTestnet-import_keynathan&quot;5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3&quot;"></a>

## 10. Import the balance \(it will transfer all available funds to the specified account!\):         _import\_balance nathan \["5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"\] true_ <a id="HowtosetupaPublicTestnet-10.Importthebalance(itwilltransferallavailablefundstothespecifiedaccount!):import_balancenathan[&quot;5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3&quot;]true"></a>

## 11. Make nathan account a lifetime member:         _upgrade\_account nathan true_ <a id="HowtosetupaPublicTestnet-11.Makenathanaccountalifetimemember:upgrade_accountnathantrue"></a>

**Create the "faucet" account, which will hold the balance**

12. Get a brain key:  
 

##      _suggest\_brain\_key_ <a id="HowtosetupaPublicTestnet-suggest_brain_key"></a>

{

"brain\_priv\_key": "PSHA PLEONAL RIDDLE STARTY HURROO DIAENE COWY ADORNER SAVANT ILLY CYPRINE BINARY UNDIG COSILY CODILLE TRANKER",

"wif\_priv\_key": "5JFkMirYMzkyJmTeynvMALoyio28wYyRt8SeoDMVEnEZCQjRUDp",

"pub\_key": "PPY62PwHRPZLiDmNZXPbkFD8ZCm3w4cqjv5yR6YBFt1N9eaKhB5DQ"

}

13. Register a new user "faucet" with the suggested brain key. You can either generate a new one  \(suggest\_brain\_key\) or to use one above:

##       _create\_account\_with\_brain\_key "PSHA PLEONAL RIDDLE STARTY HURROO DIAENE COWY ADORNER SAVANT ILLY CYPRINE BINARY UNDIG COSILY CODILLE TRANKER" faucet nathan nathan true_ <a id="HowtosetupaPublicTestnet-create_account_with_brain_key&quot;PSHAPLEONALRIDDLESTARTYHURROODIAENECOWYADORNERSAVANTILLYCYPRINEBINARYUNDIGCOSILYCODILLETRANKER&quot;faucetnathannathantrue"></a>

14. Check nathan balance:

##       _list\_account\_balances nathan_ <a id="HowtosetupaPublicTestnet-list_account_balancesnathan"></a>

15. Transfer nathan balance to the newly created "faucet" account:

##       _transfer nathan faucet 8498000000 PPY "here is the cash for the first true witness" true_ <a id="HowtosetupaPublicTestnet-transfernathanfaucet8498000000PPY&quot;hereisthecashforthefirsttruewitness&quot;true"></a>

     \*\*\*Make sure the "nathan" account still has at least 800000PPY \(to create committee members\)

16. Make "faucet" a lifetime member:

##       _upgrade\_account faucet true_ <a id="HowtosetupaPublicTestnet-upgrade_accountfaucettrue"></a>

**Optional \(steps 17-23\) - Create a witness:**

17 .Create a new account "alpha" similar to "faucet"

18. Transfer some funds, and make the account a lifetime member.

19. Make "alpha" a witness:

##       _create\_witness alpha "_[_http://www.alpha_](http://www.alpha)_" true_ <a id="HowtosetupaPublicTestnet-create_witnessalpha&quot;http://www.alpha&quot;true"></a>

You should see:

    {  
      "ref\_block\_num": 1007,  
      "ref\_block\_prefix": 4159002635,  
      "expiration": "2018-05-14T14:58:30",  
      "operations": \[\[  
          20,{  
        "fee": {  
          "amount": 500000000,  
          "asset\_id": "1.3.0"  
        },  
        "witness\_account": "1.2.20",  
        "url": "[http://peerplays.com/alpha](http://peerplays.com/alpha)",  
        "block\_signing\_key": "PPY51gPUZBm7q5hECdJbdGCweWZhB4z6JcYuFgqjXGNG6X8ncv8Tn",  
        "initial\_secret": "a644235d52f593a81606cf569341aeb540a721bc"  
          }  
        \]  
      \],  
      "extensions": \[\],  
      "signatures": \[  
        "1f074a966ec776e3f630cb7929de6cc86a204f10a9c480d1f52eb9ed060a9cf35456b8eb5c0a161483dc2d491d8c118bbe815c3cf2e6309809398a9ccba539491b"  
      \]  
    }  
 

20. Vote in the witness:  __

##      _vote\_for\_witness nathan aplha true true_ <a id="HowtosetupaPublicTestnet-vote_for_witnessnathanaplhatruetrue"></a>

21. Check the nest maintenance interval:

##       _get\_dynamic\_global\_properties_ <a id="HowtosetupaPublicTestnet-get_dynamic_global_properties"></a>

22. Once the next maintenance interval passes, check that the witness has been created:

##      _get\_global\_properties_ <a id="HowtosetupaPublicTestnet-get_global_properties"></a>

20. Get the witness's properties and signing key:

##     _get\_witness alpha_ <a id="HowtosetupaPublicTestnet-get_witnessalpha"></a>

21. Get the private key:

##     _dump\_private\_keys_ <a id="HowtosetupaPublicTestnet-dump_private_keys"></a>

   dump\_private\_keys  
    \[\[  
        "PPY79EAyqanyPwiP2MYhG3DkCr8Z1hxzBEY5oThaUZZ7raBAufCMc",  
        "5JyRjK3baAuZ4grqovsWP3CeahNQzMeBbgb9PsyAKDk3rm4vFwL"  
      \],\[  
        "PPY6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",  
        "5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3"  
      \],\[  
        "PPY51gPUZBm7q5hECdJbdGCweWZhB4z6JcYuFgqjXGNG6X8ncv8Tn",  
        "5KYGXHrQDiSFxyRs7TJ1ZWJDvLyxB1HFo4adqnWMGgNZiBzLoka"  
      \]  
    \]

22. Exit the wallet and node.

  
23. Open the data/my-blockprod/config.ini file

add the witness to the list of witnesses:

witness-id = “alpha”

private-key = \["PPY79EAyqanyPwiP2MYhG3DkCr8Z1hxzBEY5oThaUZZ7raBAufCMc", "5JyRjK3baAuZ4grqovsWP3CeahNQzMeBbgb9PsyAKDk3rm4vFwL"\]

  
24. Close the ini file and restart the witness node:

##       _programs/witness\_node/witness\_node --data-dir data/my-blockprod --genesis-json genesis.json_ <a id="HowtosetupaPublicTestnet-programs/witness_node/witness_node--data-dirdata/my-blockprod--genesis-jsongenesis.json"></a>

25. Open tapin/config.yml:

registrar: "faucet"

default\_referrer: "faucet"

referrer\_percent: 50 \# in percent

wif: "5JFkMirYMzkyJmTeynvMALoyio28wYyRt8SeoDMVEnEZCQjRUDp"

The node should start producing blocks.

