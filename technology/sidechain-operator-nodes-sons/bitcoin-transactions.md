# Depositing and Withdrawing Bitcoin on Peerplays \(Using BTC Regtest\)

{% hint style="info" %}
The following instructions are designed for use within a Bitcoin regtest network. Read more about it here: [https://developer.bitcoin.org/examples/testing.html](https://developer.bitcoin.org/examples/testing.html)
{% endhint %}

## Creating a Peerplays account

Use the [CLI Wallet](running-son-with-docker.md#using-the-cli-wallet) to suggest a brain key:

```text
# In the CLI wallet
suggest_brain_key
```

{% hint style="warning" %}
Make sure to backup the information that is output
{% endhint %}

Create an account using the brain key generated:

```text
# In the CLI wallet
create_account_with_brain_key <BRAIN-KEY> <YOUR-ACCOUNT-NAME> nathan nathan true
```

## Generating Bitcoin addresses

{% hint style="danger" %}
Wait for 15 minutes after the setup script was run \(new local networks\) before proceeding to the next steps. This is the default review period it takes for the sonaccount's to become active sons. To check if they are active:

```text
# In the CLI wallet
list_active_sons
```

The output should **not** be an empty array.
{% endhint %}

Create two bitcoin addresses and get their information \(run these commands twice\):

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getnewaddress
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getaddressinfo <BITCOIN_ADDRESS>
```

{% hint style="warning" %}
Take note of the "pubkey" value when getting the address info
{% endhint %}

## Mapping the Bitcoin addresses to a Peerplays account

```text
# In the CLI wallet
add_sidechain_address <ACCOUNT> bitcoin <BITCOIN_DEPOSIT_PUBLIC_KEY> <BITCOIN_DEPOSIT_ADDRESS> <BITCOIN_WITHDRAW_PUBLIC_KEY> <BITCOIN_WITHDRAW_ADDRESS> true
```

## Depositing Bitcoin to a Peerplays account

{% hint style="warning" %}
Blocks will first need to be mined on a new regtest network in order to have funds to send. Generate 101 blocks to a Bitcoin address to start:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli  -rpcwallet="son-wallet" generatetoaddress 101 <BITCOIN_ADDRESS>
```

Generating more blocks will increase the amount of BTC that is available on the network.
{% endhint %}

### Getting the sidechain deposit address for BTC transactions

{% hint style="danger" %}
The sidechain deposit addresses and Bitcoin deposit addresses are different.
{% endhint %}

```text
# In the CLI wallet
get_sidechain_address_by_account_and_sidechain <ACCOUNT> bitcoin
```

### Send some Bitcoin to the sidechain deposit address of a Peerplays account:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" sendtoaddress <SIDECHAIN_DEPOSIT_ADDRESS> <AMOUNT> "" "" true
```

### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

Proposals will be created on the Peerplays chain after block generation. Wait for a minute for some blocks to be generated on the Peerplays chain and then generate another block in the regtest network: 

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

{% hint style="info" %}
To check if the Bitcoin was successfully transferred to the Peerplays account list its account balance:

```text
# In the CLI wallet
list_account_balances <ACCOUNT>
```

The output will show some BTC.
{% endhint %}

## Withdrawing Bitcoin from a Peerplays account

### Transfer BTC from a Peerplays account to the `son-account`:

{% hint style="warning" %}
In order to transfer funds, the Peerplays account sending them must have some core tokens to pay the transfer fee.  
  
Fund the account with some core tokens \(TEST in this case\):

```text
# In the CLI wallet
transfer nathan <ACCOUNT> 100 TEST "" true
```
{% endhint %}

```text
# In the CLI wallet
transfer <ACCOUNT> son-account <WITHDRAW_AMOUNT> BTC "" true
```

### Generate a block in the regtest network so that the transaction goes through:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <BITCOIN_ADDRESS>
```

{% hint style="info" %}
To check if the Bitcoin was successfully sent to the withdrawal address that was mapped to the Peerplays account, check the received balance in the Bitcoin node:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getreceivedbyaddress <BITCOIN_WITHDRAW_ADDRESS>
```

The output will show some BTC.
{% endhint %}

