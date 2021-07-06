---
title: Using the CLI Wallet
description: A guide for node operators
---

# Using the CLI Wallet

## 1. Overview

When installing a node, the CLI wallet is crucial for completing the process. It's necessary for generating public-private key-pairs for security, block signing, and managing a backup node. It's also needed for creating a Witness or SON account. This guide will cover these topics and list all the most common functions that node operators use.

## 2. CLI Wallet Fundamentals

### 2.1. So... What is a CLI wallet?

**CLI** stands for "Command Line Interface" which means that the program uses the text-based command line window to take user input and show its output.

The Peerplays CLI wallet is a program \(named `cli_wallet`\) that is installed along with the node software when installing a Peerplays node on a server. It's used as a way to store your Peerplays account keys locally, and as a way to interact with the Peerplays chain for account and asset related transactions. You can also use the CLI wallet to lookup information from the chain.

### 2.2. CLI wallet security

The wallet is encrypted with a password of your choosing. Additionally it stores all keys locally,  never exposing your keys to anyone as it signs transactions locally before transmitting them to the connected node. The node then broadcasts the signed transactions to the network.

The wallet creates a local wallet.json file that contains the encrypted private keys required to access the funds in your account.

### 2.3. CLI wallet prerequisites

#### 2.3.1. Installation

If you have installed a Peerplays witness, API, seed, or SON node you already have the CLI wallet installed.

#### 2.3.2. Connections

The CLI wallet requires a connection to a running node to reach the blockchain. You can run the wallet using your own node or another node which allows external connections. Either way, the node needs to be synced with the chain.

### 2.4. Running the CLI wallet

#### 2.4.1. Using your own node

Once your node has synced with the blockchain, you can simply run the program:

{% tabs %}
{% tab title="Manual or GitLab Installed Nodes" %}
```text
cli_wallet
```
{% endtab %}

{% tab title="Docker Installed Nodes" %}
```text
cd /home/ubuntu/peerplays-docker
sudo ./run.sh wallet
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Since the node must be running, you will either have to open a new command line window or run the node in the background to run the CLI wallet.
{% endhint %}

#### 2.4.2. Using an external node

You can choose to connect to someone else's running and synced node. In that case you can specify the connection as a program parameter:

{% tabs %}
{% tab title="Manual or GitLab Installed Nodes" %}
```text
cli_wallet -s <Websocket Address>
```
{% endtab %}

{% tab title="Docker Installed Nodes" %}
```text
cd /home/ubuntu/peerplays-docker
sudo ./run.sh remote_wallet <Websocket Address>
```
{% endtab %}
{% endtabs %}

The `<Websocket Address>` in the code above must be replaced with the address of some public node. The address will use the websocket or secure websocket protocol \(`ws://` or `wss://` respectively\). Some node operators may have mapped their websocket address to a more friendly looking domain name which can be used here as well.

{% hint style="info" %}
It is completely safe to use the CLI wallet with an external node because your private keys are never sent to the remote server. 👍
{% endhint %}

### 2.5. Setting the password

If you have started the CLI wallet successfully, you will receive the `new >>>` prompt. At this point you'll be asked to set a password. Here's what you'll see:

```text
Please use the set_password method to initialize a new wallet before continuing
new >>>
```

Type `set_password` followed by a password of your choice and hit enter. It will look like this:

```text
new >>> set_password supersecretpassword
```

If the password was saved successfully, the prompt will change to `locked >>>`. At this point, the CLI wallet is locked and nobody can access it without the password you just set.

{% hint style="danger" %}
Be sure to remember / back up / save / write down your password \(securely of course\) because it **can't be recovered** if you lose it.
{% endhint %}

### 2.5. Unlocking the wallet

Then to unlock your wallet, you'll use the `unlock` command with your password and hit enter.

```text
locked >>> unlock supersecretpassword
```

If successful, the prompt will now read `unlocked >>>`. Your wallet is now unlocked and ready for use. 

### 2.6. Locking the wallet

After the CLI wallet has been unlocked, if there are any funds in the wallet, they are accessible. In general, `lock` the wallet and only `unlock` when it’s needed.

 To lock it, type `lock` and hit enter.

```text
unlocked >>> lock
```

The prompt will return to `locked >>>`.

### 2.7. Reset the password

If the current password needs to be changed, unlock the wallet and use `set_password` to do so.

Type `set_password` and the new password, then hit enter.

