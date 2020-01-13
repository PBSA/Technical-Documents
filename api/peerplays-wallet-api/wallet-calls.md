# Wallet Calls

## Wallet Calls

### is\_new

Checks whether the wallet has just been created and has not yet had a password set.

Calling `set_password` will transition the wallet to the locked state.

```cpp
bool graphene::wallet::wallet_api::is_new()const
```

{% tabs %}
{% tab title="Return" %}
_True_ if the wallet is new.
{% endtab %}
{% endtabs %}

### is\_locked

Checks whether the wallet is locked \(is unable to use its private keys\).

This state can be changed by calling [`lock()`](wallet-calls.md#lock) or [`unlock()`](wallet-calls.md#unlock)\`\`

```cpp
bool graphene::wallet::wallet_api::is_locked()const
```

{% tabs %}
{% tab title="Return" %}
_True_ if the wallet is locked
{% endtab %}
{% endtabs %}

### lock

Locks the wallet immediately.

```cpp
void graphene::wallet::wallet_api::lock()
```

### unlock

Unlocks the wallet.

The wallet remain unlocked until the `lock` is called or the program exits.

When used in command line, if typed “unlock” without a password followed, the user will be prompted to input a password without echo.

```cpp
void graphene::wallet::wallet_api::unlock(
    string password)
```

{% tabs %}
{% tab title="Parameters" %}
* **`password`**: the password previously set with [`set_password()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a2a5d174ec4fde8633b8e962fabc00804)
{% endtab %}
{% endtabs %}

### **set\_password**

Sets a new password on the wallet.

The wallet must be either ‘new’ or ‘unlocked’ to execute this command.

When used in command line, if typed “set\_password” without a password followed, the user will be prompted to input a password without echo.

```cpp
void graphene::wallet::wallet_api::set_password(
    string password)
```

{% tabs %}
{% tab title="Parameters" %}
* **`password`**: a new password
{% endtab %}
{% endtabs %}

### **dump\_private\_keys**

Dumps all private keys owned by the wallet.

The keys are printed in WIF format. You can import these keys into another wallet using [`import_key()`](wallet-calls.md#import_key)\`\`

```cpp
map<public_key_type, string> graphene::wallet::wallet_api::dump_private_keys()
```

{% tabs %}
{% tab title="Return" %}
A ****map containing the private keys, indexed by their public key
{% endtab %}
{% endtabs %}

### import\_key

Imports the private key for an existing account.

The private key must match either an owner key or an active key for the named account.

See also ****[`dump_private_keys()`](wallet-calls.md#dump_private_keys)\`\`

```cpp
bool graphene::wallet::wallet_api::import_key(
    string account_name_or_id, 
    string wif_key)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: the account owning the key
* **`wif_key`**: the private key in WIF format
{% endtab %}

{% tab title="Return" %}
_true_ if the key was imported
{% endtab %}
{% endtabs %}

### import\_accounts

Imports accounts from a BitShares 0.x wallet file. Current wallet file must be unlocked to perform the import.

```cpp
map<string, bool> graphene::wallet::wallet_api::import_accounts(
    string filename, 
    string password)
```

{% tabs %}
{% tab title="Parameters" %}
* **`filename`**: the BitShares 0.x wallet file to import
* **`password`**: the password to encrypt the BitShares 0.x wallet file
{% endtab %}

{% tab title="Return" %}
A map containing the accounts found and whether imported.
{% endtab %}
{% endtabs %}

### import\_account\_keys

Imports from a BitShares 0.x wallet file, find keys that were bound to a given account name on the BitShares 0.x chain, rebind them to an account name on the 2.0 chain. Current wallet file must be unlocked to perform the import.

```cpp
bool graphene::wallet::wallet_api::import_account_keys(
    string filename, 
    string password, 
    string src_account_name, 
    string dest_account_name)
```

{% tabs %}
{% tab title="First Tab" %}
* **`filename`**: the BitShares 0.x wallet file to import
* **`password`**: the password to encrypt the BitShares 0.x wallet file
* **`src_account_name`**: name of the account on BitShares 0.x chain
* **`dest_account_name`**: name of the account on BitShares 2.0 chain, can be same or different to `src_account_name`
{% endtab %}

{% tab title="Return" %}
Whether the import has succeeded
{% endtab %}
{% endtabs %}

### import\_balance

This call will construct transaction\(s\) that will claim all balances controlled by wif\_keys and deposit them into the given account.

```cpp
vector<signed_transaction> graphene::wallet::wallet_api::import_balance(
    string account_name_or_id, 
    const vector<string> &wif_keys, 
    bool broadcast)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: name or ID of an account that to claim balances to
* **`wif_keys`**: private WIF keys of balance objects to claim balances from
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}
{% endtabs %}

### suggest\_brain\_key

Suggests a safe brain key to use for creating your account. [`create_account_with_brain_key()`](account-calls.md#create_account_with_brain_key) requires you to specify a ‘brain key’, a long passphrase that provides enough entropy to generate cryptographic keys. 

This function will suggest a suitably random string that should be easy to write down \(and, with effort, memorize\).

```cpp
brain_key_info graphene::wallet::wallet_api::suggest_brain_key()const
```

{% tabs %}
{% tab title="Return" %}
A suggested brain\_key
{% endtab %}
{% endtabs %}

### get\_transaction\_id

This method is used to convert a JSON transaction to its transacting ID.

```cpp
transaction_id_type graphene::wallet::wallet_api::get_transaction_id(
    const signed_transaction &trx)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`trx`**: a JSON transaction
{% endtab %}

{% tab title="Return" %}
The ID \(hash\) of the transaction.
{% endtab %}
{% endtabs %}

### **get\_private\_key**

Get the WIF private key corresponding to a public key. The private key must already be in the wallet.

```cpp
string graphene::wallet::wallet_api::get_private_key(
    public_key_type pubkey)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`pubkey`**: a public key in Base58 format
{% endtab %}

{% tab title="Return" %}
**T**he WIF private key
{% endtab %}
{% endtabs %}

### **load\_wallet\_file**

Loads a specified Graphene wallet.

The current wallet is closed before the new wallet is loaded.

{% hint style="danger" %}
**Important:** This does not change the filename that will be used for future wallet writes, so this may cause you to overwrite your original wallet unless you also call `set_wallet_filename()`
{% endhint %}

```cpp
bool graphene::wallet::wallet_api::load_wallet_file(
    string wallet_filename = "")
```

{% tabs %}
{% tab title="Parameters" %}
* **`wallet_filename`**: the filename of the wallet JSON file to load. If `wallet_filename` is empty, it reloads the existing wallet file.
{% endtab %}

{% tab title="Return" %}
_True_ if the specified wallet is loaded.
{% endtab %}
{% endtabs %}

### normalize\_brain\_key

Transforms a brain key to reduce the chance of errors when re-entering the key from memory.

This takes a user-supplied brain key and normalizes it into the form used for generating private keys. In particular, this upper-cases all ASCII characters and collapses multiple spaces into one.

```cpp
string graphene::wallet::wallet_api::normalize_brain_key(
    string s)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`s`**: the brain key as supplied by the user
{% endtab %}

{% tab title="Return" %}
The brain key in its normalized form.
{% endtab %}
{% endtabs %}

\*\*\*\*

* \*\*\*\*[**save\_wallet\_file**](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id25)\*\*\*\*

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::save_wallet_file`\(string _wallet\_filename_ = ""\)  


Saves the current wallet to the given filename.

**Warning**

This does not change the wallet filename that will be used for future writes, so think of this function as ‘Save a Copy As…’ instead of ‘Save As…’. Use [`set_wallet_filename()`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#classgraphene_1_1wallet_1_1wallet__api_1aa5804e1ee29ff8f2c3bcc668ad2bfbcd) to make the filename persist.**Parameters**

* `wallet_filename`: the filename of the new wallet JSON file to create or overwrite. If `wallet_filename` is empty, save to the current filename.

