# Bitcoin Transactions

### Deposit Bitcoin on an account

#### Consider any peerplays user account.

Now, mapped the Bitcoin deposit and boitcoin withdrawl address on it using the below steps:  
 - In BTC node, generate BTC addresses and public key using below command

```text
bitcoin-core.cli  -rpcuser=1 -rpcpassword=1 -rpcwallet="" getnewaddress

bitcoin-core.cli -rpcuser=1 -rpcpassword=1 -rpcwallet="" getaddressinfo <<Bitcoin_address>>
```

 - Map generated btc addresses to peerplays account

```text
add_sidechain_address <<account>> bitcoin <<deposit_public_key>> <<deposit_address>> <<withdraw_public_key>> <<withdraw_address>> true
```

#### After mapping the bitcoin addresses , now make a transaction to transfer the bitcoin

Send some bitcoins to any address that is registered as a sidechain address

```text
 bitcoin-core.cli -rpcuser=1 -rpcwallet="" -rpcpassword=1 sendtoaddress <<account_deposit_address>> <<amount>> "" "" false
```

NOTE : Get the account's bitcoin deposit address using :

```text
get_sidechain_address_by_account_and_sidechain <<account>> bitcoin
```

Generate a block containing the transactions

```text
bitcoin-core.cli -rpcuser=1 -rpcpassword=1 -rpcwallet="" generatetoaddress 1 <<account_deposit_address>>
```

Here, we have successfully deposit bitcoins to any other account.



### Withdrawl Bitcoin on an account



Consider any peerplay account and mapped btc deposit and withdrawl address on it.

To withdraw BTC, transfer pBTC from user account to son-account.

```text
transfer <<account_name>> son-account <<withdraw_amount>> pBTC "" true
```

Wait for 10 minutes for the bitcoin node to generate block to make your withdrawl successfull.



