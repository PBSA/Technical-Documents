# Creating a witness account

For given network, say MAINNET or TESTNET we need to first create a user account using a faucet.&#x20;

{% hint style="info" %}
**Faucet ?!**

In the blockchain world, account creation services are nicknamed as faucets. They create unique Network Address Identifiers.
{% endhint %}

To create a Peerplays MAINNET account, the easiest way is to use the GUI wallet available to download from [Github](https://github.com/peerplays-network/peerplays-core-gui/releases).

Creating user in Peerplays GUI wallet is self explanatory.

#### 2. Upgrading the Peerplays account to a witness account

We can do this from the `cli_wallet` & this includes multiple steps.

1. Buy some PPY (or ask your friendly whale!) from an exchange
2. Upgrade your new account to a Life Time Member (LTM)
3. register and update the witness to the network

For the first step, we need to access the newly created Peerplays account from the cli\_wallet. Instructions are given below:.

{% content-ref url="../obtaining-private-keys-for-use-in-cli_wallet.md" %}
[obtaining-private-keys-for-use-in-cli\_wallet.md](../obtaining-private-keys-for-use-in-cli\_wallet.md)
{% endcontent-ref %}



## CLI Wallet Setup

In it's own terminal window, start the command-line wallet `cli_wallet`:

```
./programs/cli_wallet/cli_wallet
```

To set your initial password to` password` use:

```
>>> set_password password
>>> unlock password
```

{% hint style="info" %}
**Tip**: A list of CLI wallet commands is available here:[ ](https://github.com/PBSA/peerplays/blob/master/libraries/wallet/include/graphene/wallet/api\_documentation.hpp)[CLI Wallet commands](https://github.com/PBSA/peerplays/blob/master/libraries/wallet/include/graphene/wallet/api\_documentation.hpp)
{% endhint %}

### Get Owner and Active Keys

To generate your owner and active keys use the `get_private_key_from_password` command:

```
get_private_key_from_password your_witness_username active the_key_you_received_from_the_faucet
```

This will return an array, with your active key, in the form `["PPYxxx", "xxxx"].`

### Import Keys into your CLI Wallet

To import your keys, generated in the previous step, into your CLI wallet do the following:

1. Use the second value in the array for the private key.
2. Make sure your username is in quotes.&#x20;
3. Import the key using the following command&#x20;

{% hint style="warning" %}
**Note**: Substitute `your-witness-username` with your own username.
{% endhint %}

```
import_key "your_witness_username" xxxx
```

### Upgrade Your Account to Lifetime Membership

```
upgrade_account your_witness_username true
```

### Create Yourself as a Witness

Make sure your URL is in quotes.&#x20;

{% hint style="warning" %}
**Note**: Substitute `"url"` with your witness URL.
{% endhint %}

```
create_witness your_witness_username "url" true
```

This will return your `block_signing_key`

{% hint style="danger" %}
**Important**: Be sure to take note of  your `block_signing_key`
{% endhint %}

Run the following command using your `block_signing_key` from the previous step:

```
get_private_key block_signing_key
```

Compare this result to the three pairs of keys generated when you run the following command:

```
dump_private_keys
```

One of the pairs should match your `block_signing_key` , this is the one you'll use for future operations.

### Get Your Witness ID

{% hint style="warning" %}
**Note**: Substitute `username` with you Witness username.
{% endhint %}

```
get_witness username 
```

Take note of the `id` for the next step.

### Modify Your Witness Node Configuration

Next the Witness node configuration file, `witness_node_data_dir/config.ini`, needs to be modified to include your Witness ID (from the previous step), and your private key pair.

{% hint style="warning" %}
**Note**: Substitute `"your_witness_id"` with your Witness ID.&#x20;

Don't forget to enclose in quotes!
{% endhint %}

{% hint style="warning" %}
**Note**: Substitute `"block_signing_key"` with your block signing.

Substitute `"private_key_for_your_block_signing_key"` with your private key.

Don't forget to enclose in quotes!
{% endhint %}

```
vim witness_node_data_dir/config.ini

witness-id = "your_witness_id"
private-key = ["block_signing_key","private_key_for_your_block_signing_key"]
```

### Start Your Witness Node

```
./programs/witness_node/witness_node
```

If your Witness node fails to start, try again with these flags:

{% hint style="danger" %}
**Important**: Not for permanent use.
{% endhint %}

```
./programs/witness_node/witness_node --resync --replay
```

### Vote For Yourself

All Witnesses have to be voted in, so start by voting for yourself!

```
vote_for_witness your_witness_account your_witness_account true true
```

### Ask to be Voted In

The following document gives information on the Peerplays Witness voting process and advice on how to get voted in:

{% content-ref url="../becoming-a-witness.md" %}
[becoming-a-witness.md](../becoming-a-witness.md)
{% endcontent-ref %}

Once you've received enough votes to become an active Witness you'll start producing blocks at the next maintenance interval (once per hour). You can check your votes with:

```
get_witness your_witness_account
```

###
