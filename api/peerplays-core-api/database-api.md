# Database API

The database API is available from the full node via web-sockets.

## Objects

### get\_objects

Get the objects corresponding to the provided IDs.

If any of the provided IDs does not map to an object, a null variant is returned in its position.

```cpp
fc::variants graphene::app::database_api::get_objects(
    const vector<object_id_type> &ids, 
    optional<bool> subscribe = optional<bool>())const
```

{% tabs %}
{% tab title="Parameters" %}
* **`ids`**: IDs of the objects to retrieve
* **`subscribe`**: _true_ to subscribe to the queried objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\)
{% endtab %}

{% tab title="Return" %}
The objects retrieved, in the order they are mentioned in ids.
{% endtab %}
{% endtabs %}

## Subscriptions

### set\_subscribe\_callback

Register a callback handle which then can be used to subscribe to object database changes.

{% hint style="warning" %}
**Note**: auto-subscription is enabled by default and can be disabled with [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c) 
{% endhint %}

```cpp
void graphene::app::database_api::set_subscribe_callback(
    std::function<void(const variant&)> cb, 
    bool notify_remove_create, )

```

{% tabs %}
{% tab title="Parameters" %}
* **`cb`**: The callback handle to register
* **`notify_remove_create`**: Whether subscribe to universal object creation and removal events. If this is set to true, the API server will notify all newly created objects and ID of all newly removed objects to the client, no matter whether client subscribed to the objects. By default, API servers don’t allow subscribing to universal events, which can be changed on server startup.
{% endtab %}
{% endtabs %}

### set\_pending\_transaction\_callback

Register a callback handle which will get notified when a transaction is pushed to database.

{% hint style="warning" %}
**Note**: A transaction can be pushed to the database and be popped from the database several times while processing, before and after,, included in a block. Every time a push is done, the client will be notified.
{% endhint %}

```cpp
void graphene::app::database_api::set_pending_transaction_callback(
    std::function<void(const variant &signed_transaction_object)> cb)
```

{% tabs %}
{% tab title="Parameters" %}
* **`cb`**: The callback handle to register
{% endtab %}
{% endtabs %}

### set\_block\_applied\_callback

Register a callback handle which will get notified when a block is pushed to database.

```cpp
void graphene::app::database_api::set_block_applied_callback(
    std::function<void(const variant &block_id)> cb)
```

{% tabs %}
{% tab title="Parameters" %}
* **`cb`**: The callback handle to register
{% endtab %}
{% endtabs %}

### cancel\_all\_subscriptions

Stop receiving any notifications.

This unsubscribes from all subscribed markets and objects.

```cpp
void graphene::app::database_api::cancel_all_subscriptions()
```

## Blocks and transactions

### get\_block\_header

Retrieve a block header.

```cpp
optional<block_header> graphene::app::database_api::get_block_header(
    uint32_t block_num)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`block_num`**: Height of the block whose header should be returned
{% endtab %}

{% tab title="Return" %}
* The header of the referenced block, or null if no matching block was foun
{% endtab %}
{% endtabs %}

### get\_block

Retrieve a full, signed block.

```cpp
optional<signed_block> graphene::app::database_api::get_block(
    uint32_t block_num)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`block_num`**: Height of the block to be returned
{% endtab %}

{% tab title="Return" %}
The referenced block, or null if no matching block was found.
{% endtab %}
{% endtabs %}

### get\_transaction

Fetch an individual transaction.

```cpp
processed_transaction graphene::app::database_api::get_transaction(
    uint32_t block_num, uint32_t trx_in_block)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`block_num`**: height of the block to fetch
* **`trx_in_block`**: the index \(sequence number\) of the transaction in the block, starts from 0
{% endtab %}

{% tab title="Return" %}
The transaction at the given position.
{% endtab %}
{% endtabs %}

### get\_recent\_transaction\_by\_id

```cpp
optional<signed_transaction> graphene::app::database_api::get_recent_transaction_by_id(
    const transaction_id_type &txid)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`txid`**: hash of the transaction
{% endtab %}

{% tab title="Return" %}
The corresponding transaction if found, or null if not found.
{% endtab %}
{% endtabs %}

If the transaction has not expired, this method will return the transaction for the given ID or it will return NULL if it is not known. Just because it is not known does not mean it wasn’t included in the blockchain.

## Globals

### get\_chain\_properties

