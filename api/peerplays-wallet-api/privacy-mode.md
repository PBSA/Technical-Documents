# Privacy Mode

## Privacy Mode

### set\_key\_label

These methods are used for stealth transfers This method can be used to set a label for a public key

{% hint style="warning" %}
**Note**: No two keys can have the same label.
{% endhint %}

```cpp
bool graphene::wallet::wallet_api::set_key_label(
    public_key_type key, 
    string label)
```

{% tabs %}
{% tab title="Parameters" %}
* **`key`**: a public key
* **`label`**: a user-defined string as label
{% endtab %}

{% tab title="Return" %}
_True_ if the label was set, otherwise _false_
{% endtab %}
{% endtabs %}

### get\_key\_label

Get label of a public key.

```cpp
string graphene::wallet::wallet_api::get_key_label(
    public_key_type key)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`key`**: a public key
{% endtab %}

{% tab title="Return" %}
The label if already set by [`set_key_label()`](privacy-mode.md#set_key_label), or an empty string if not set
{% endtab %}
{% endtabs %}

### get\_public\_key

Get the public key associated with a given label

```cpp
public_key_type graphene::wallet::wallet_api::get_public_key(
    string label)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`label`**: a label
{% endtab %}

{% tab title="Return" %}
The public key associated with the given label.
{% endtab %}
{% endtabs %}

### get\_blind\_accounts

Get all blind accounts.

```cpp
map<string, public_key_type> graphene::
wallet
::
wallet_api
::get_blind_accounts()const
```

{% tabs %}
{% tab title="Return" %}
All blind accounts
{% endtab %}
{% endtabs %}

### get\_my\_blind\_accounts

Get all blind accounts for which this wallet has the private key.

```cpp
map<string, public_key_type> graphene::wallet::wallet_api::get_my_blind_accounts()const
```

{% tabs %}
{% tab title="Return" %}
All blind accounts for which this wallet has the private key.
{% endtab %}
{% endtabs %}

### get\_blind\_balances

Return the total balances of all blinded commitments that can be claimed by the given account key or label.

```cpp
vector<asset> graphene::wallet::wallet_api::get_blind_balances(
    string key_or_label)
```

{% tabs %}
{% tab title="Parameters" %}
* **`key_or_label`**: a public key in Base58 format or a label
{% endtab %}

{% tab title="Return" %}
The total balances of all blinded commitments that can be claimed by the given account key or label
{% endtab %}
{% endtabs %}

### **create\_blind\_account**

Generates a new blind account for the given brain key and assigns it the given label

```cpp
public_key_type graphene::
wallet
::
wallet_api
::create_blind_account(string label, string brain_key)
```

{% tabs %}
{% tab title="Parameters" %}
* **`label`**: a label
* **`brain_key`**: the brain key to be used to generate a new blind account
{% endtab %}

{% tab title="Return" %}
The public key of the new account
{% endtab %}
{% endtabs %}

### transfer\_to\_blind

Transfers a public balance from `from_account_id_or_name` to one or more blinded balances using a stealth transfer.

```cpp
blind_confirmationgraphene::wallet::wallet_api::transfer_to_blind(
    string from_account_id_or_name, 
    string asset_symbol, 
    vector<pair<string, string>> to_amounts, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from_account_id_or_name`**: ID or name of an account to transfer from
* **`asset_symbol`**: symbol or ID of the asset to be transferred
* **`to_amounts`**: map from key or label to amount
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
A blind confirmation
{% endtab %}
{% endtabs %}

### transfer\_from\_blind

Transfers funds from a set of blinded balances to a public account balance.

```cpp
blind_confirmationgraphene::wallet::wallet_api::transfer_from_blind(
    string from_blind_account_key_or_label, 
    string to_account_id_or_name, 
    string amount, 
    string asset_symbol, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from_blind_account_key_or_label`**: a public key in Base58 format or a label to transfer from
* **`to_account_id_or_name`**: ID or name of an account to transfer to
* **`amount`**: the amount to be transferred
* **`asset_symbol`**: symbol or ID of the asset to be transferred
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
A blind confirmation.
{% endtab %}
{% endtabs %}

### blind\_transfer

Transfer from one set of blinded balances to another.

```cpp
blind_confirmationgraphene::wallet::wallet_api::blind_transfer(
    string from_key_or_label, 
    string to_key_or_label, 
    string amount, 
    string symbol, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from_key_or_label`**: a public key in Base58 format or a label to transfer from
* **`to_key_or_label`**: a public key in Base58 format or a label to transfer to
* **`amount`**: the amount to be transferred
* **`symbol`**: symbol or ID of the asset to be transferred
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
A blind confirmation
{% endtab %}
{% endtabs %}

### blind\_history

Get all blind receipts to/form a particular account.

```cpp
vector<blind_receipt> graphene::wallet::wallet_api::blind_history(
    string key_or_account)
```

{% tabs %}
{% tab title="Parameters" %}
* **`key_or_account`**: a public key in Base58 format or an account
{% endtab %}

{% tab title="Return" %}
All blind receipts to/form the account.
{% endtab %}
{% endtabs %}

### **receive\_blind\_transfer**

Given a confirmation receipt, this method will parse it for a blinded balance and confirm that it exists in the blockchain. If it exists then it will report the amount received and who sent it.

```cpp
blind_receipt graphene::
wallet
::
wallet_api
::receive_blind_transfer(string confirmation_receipt, string opt_from, string opt_memo)
```

{% tabs %}
{% tab title="Parameters" %}
* **`confirmation_receipt`**: a base58 encoded stealth confirmation
* **`opt_from`**: if not empty and the sender is a unknown public key, then the unknown public key will be given the label `opt_from`
* **`opt_memo`**: a self-defined label for this transfer to be saved in local wallet file
{% endtab %}

{% tab title="Return" %}
A blind receipt.
{% endtab %}
{% endtabs %}

