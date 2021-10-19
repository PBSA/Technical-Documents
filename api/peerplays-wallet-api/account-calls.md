# Account Calls

## Account Calls

### list\_my\_accounts

Lists all accounts controlled by this wallet. This returns a list of the full account objects for all accounts whose private keys we possess

```cpp
vector<account_object> graphene::wallet::wallet_api::list_my_accounts()
```

{% tabs %}
{% tab title="Return" %}
A list of account objects
{% endtab %}
{% endtabs %}

### list\_accounts

Lists all accounts registered in the blockchain. This returns a list of all account names and their account ids, sorted by account name.

Use the `lowerbound` and limit parameters to page through the list. To retrieve all accounts, start by setting `lowerbound` to the empty string `""`, and then each iteration, pass the last account name returned as the `lowerbound` for the next `list_accounts()` call.

```cpp
map<string, account_id_type> graphene::wallet::wallet_api::list_accounts(
    const string &lowerbound, 
    uint32_t limit)
```

{% tabs %}
{% tab title="Parameters" %}
* **`lowerbound`**: the name of the first account to return. If the named account does not exist, the list will start at the account that comes after `lowerbound`
* **`limit`**: the maximum number of accounts to return (max: 1000)
{% endtab %}

{% tab title="Return" %}
A list of accounts mapping account names to account ids.
{% endtab %}
{% endtabs %}

### list\_account\_balances

List the balances of an account. Each account can have multiple balances, one for each type of asset owned by that account. The returned list will only contain assets for which the account has a non-zero balance.

```cpp
vector<asset> graphene::wallet::wallet_api::list_account_balances(
    const string &id)
```

{% tabs %}
{% tab title="Parameters" %}
* **`id`**: the name or id of the account whose balances you want
{% endtab %}

{% tab title="Return" %}
A list of the given account’s balances
{% endtab %}
{% endtabs %}

### **register\_account**

Registers a third party’s account on the blockchain.

This function is used to register an account for which you do not own the private keys. When acting as a registrar, an end user will generate their own private keys and send you the public keys. The registrar will use this function to register the account on behalf of the end user.

