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

Returns the block chain’s slowly-changing settings.&#x20;

This object contains all of the properties of the blockchain that are fixed or that change only once per maintenance interval (daily) such as the current list of witnesses, committee\_members, block interval, etc.

**See** [`get_dynamic_global_properties()`](blockchain-inspection.md#get\_dynamic\_global\_properties) for frequently changing properties.

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

``[`get_global_properties()`](blockchain-inspection.md#get\_global\_properties) for less-frequently changing properties

```cpp
dynamic_global_property_object graphene::wallet::wallet_api::get_dynamic_global_properties()const
```

{% tabs %}
{% tab title="Return" %}
The dynamic global properties.
{% endtab %}
{% endtabs %}

### get\_object

Returns the blockchain object corresponding to the given id.

This generic function can be used to retrieve any object from the blockchain that is assigned an ID. Certain types of objects have specialized convenience functions to return their objects e.g., assets have [`get_asset()`](asset-calls.md#get\_asset), accounts have [`get_account()`](account-calls.md#get\_account), but this function will work for any object.

```cpp
variant graphene::wallet::wallet_api::get_object(
    object_id_type id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`id`**: the id of the object to return.
{% endtab %}

{% tab title="Return" %}
The requested object.
{% endtab %}
{% endtabs %}
