# Bitcoin Transactions

### Creating a Peerplays Account

Use the [CLI Wallet](./#using-the-cli-wallet) to suggest a brain key:

```text
# In the CLI wallet
suggest_brain_key
```

{% hint style="warning" %}
Make sure to backup the information that is outputted
{% endhint %}

Create an account using the brain key generated:

```text
# In the CLI wallet
create_account_with_brain_key <BRAIN-KEY> <YOUR-ACCOUNT-NAME> nathan  nathan true
```

### Deposit Bitcoin to an account

Create two bitcoin addresses and get their information:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getnewaddress
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" getaddressinfo <BITCOIN_ADDRESS>
```

{% hint style="warning" %}
Take note of the "pubkey" value when getting the address info
{% endhint %}

Map the generated Bitcoin addresses to a Peerplays account:

```text
# In the CLI wallet
add_sidechain_address <ACCOUNT> bitcoin <DEPOSIT_PUBLIC_KEY> <DEPOSIT_ADDRESS> <WITHDRAW_PUBLIC_KEY> <WITHDRAW_ADDRESS> true
```

Supply the mapped deposit address with Bitcoin:

```text
# In the local terminal
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" sendtoaddress <DEPOSIT_ADDRESS> <AMOUNT> "" "" true
```

{% hint style="info" %}
To get the deposit address of an account use:

```text
# In the CLI wallet
get_sidechain_address_by_account_and_sidechain <ACCOUNT> bitcoin
```
{% endhint %}

Generate a block so that the transaction goes through:

```text
docker exec bitcoind-node bitcoin-cli -rpcwallet="son-wallet" generatetoaddress 1 <DEPOSIT_ADDRESS>
```

### Withdraw Bitcoin to an account

To withdraw BTC, transfer pBTC from an account to the `son-account`:

```text
transfer <ACCOUNT> son-account <WITHDRAW_AMOUNT> pBTC "" true
```

Wait for 10 minutes for the bitcoin node to generate block to make your withdrawal successful.