**See also **[create\_account\_with\_brain\_key()](https://dev.bitshares.works/en/master/api/wallet\_api.html?highlight=set\_voting\_proxy#classgraphene\_1\_1wallet\_1\_1wallet\_\_api\_1ac27928f7ca6db74e0ec4aee3ff0c545e)****

```cpp
signed_transaction graphene::wallet::wallet_api::register_account(
    string name, 
    public_key_type owner, 
    public_key_type active, 
    string registrar_account, 
    string referrer_account, 
    uint32_t referrer_percent, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`name`**: the name of the account, must be unique on the blockchain. Shorter names are more expensive to register; the rules are still in flux, but in general names of more than 8 characters with at least one digit will be cheap.
* **`owner`**: the owner key for the new account
* **`active`**: the active key for the new account
* **`registrar_account`**: the account which will pay the fee to register the user
* **`referrer_account`**: the account who is acting as a referrer, and may receive a portion of the user’s transaction fees. This can be the same as the registrar\_account if there is no referrer.
* **`referrer_percent`**: the percentage (0 - 100) of the new user’s transaction fees not claimed by the blockchain that will be distributed to the referrer; the rest will be sent to the registrar. Will be multiplied by GRAPHENE\_1\_PERCENT when constructing the transaction.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction registering the account
{% endtab %}
{% endtabs %}

### upgrade\_account

Upgrades an account to prime status. This makes the account holder a ‘lifetime member’.

```cpp
signed_transaction graphene::wallet::wallet_api::upgrade_account(
    string name, 
    bool broadcast)
```

{% tabs %}
{% tab title="Parameters" %}
* **`name`**: the name or id of the account to upgrade
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction upgrading the account
{% endtab %}
{% endtabs %}

### create\_account\_with\_brain\_key

Creates a new account and registers it on the blockchain.

**See also **[suggest\_brain\_key()](https://dev.bitshares.works/en/master/api/wallet\_api.html?highlight=set\_voting\_proxy#classgraphene\_1\_1wallet\_1\_1wallet\_\_api\_1ab936e7a26d41b35cbfaf44d369d60e1d),  [register\_account()](https://dev.bitshares.works/en/master/api/wallet\_api.html?highlight=set\_voting\_proxy#classgraphene\_1\_1wallet\_1\_1wallet\_\_api\_1aba1c5e3025f44273fb19e264c2b3ec2f)****

```cpp
signed_transaction graphene::wallet::wallet_api::create_account_with_brain_key(
    string brain_key, 
    string account_name, 
    string registrar_account, 
    string referrer_account, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`brain_key`**: the brain key used for generating the account’s private keys
* **`account_name`**: the name of the account, must be unique on the blockchain. Shorter names are more expensive to register; the rules are still in flux, but in general names of more than 8 characters with at least one digit will be cheap.
* **`registrar_account`**: the account which will pay the fee to register the user
* **`referrer_account`**: the account who is acting as a referrer, and may receive a portion of the user’s transaction fees. This can be the same as the registrar\_account if there is no referrer.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction registering the account
{% endtab %}
{% endtabs %}

### transfer

Transfer an amount from one account to another.

```cpp
signed_transaction graphene::wallet::wallet_api::transfer(
    string from, 
    string to, 
    string amount, 
    string asset_symbol, 
    string memo, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the name or id of the account sending the funds
* **`to`**: the name or id of the account receiving the funds
* **`amount`**: the amount to send (in nominal units to send half of a BTS, specify 0.5)
* **`asset_symbol`**: the symbol or id of the asset to send
* **`memo`**: a memo to attach to the transaction. The memo will be encrypted in the transaction and readable for the receiver. There is no length limit other than the limit imposed by maximum transaction size, but transaction increase with transaction size
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction transferring funds
{% endtab %}
{% endtabs %}

### transfer2

This method works just like transfer, except it always broadcasts and returns the transaction ID (hash) along with the signed transaction.

```cpp
pair<transaction_id_type, signed_transaction> graphene::wallet::wallet_api::transfer2(
    string from, 
    string to, 
    string amount, 
    string asset_symbol, 
    string memo)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the name or id of the account sending the funds
* **`to`**: the name or id of the account receiving the funds
* **`amount`**: the amount to send (in nominal units to send half of a BTS, specify 0.5)
* **`asset_symbol`**: the symbol or id of the asset to send
* **`memo`**: a memo to attach to the transaction. The memo will be encrypted in the transaction and readable for the receiver. There is no length limit other than the limit imposed by maximum transaction size, but transaction increase with transaction size
{% endtab %}

{% tab title="Return" %}
The transaction ID (hash) along with the signed transaction transferring funds
{% endtab %}
{% endtabs %}

### whitelist\_account

Whitelist and blacklist accounts, primarily for transacting in whitelisted assets.

Accounts can freely specify opinions about other accounts, in the form of either whitelisting or blacklisting them. This information is used in chain validation only to determine whether an account is authorized to transact in an asset type which enforces a whitelist, but third parties can use this information for other uses as well, as long as it does not conflict with the use of whitelisted assets.

An asset which enforces a whitelist specifies a list of accounts to maintain its whitelist, and a list of accounts to maintain its blacklist. In order for a given account A to hold and transact in a whitelisted asset S, A must be whitelisted by at least one of S’s whitelist\_authorities and blacklisted by none of S’s blacklist\_authorities. If A receives a balance of S, and is later removed from the whitelist(s) which allowed it to hold S, or added to any blacklist S specifies as authoritative, A’s balance of S will be frozen until A’s authorization is reinstated.

```cpp
signed_transaction graphene::wallet::wallet_api::whitelist_account(
    string authorizing_account, 
    string account_to_list, 
    account_whitelist_operation::account_listing new_listing_status, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`authorizing_account`**: the account who is doing the whitelisting
* **`account_to_list`**: the account being whitelisted
* **`new_listing_status`**: the new whitelisting status
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction changing the whitelisting status
{% endtab %}
{% endtabs %}

### get\_vesting\_balances

Get information about a vesting balance object or vesting balance objects owned by an account.

```cpp
vector<vesting_balance_object_with_info> graphene::wallet::wallet_api::get_vesting_balances(
    string account_name)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name`**: An account name, account ID, or vesting balance object ID.
{% endtab %}

{% tab title="Return" %}
A list of vesting balance objects with additional info
{% endtab %}
{% endtabs %}

### withdraw\_vesting

Withdraw a vesting balance.

```cpp
signed_transaction graphene::wallet::wallet_api::withdraw_vesting(
    string witness_name, 
    string amount, 
    string asset_symbol, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`witness_name`**: The account name of the witness, also accepts account ID or vesting balance ID type.
* **`amount`**: The amount to withdraw.
* **`asset_symbol`**: The symbol of the asset to withdraw.
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed transaction
{% endtab %}
{% endtabs %}

### get\_account

Returns information about the given account.

```cpp
account_object graphene::wallet::wallet_api::get_account(
    string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: the name or ID of the account to provide information about
{% endtab %}

{% tab title="Return" %}
The public account data stored in the blockchain
{% endtab %}
{% endtabs %}

### **get\_account\_id**

Lookup the id of a named account.

```cpp
account_id_type graphene::wallet::wallet_api::get_account_id(
    string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: the name or ID of the account to look up
{% endtab %}

{% tab title="Return" %}
The id of the named account
{% endtab %}
{% endtabs %}

### get\_account\_history

Returns the most recent operations on the named account.

This returns a list of operation history objects, which describe activity on the account.

```cpp
vector<operation_detail> graphene::wallet::wallet_api::get_account_history(
    string name, 
    int limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`name`**: the name or id of the account
* **`limit`**: the number of entries to return (starting from the most recent)
{% endtab %}

{% tab title="Return" %}
A list of `operation_history_objects`
{% endtab %}
{% endtabs %}

### approve\_proposal

Approve or disapprove a proposal.

```cpp
signed_transaction graphene::wallet::wallet_api::approve_proposal(
    const string &fee_paying_account, 
    const string &proposal_id, 
    const approval_delta &delta, 
    bool broadcast)
```

{% tabs %}
{% tab title="Parameters" %}
* **`fee_paying_account`**: The account paying the fee for the op.
* **`proposal_id`**: The proposal to modify.
* **`delta`**: Members contain approvals to create or remove. In JSON you can leave empty members undefined.
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed version of the transaction
{% endtab %}
{% endtabs %}