Retrieve the [graphene::chain::chain\_property\_object](https://dev.bitshares.works/en/master/api/namespaces/chain.html#classgraphene_1_1chain_1_1chain__property__object) associated with the chain.

```cpp
chain_property_object graphene::app::database_api::get_chain_properties()const
```

### get\_global\_properties

Retrieve the current [graphene::chain::global\_property\_object](https://dev.bitshares.works/en/master/api/namespaces/chain.html#classgraphene_1_1chain_1_1global__property__object).

```cpp
global_property_object graphene::app::database_api::get_global_properties()const
```

### get\_config

Retrieve compile-time constants.

```cpp
fc::variant_object graphene::app::database_api::get_config()const
```

### get\_chain\_id

Get the chain ID.

```cpp
chain_id_type graphene::app::database_api::get_chain_id()cons
```

### get\_dynamic\_global\_properties

Retrieve the current [graphene::chain::dynamic\_global\_property\_object](https://dev.bitshares.works/en/master/api/namespaces/chain.html#classgraphene_1_1chain_1_1dynamic__global__property__object).

```cpp
dynamic_global_property_object graphene::app::database_api::get_dynamic_global_properties()const


```

## Keys

### get\_key\_references

Get all accounts that refer to the specified public keys in their owner authority, active authorities or memo key.

```cpp
vector<flat_set<account_id_type>> graphene::app::database_api::get_key_references(
    vector<public_key_type> keys)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`keys`**: a list of public keys to query
{% endtab %}

{% tab title="Return" %}
ID of all accounts that refer to the specified keys.
{% endtab %}
{% endtabs %}

## Accounts

### get\_accounts

Get a list of accounts by names or IDs.

This function has semantics identical to[get\_objects](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a1f20e51d290fc3ac2409c49c058585b3)\*\*\*\*

```cpp
vector<optional<account_object>> graphene::app::database_api::get_accounts(
    const vector<std::string> &account_names_or_ids, 
    optional<bool> subscribe = optional<bool>())const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_names_or_ids`**: names or IDs of the accounts to retrieve
* **`subscribe`**: _true_ to subscribe to the queried account objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\)
{% endtab %}

{% tab title="Return" %}
The accounts corresponding to the provided names or IDs.
{% endtab %}
{% endtabs %}

### get\_full\_accounts

Fetch all objects relevant to the specified accounts and optionally subscribe to updates.

This function fetches all relevant objects for the given accounts, and subscribes to updates to the given accounts. If any of the strings in`names_or_ids` cannot be tied to an account, that input will be ignored. All other accounts will be retrieved and subscribed.

```cpp
std::map<string, full_account> graphene::app::database_api::get_full_accounts(
    const vector<string> &names_or_ids, 
    optional<bool> subscribe = optional<bool>())
```

{% tabs %}
{% tab title="Parameters" %}
* **`names_or_ids`**: Each item must be the name or ID of an account to retrieve
* **`subscribe`**: _true_ to subscribe to the queried full account objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\)
{% endtab %}

{% tab title="Return" %}
Map of string from `names_or_ids` to the corresponding account.
{% endtab %}
{% endtabs %}

### get\_account\_by\_name

Get info of an account by name.

```cpp
optional<account_object> graphene::app::database_api::get_account_by_name(
    string name)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`name`**: Name of the account to retrieve
{% endtab %}

{% tab title="Return" %}
The account holding the provided name.
{% endtab %}
{% endtabs %}

### get\_account\_references

Get all accounts that refer to the specified account in their owner or active authorities.

```cpp
vector<account_id_type> graphene::app::database_api::get_account_references(
    const std::string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: Account name or ID to query
{% endtab %}

{% tab title="Return" %}
All accounts that refer to the specified account in their owner or active authorities
{% endtab %}
{% endtabs %}

### lookup\_account\_names

Get a list of accounts by name.

This function has semantics identical to [get\_objects](database-api.md#get_objects), but doesn’t subscribe

```cpp
vector<optional<account_object>> graphene::app::database_api::lookup_account_names(
    const vector<string> &account_names)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`account_names`**: Names of the accounts to retrieve
{% endtab %}

{% tab title="Return" %}
The accounts holding the provided names.
{% endtab %}
{% endtabs %}

### lookup\_accounts

Get names and IDs for registered accounts.

{% hint style="warning" %}
**Note**: In addition to the common auto-subscription rules, this API will subscribe to the returned account only if `limit` is 1.
{% endhint %}

```cpp
map<string, account_id_type> graphene::app::database_api::lookup_accounts(
    const string &lower_bound_name, 
    uint32_t limit, 
    optional<bool> subscribe = optional<bool>())const

```

{% tabs %}
{% tab title="Parameters" %}
* **`lower_bound_name`**: Lower bound of the first name to return
* **`limit`**: Maximum number of results to return must not exceed 1000
* **`subscribe`**: _true_ to subscribe to the queried account objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\).
{% endtab %}

{% tab title="Return" %}
Map of account names to corresponding IDs.
{% endtab %}
{% endtabs %}

### get\_account\_count

Get the total number of accounts registered with the blockchain.

```cpp
uint64_t graphene::app::database_api::get_account_count()const

```

## Balances

### get\_account\_balances

Get an account’s balances in various assets.

```cpp
vector<asset> graphene::app::database_api::get_account_balances(
    const std::string &account_name_or_id, const flat_set<asset_id_type> &assets)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: name or ID of the account to get balances for.
* **`assets`**: IDs of the assets to get balances of; if empty, get all assets account has a balance in.
{% endtab %}

{% tab title="Return" %}
Balances of the account.
{% endtab %}
{% endtabs %}

### get\_named\_account\_balances

Semantically equivalent to [get\_account\_balances](database-api.md#get_account_balances).

```cpp
vector<asset> graphene::app::database_api::get_named_account_balances(
    const std::string &name, 
    const flat_set<asset_id_type> &assets)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: name or ID of the account to get balances for.
* **`assets`**: IDs of the assets to get balances of; if empty, get all assets account has a balance in.
{% endtab %}

{% tab title="Return" %}
Balances of the account.
{% endtab %}
{% endtabs %}

### get\_balance\_objects

```cpp
vector<balance_object> graphene::app::database_api::get_balance_objects(
    const vector<address> &addrs)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`addrs`**: a list of addresses
{% endtab %}

{% tab title="Return" %}
All unclaimed balance objects for the addresses.
{% endtab %}
{% endtabs %}

### get\_vested\_balances

Calculate how much assets in the given balance objects are able to be claimed at current head block time.

```cpp
vector<asset> graphene::app::database_api::get_vested_balances(
    const vector<balance_id_type> &objs)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`objs`**: a list of balance object IDs
{% endtab %}

{% tab title="Return" %}
A list indicating how much asset in each balance object is available to be claimed.
{% endtab %}
{% endtabs %}

### get\_vesting\_balances

Return all vesting balance objects owned by an account.

```cpp
vector<vesting_balance_object> graphene::app::database_api::get_vesting_balances(
    const std::string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: name or ID of an account
{% endtab %}

{% tab title="Return" %}
All vesting balance objects owned by the account.
{% endtab %}
{% endtabs %}

## Assets

### get\_assets

Get a list of assets by symbol names or IDs.

Semantically equivalent to [get\_objects](database-api.md#get_objects).

```cpp
vector<optional<extended_asset_object>> graphene::app::database_api::get_assets(
    const vector<std::string> &asset_symbols_or_ids, 
    optional<bool> subscribe = optional<bool>())const
```

{% tabs %}
{% tab title="Parameters" %}
* **`asset_symbols_or_ids`**: symbol names or IDs of the assets to retrieve
* **`subscribe`**: _true_ to subscribe to the queried asset objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\)
{% endtab %}

{% tab title="Return" %}
The assets corresponding to the provided symbol names or IDs.
{% endtab %}
{% endtabs %}

### list\_assets

Get assets alphabetically by symbol name.

```cpp
vector<extended_asset_object> graphene::
app
::
database_api
::list_assets(const string &lower_bound_symbol, uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`lower_bound_symbol`**: Lower bound of symbol names to retrieve
* **`limit`**: Maximum number of assets to fetch \(must not exceed 101\)
{% endtab %}

{% tab title="Return" %}
The assets found.
{% endtab %}
{% endtabs %}

### lookup\_asset\_symbols

Get a list of assets by symbol names or IDs.

Semantically equivalent to [get\_objects](database-api.md#get_objects), but doesn’t subscribe.

```cpp
vector<optional<extended_asset_object>> graphene::app::database_api::lookup_asset_symbols(
    const vector<string> &symbols_or_ids)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`symbols_or_ids`**: symbol names or IDs of the assets to retrieve
{% endtab %}

{% tab title="Return" %}
The assets corresponding to the provided symbols or IDs
{% endtab %}
{% endtabs %}

## Markets / Feeds

### get\_order\_book

Returns the order book for the market base

```cpp
order_book graphene::app::database_api::get_order_book(
    const string &base, 
    const string &quote, 
    unsigned limit = 50)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`base`**: symbol name or ID of the base asset
* **`quote`**: symbol name or ID of the quote asset
* **`limit`**: depth of the order book to retrieve, for bids and asks each, capped at 50
{% endtab %}

{% tab title="Return" %}
Order book of the market.
{% endtab %}
{% endtabs %}

### get\_limit\_orders

Get limit orders in a given market.

```cpp
vector<limit_order_object> graphene::app::database_api::get_limit_orders(
    std::string a, 
    std::string b, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`a`**: symbol or ID of asset being sold
* **`b`**: symbol or ID of asset being purchased
* **`limit`**: Maximum number of orders to retrieve
{% endtab %}

{% tab title="Return" %}
The limit orders, ordered from least price to greatest.
{% endtab %}
{% endtabs %}

### get\_call\_orders

Get call orders \(aka margin positions\) for a given asset.

```cpp
vector<call_order_object> graphene::app::database_api::get_call_orders(
    const std::string &a, 
    uint32_t limit)const

```

{% tabs %}
{% tab title="Parameters" %}
* `a`: symbol name or ID of the debt asset
* `limit`: Maximum number of orders to retrieve
{% endtab %}

{% tab title="Return" %}
The call orders, ordered from earliest to be called to latest
{% endtab %}
{% endtabs %}

### get\_settle\_orders

Get forced settlement orders in a given asset.

```cpp
vector<force_settlement_object> graphene::app::database_api::get_settle_orders(
    const std::string &a, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`a`**: Symbol or ID of asset being settled
* **`limit`**: Maximum number of orders to retrieve
{% endtab %}

{% tab title="Return" %}
The settle orders, ordered from earliest settlement date to latest.
{% endtab %}
{% endtabs %}

### get\_margin\_positions

Get all open margin positions of a given account.

Similar to [get\_call\_orders\_by\_account](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a78eb082a3a0cfb33ccd00adeb8cfac1d), but without pagination.

```cpp
vector<call_order_object> graphene::app::database_api::get_margin_positions(
    const std::string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* `account_name_or_id`: name or ID of an account
{% endtab %}

{% tab title="Return" %}
All open margin positions of the account.
{% endtab %}
{% endtabs %}

### subscribe\_to\_market

Request notification when the active orders in the market between two assets changes.

Callback will be passed a variant containing a vector&lt;pair&lt;operation, operation\_result&gt;&gt;. 

The vector will contain, in order, the operations which changed the market, and their results

```cpp
void graphene::
app
::
database_api
::subscribe_to_market(std::function<void(const variant&)> callback, const std::string &a, const std::string &b, )

```

{% tabs %}
{% tab title="Parameters" %}
* **`callback`**: Callback method which is called when the market changes
* **`a`**: symbol name or ID of the first asset
* **`b`**: symbol name or ID of the second asset
{% endtab %}
{% endtabs %}

### unsubscribe\_from\_market

Unsubscribe from updates to a given market.

```cpp
void graphene::app::database_api::unsubscribe_from_market(
    const std::string &a, 
    const std::string &b)

```

{% tabs %}
{% tab title="Parameters" %}
* **`a`**: symbol name or ID of the first asset
* **`b`**: symbol name or ID of the second asset
{% endtab %}
{% endtabs %}

### get\_ticker

Returns the ticker for the market assetA:assetB.

```cpp
market_ticker graphene::app::database_api::get_ticker(
    const string &base, 
    const string &quote)const

```

{% tabs %}
{% tab title="Parameters" %}
* **`base`**: symbol name or ID of the base asset
* **`quote`**: symbol name or ID of the quote asset
{% endtab %}

{% tab title="Return" %}
The market ticker for the past 24 hours.
{% endtab %}
{% endtabs %}

### get\_24\_volume

Returns the 24 hour volume for the market assetA:assetB.

```cpp
market_volume graphene::app::database_api::get_24_volume(
    const string &base, 
    const string &quote)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`base`**: symbol name or ID of the base asset
* **`quote`**: symbol name or ID of the quote asset
{% endtab %}

{% tab title="Return" %}
The market volume over the past 24 hours.
{% endtab %}
{% endtabs %}

### get\_trade\_history

Returns recent trades for the market base:quote, ordered by time, most recent first. 

{% hint style="warning" %}
**Note**: Currently, timezone offsets are not supported. The time must be UTC.
{% endhint %}

The range is \[stop, start\). In case  there are more than 100 trades occurring in the same second, this API only returns the first 100 records; use [get\_trade\_history\_by\_sequence](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a19c22f540701825c9292e4a790a4b0d3) to query for the rest.

```cpp
vector<market_trade> graphene::app::database_api::get_trade_history(
    const string &base, 
    const string &quote, 
    fc::time_point_sec start, 
    fc::time_point_sec stop, 
    unsigned limit = 100)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`base`**: symbol or ID of the base asset
* **`quote`**: symbol or ID of the quote asset
* **`start`**: Start time as a UNIX timestamp, the latest trade to retrieve
* **`stop`**: Stop time as a UNIX timestamp, the earliest trade to retrieve
* **`limit`**: Number of transactions to retrieve, capped at 100.
{% endtab %}

{% tab title="Return" %}
Recent transactions in the market
{% endtab %}
{% endtabs %}

## Witnesses

### get\_witnesses

Get a list of witnesses by ID.

Semantically equivalent to [get\_objects](database-api.md#get_objects), but doesn’t subscribe.

```cpp
vector<optional<witness_object>> graphene::app::database_api::get_witnesses(
    const vector<witness_id_type> &witness_ids)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`witness_ids`**: IDs of the witnesses to retrieve
{% endtab %}

{% tab title="Return" %}
The witnesses corresponding to the provided IDs.
{% endtab %}
{% endtabs %}

### get\_witness\_by\_account

Get the witness owned by a given account.

```cpp
fc::optional<witness_object> graphene::app::database_api::get_witness_by_account(
    const std::string account_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_name_or_id`**: The name or ID of the account whose witness should be retrieved
{% endtab %}

{% tab title="Return" %}
The witness object, or null if the account does not have a witness.
{% endtab %}
{% endtabs %}

### **lookup\_witness\_accounts**

Get names and IDs for registered witnesses.

```cpp
map<string, witness_id_type> graphene::app::database_api::lookup_witness_accounts(
    const string &lower_bound_name, uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`lower_bound_name`**: Lower bound of the first name to return
* **`limit`**: Maximum number of results to return must not exceed 1000
{% endtab %}

{% tab title="Return" %}
Map of witness names to corresponding IDs**.**
{% endtab %}
{% endtabs %}

### get\_witness\_count

Get the total number of witnesses registered with the blockchain.

```cpp
uint64_t graphene::app::database_api::get_witness_count()const
```

## Committee members

### get\_committee\_members

vector&lt;optional&lt;committee\_member\_object&gt;&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_committee_members`\(_const_ vector&lt;committee\_member\_id\_type&gt; &_committee\_member\_ids_\)_const_  


Get a list of committee\_members by ID.

This function has semantics identical to[get\_objects](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a1f20e51d290fc3ac2409c49c058585b3), but doesn’t subscribe**Return**

The committee\_members corresponding to the provided IDs**Parameters**

* `committee_member_ids`: IDs of the committee\_members to retrieve

#### [get\_committee\_member\_by\_account](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id57)

fc::optional&lt;committee\_member\_object&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_committee_member_by_account`\(_const_ string _account\_name\_or\_id_\)_const_  


Get the committee\_member owned by a given account.

**Return**

The committee\_member object, or null if the account does not have a committee\_member**Parameters**

* `account_name_or_id`: The name or ID of the account whose committee\_member should be retrieved

#### [lookup\_committee\_member\_accounts](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id58)

map&lt;string, committee\_member\_id\_type&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::lookup_committee_member_accounts`\(_const_ string &_lower\_bound\_name_, uint32\_t _limit_\)_const_  


Get names and IDs for registered committee\_members.

**Return**

Map of committee\_member names to corresponding IDs**Parameters**

* `lower_bound_name`: Lower bound of the first name to return
* `limit`: Maximum number of results to return must not exceed 1000

### [Workers](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id59)

#### [get\_workers\_by\_account](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id60)

vector&lt;optional&lt;worker\_object&gt;&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_workers_by_account`\(_const_ std::string _account\_name\_or\_id_\)_const_  


Get the workers owned by a given account.

**Return**

A list of worker objects owned by the account**Parameters**

* `account_name_or_id`: The name or ID of the account whose worker should be retrieved

### [Votes](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id61)

#### [lookup\_vote\_ids](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id62)

vector&lt;variant&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::lookup_vote_ids`\(_const_ vector&lt;vote\_id\_type&gt; &_votes_\)_const_  


Given a set of votes, return the objects they are voting for.

This will be a mixture of committee\_member\_objects, witness\_objects, and worker\_objects**Return**

the referenced objects**Parameters**

* `votes`: a list of vote IDs

The results will be in the same order as the votes. Null will be returned for any vote IDs that are not found.  


### [Authority / Validation](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id63)

#### [get\_transaction\_hex](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id64)

std::string `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_transaction_hex`\(_const_ signed\_transaction &_trx_\)_const_  


Get a hexdump of the serialized binary form of a transaction.

**Return**

the hexdump of the transaction**Parameters**

* `trx`: a transaction to get hexdump from

#### [get\_required\_signatures](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id65)

set&lt;public\_key\_type&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_required_signatures`\(_const_ signed\_transaction &_trx_, _const_ flat\_set&lt;public\_key\_type&gt; &_available\_keys_\)_const_  


This API will take a partially signed transaction and a set of public keys that the owner has the ability to sign for and return the minimal subset of public keys that should add signatures to the transaction.

**Return**

a subset of `available_keys` that could sign for the given transaction**Parameters**

* `trx`: the transaction to be signed
* `available_keys`: a set of public keys

#### [get\_potential\_signatures](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id66)

set&lt;public\_key\_type&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_potential_signatures`\(_const_ signed\_transaction &_trx_\)_const_  


This method will return the set of all public keys that could possibly sign for a given transaction. This call can be used by wallets to filter their set of public keys to just the relevant subset prior to calling [get\_required\_signatures](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a9ae2eb6a83c27a7b4eec2b00ee8ba371) to get the minimum subset.

**Return**

a set of public keys that could possibly sign for the given transaction**Parameters**

* `trx`: the transaction to be signed

#### [get\_potential\_address\_signatures](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id67)

set&lt;address&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_potential_address_signatures`\(_const_ signed\_transaction &_trx_\)_const_  


This method will return the set of all addresses that could possibly sign for a given transaction.

**Return**

a set of addresses that could possibly sign for the given transaction**Parameters**

* `trx`: the transaction to be signed

#### [verify\_authority](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id68)

bool `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::verify_authority`\(_const_ signed\_transaction &_trx_\)_const_  


Check whether a transaction has all of the required signatures**Return**

true if the `trx` has all of the required signatures, otherwise throws an exception**Parameters**

* `trx`: a transaction to be verified

#### [verify\_account\_authority](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id69)

bool `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::verify_account_authority`\(_const_ string &_account\_name\_or\_id_, _const_ flat\_set&lt;public\_key\_type&gt; &_signers_\)_const_  


Verify that the public keys have enough authority to approve an operation for this account.

**Return**

true if the passed in keys have enough authority to approve an operation for this account**Parameters**

* `account_name_or_id`: name or ID of an account to check
* `signers`: the public keys

#### [validate\_transaction](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id70)

processed\_transaction `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::validate_transaction`\(_const_ signed\_transaction &_trx_\)_const_  


Validates a transaction against the current state without broadcasting it on the network.

**Return**

a processed\_transaction object if the transaction passes the validation, otherwise an exception will be thrown**Parameters**

* `trx`: a transaction to be validated

#### [get\_required\_fees](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id71)

vector&lt;fc::variant&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_required_fees`\(_const_ vector&lt;operation&gt; &_ops_, _const_ std::string &_asset\_symbol\_or\_id_\)_const_  


For each operation calculate the required fee in the specified asset type.

**Return**

a list of objects which indicates required fees of each operation**Parameters**

* `ops`: a list of operations to be query for required fees
* `asset_symbol_or_id`: symbol name or ID of an asset that to be used to pay the fees

### [Proposed Transactions](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id72)

#### [get\_proposed\_transactions](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id73)

vector&lt;proposal\_object&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_proposed_transactions`\(_const_ std::string _account\_name\_or\_id_\)_const_  


return a set of proposed transactions \(aka proposals\) that the specified account can add approval to or remove approval from

**Return**

a set of proposed transactions that the specified account can act on**Parameters**

* `account_name_or_id`: The name or ID of an account

### [Blinded balances](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id74)

#### [get\_blinded\_balances](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id75)

vector&lt;blinded\_balance\_object&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_blinded_balances`\(_const_ flat\_set&lt;commitment\_type&gt; &_commitments_\)_const_  


return the set of blinded balance objects by commitment ID

**Return**

the set of blinded balance objects by commitment ID**Parameters**

* `commitments`: a set of commitments to query for

  
[Next ](https://dev.bitshares.works/en/master/api/blockchain_api/history.html)[ Previous](https://dev.bitshares.works/en/master/api/blockchain_api.html)  