```text
unlocked >>> set_password mynewsupersecretpw
```

### 2.8. Getting help

You can get more detailed information by issuing the `gethelp` command. Detailed explanations for most calls are available. For example:

```text
unlocked >>> gethelp "list_account_balances"
```

You can also use the `help` command to get a list of all commands supported by the wallet. Note that you can use the `help` and `gethelp` commands even if the wallet is locked!

```text
unlocked >>> help
```

## 3. Wallet Command Reference for Node Operators

#### Table of Contents

* **Commands for node installs**
  * suggest\_brain\_key
  * get\_private\_key\_from\_password
  * import\_key
  * upgrade\_account
  * create\_vesting\_balance
  * create\_witness
  * create\_son
  * update\_witness
  * update\_son
  * get\_private\_key
  * dump\_private\_keys
  * get\_account
  * get\_witness
  * get\_son
  * vote\_for\_witness
  * vote\_for\_son

### 3.1. suggest\_brain\_key

Suggests a safe brain key to use for creating your account keys. A **brain key** is a long passphrase that provides enough entropy to generate cryptographic keys. This function will suggest a suitably random string that should be easy to write down \(and, with effort, memorize\).

{% hint style="info" %}
The GUI Wallet generates a brain key for your password when creating a new account.
{% endhint %}

{% code title="return type, namespace, & method" %}
```cpp
brain_key_info graphene::wallet::wallet_api::suggest_brain_key()const;
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
**Parameters**

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| ℹ This command has no parameters! | n/a | n/a | n/a |

**Example Call**

```cpp
suggest_brain_key
```
{% endtab %}

{% tab title="Return" %}
**Return Format**

```cpp
struct brain_key_info
{
   string brain_priv_key;
   string wif_priv_key;
   public_key_type pub_key;
};
```

**Example Successful Return**

```cpp
suggest_brain_key
{
  "brain_priv_key": "EDIFICE PALLID ANOESIA STRIDE PARREL SPORTY AXIFORM INOPINE SWOONED TONETIC CORKER OATEN PUSHER MIN CERN TACT",
  "wif_priv_key": "5JyK47o1xFmA6fE4Z4Pgzcdd7dGhQnorW2sTPzPxeXH7QaT4eN6",
  "pub_key": "PPY6TK9mXBaFr8ewaJa9zpgbRQVN7VkvAKay4hHGA2hDceDQvrRBv"
}
```

{% hint style="info" %}
Note that the returned "`pub_key`" value will be prefixed with:

* "`PPY`" in mainnet \(Alice\) as per the example above
* "`TEST`" in the public testnet \(Beatrice\)
{% endhint %}
{% endtab %}
{% endtabs %}

### 3.2. get\_private\_key\_from\_password

Returns the public-private key-pair for the `owner`, `active`, or `memo` role for a given account and its password.

{% code title="return type, namespace, & method" %}
```cpp
pair<public_key_type, string> graphene::wallet::wallet_api::get_private_key_from_password(
    string account,
    string role,
    string password
    )const;
```
{% endcode %}

{% tabs %}
{% tab title="Function Call" %}
**Parameters**

| name | data type | description | details |
| :--- | :--- | :--- | :--- |
| account | string | The account name we're creating keys for. | no quotes required. |
| role | string | The role we're creating keys for. One of: `owner`, `active`, or `memo` | no quotes required. |
| password | string | A brain key. It might be the password provided by the GUI wallet, or obtained from the `suggest_brain_key`command \(`wif_priv_key`\). | no quotes required. |

**Example Call**

```cpp
get_private_key_from_password myaccountname1 owner 5JyK47o1xFmA6fE4Z4Pgzcdd7dGhQnorW2sTPzPxeXH7QaT4eN6
```
{% endtab %}

{% tab title="Return" %}
**Return Format**

```cpp
pair<public_key_type, string>
[
  //the public key,
  //the private key
]
```

**Example Successful Return**

```cpp
get_private_key_from_password myaccountname1 owner 5JyK47o1xFmA6fE4Z4Pgzcdd7dGhQnorW2sTPzPxeXH7QaT4eN6
[ 
  "PPY5xmkfRJhsG54kxNpoBtWqnEpScGBxczooapTbCpmetFAmzUvJ1",
  "5KPHKeuqRyNfuc32LGDzc6tqcCPzyLgfguQzN4Xkrys3VfMxtjB"
]
```
{% endtab %}
{% endtabs %}

