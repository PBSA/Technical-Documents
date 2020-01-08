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

#### [get\_account\_by\_name](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id24)

optional&lt;account\_object&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_account_by_name`\(string _name_\)_const_  


Get info of an account by name.

**Return**

The account holding the provided name**Parameters**

* `name`: Name of the account to retrieve

#### [get\_account\_references](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id25)

vector&lt;account\_id\_type&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_account_references`\(_const_ std::string _account\_name\_or\_id_\)_const_  


Get all accounts that refer to the specified account in their owner or active authorities.

**Return**

all accounts that refer to the specified account in their owner or active authorities**Parameters**

* `account_name_or_id`: Account name or ID to query

#### [lookup\_account\_names](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id26)

vector&lt;optional&lt;account\_object&gt;&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::lookup_account_names`\(_const_ vector&lt;string&gt; &_account\_names_\)_const_  


Get a list of accounts by name.

This function has semantics identical to[get\_objects](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a1f20e51d290fc3ac2409c49c058585b3), but doesn’t subscribe.**Return**

The accounts holding the provided names**Parameters**

* `account_names`: Names of the accounts to retrieve

#### [lookup\_accounts](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id27)

map&lt;string, account\_id\_type&gt; `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::lookup_accounts`\(_const_ string &_lower\_bound\_name_, uint32\_t _limit_, optional&lt;bool&gt; _subscribe_ = optional&lt;bool&gt;\(\)\)_const_  


Get names and IDs for registered accounts.

**Return**

Map of account names to corresponding IDs**Note**

In addition to the common auto-subscription rules, this API will subscribe to the returned account only if `limit` is 1.**Parameters**

* `lower_bound_name`: Lower bound of the first name to return
* `limit`: Maximum number of results to return must not exceed 1000
* `subscribe`: _true_ to subscribe to the queried account objects; _false_ to not subscribe; _null_ to subscribe or not subscribe according to current auto-subscription setting \(see [set\_auto\_subscription](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1database__api_1a7ef2faf3e3e402ea9572067554c2dd2c)\)

#### [get\_account\_count](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id28)

uint64\_t `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`database_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app12database_apiE)`::get_account_count`\(\)_const_  


Get the total number of accounts registered with the blockchain.  


### [Balances](https://dev.bitshares.works/en/master/api/blockchain_api/database.html#id29)



