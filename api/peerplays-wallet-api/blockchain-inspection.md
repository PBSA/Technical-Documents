# Blockchain Inspection

## Blockchain Inspection

### get\_block

Returns info about a specified block.

```cpp
optional<signed_block_with_info> graphene::wallet::wallet_api::get_block(
    uint32_t num)
```

{% tabs %}
{% tab title="Parameters" %}
* **`num`**: height of the block to retrieve
{% endtab %}

{% tab title="Return" %}
Info about the block, or null if not found.
{% endtab %}
{% endtabs %}

### **get\_account\_count**

Returns the number of accounts registered on the blockchain**.**

```cpp
uint64_t graphene::wallet::wallet_api::get_account_count()const
```

{% tabs %}
{% tab title="Return" %}
The number of registered accounts
{% endtab %}
{% endtabs %}

### get\_global\_properties

Returns the block chain’s slowly-changing settings. 

This object contains all of the properties of the blockchain that are fixed or that change only once per maintenance interval \(daily\) such as the current list of witnesses, committee\_members, block interval, etc.

**See** [`get_dynamic_global_properties()`](blockchain-inspection.md#get_dynamic_global_properties) for frequently changing properties.

```cpp
global_property_object graphene::wallet::wallet_api::get_global_properties()const
```

{% tabs %}
{% tab title="Return" %}
The global properties.
{% endtab %}
{% endtabs %}

### get\_dynamic\_global\_properties

Returns the block chain’s rapidly-changing properties. The returned object contains information that changes every block interval such as the head block number, the next witness, etc.**See**

\`\`[`get_global_properties()`](blockchain-inspection.md#get_global_properties) for less-frequently changing properties

```cpp
dynamic_global_property_object graphene::wallet::wallet_api::get_dynamic_global_properties()const
```

{% tabs %}
{% tab title="Return" %}
The dynamic global properties.
{% endtab %}
{% endtabs %}

#### [get\_object](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id98)

variant `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_object`\(object\_id\_type _id_\)_const_  


Returns the blockchain object corresponding to the given id.

This generic function can be used to retrieve any object from the blockchain that is assigned an ID. Certain types of objects have specialized convenience functions to return their objects e.g., assets have [`get_asset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1aae54080626cf4e4b24572f4836e8dfdd), accounts have [`get_account()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1ae4133a2fe8f63695385c20d327a88ff9), but this function will work for any object.

**Return**

the requested object**Parameters**

* `id`: the id of the object to return

