# Transaction Builder

## Transaction Builder

### begin\_builder\_transaction

Create a new transaction builder.

```cpp
transaction_handle_typegraphene::wallet::wallet_api::begin_builder_transaction()
```

{% tabs %}
{% tab title="Return" %}
Handle of the new transaction builder.
{% endtab %}
{% endtabs %}

### add\_operation\_to\_builder\_transaction

Append a new operation to a transaction builder.

```cpp
void graphene::wallet::wallet_api::add_operation_to_builder_transaction(
    transaction_handle_typetransaction_handle, 
    const operation &op)
```

{% tabs %}
{% tab title="Parameters" %}
* **`transaction_handle`**: handle of the transaction builder
* **`op`**: the operation in JSON format
{% endtab %}
{% endtabs %}

### replace\_operation\_in\_builder\_transaction

Replace an operation in a transaction builder with a new operation.

```cpp
void graphene::wallet::wallet_api::replace_operation_in_builder_transaction(
    transaction_handle_typehandle, 
    unsigned operation_index, 
    const operation &new_op)
```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
* **`operation_index`**: the index of the old operation in the builder to be replaced
* **`new_op`**: the new operation in JSON format
{% endtab %}
{% endtabs %}

### set\_fees\_on\_builder\_transaction

Calculate and update fees for the operations in a transaction builder.

```cpp
asset graphene::wallet::wallet_api::set_fees_on_builder_transaction(
    transaction_handle_typehandle, 
    string fee_asset = GRAPHENE_SYMBOL)
```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
* **`fee_asset`**: name or ID of an asset that to be used to pay fees
{% endtab %}

{% tab title="Return" %}
Total fees.
{% endtab %}
{% endtabs %}

### preview\_builder\_transaction

Show content of a transaction builder.

```cpp
transaction graphene::wallet::wallet_api::preview_builder_transaction(
    transaction_handle_typehandle)
```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
{% endtab %}

{% tab title="Return" %}
A transaction.
{% endtab %}
{% endtabs %}

### **sign\_builder\_transaction**

Sign the transaction in a transaction builder and optionally broadcast to the network.

```cpp
signed_transaction graphene::wallet::wallet_api::sign_builder_transaction(
    transaction_handle_typetransaction_handle, 
    bool broadcast = true)
```

{% tabs %}
{% tab title="Parameters" %}
* **`transaction_handle`**: handle of the transaction builder
* **`broadcast`**: whether to broadcast the signed transaction to the network
{% endtab %}

{% tab title="Return" %}
A signed transaction.
{% endtab %}
{% endtabs %}

### propose\_builder\_transaction

Create a proposal containing the operations in a transaction builder \(create a new proposal\_create operation, then replace the transaction builder with the new operation\), then sign the transaction and optionally broadcast to the network.

{% hint style="warning" %}
**Note**: this command is not effective because you're unable to specify a proposer. It will be deprecated in a future release. Use [`propose_builder_transaction2()`](transaction-builder.md#propose_builder_transaction2) instead.
{% endhint %}

```cpp
signed_transaction graphene::wallet::wallet_api::propose_builder_transaction(
    transaction_handle_typehandle, 
    time_point_sec expiration = time_point::now() + fc::minutes(1), 
    uint32_t review_period_seconds = 0, 
    bool broadcast = true)

```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
* **`expiration`**: when the proposal will expire
* **`review_period_seconds`**: review period of the proposal in seconds
* **`broadcast`**: whether to broadcast the signed transaction to the network
{% endtab %}

{% tab title="Return" %}
A signed transaction.
{% endtab %}
{% endtabs %}

### propose\_builder\_transaction2

Create a proposal containing the operations in a transaction builder \(create a new proposal\_create operation, then replace the transaction builder with the new operation\), then sign the transaction and optionally broadcast to the network.

```cpp
signed_transaction graphene::wallet::wallet_api::propose_builder_transaction2(
    transaction_handle_typehandle, 
    string account_name_or_id, 
    time_point_sec expiration = time_point::now() + fc::minutes(1), 
    uint32_t review_period_seconds = 0, 
    bool broadcast = true)
```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
* **`account_name_or_id`**: name or ID of the account who would pay fees for creating the proposal
* **`expiration`**: when the proposal will expire
* **`review_period_seconds`**: review period of the proposal in seconds
* **`broadcast`**: whether to broadcast the signed transaction to the network
{% endtab %}

{% tab title="Return" %}
A signed transaction.
{% endtab %}
{% endtabs %}

### remove\_builder\_transaction

Destroy a transaction builder.

```cpp
void graphene::wallet::wallet_api::remove_builder_transaction(
    transaction_handle_typehandle)
```

{% tabs %}
{% tab title="Parameters" %}
* **`handle`**: handle of the transaction builder
{% endtab %}
{% endtabs %}

### serialize\_transaction

Converts a signed\_transaction in JSON form to its binary representation.

```cpp
string graphene::wallet::wallet_api::serialize_transaction(
    signed_transaction tx)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`tx`**: the transaction to serialize
{% endtab %}

{% tab title="Return" %}
The binary form of the transaction. It will not be hex encoded, this returns a raw string that may have null characters embedded in it
{% endtab %}
{% endtabs %}

### **sign\_transaction**

Signs a transaction.

Given a fully-formed transaction that is only lacking signatures, this signs the transaction with the necessary keys and optionally broadcasts the transaction**.**

```cpp
signed_transaction graphene::wallet::wallet_api::sign_transaction(
    signed_transaction tx, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`tx`**: the unsigned transaction
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed version of the transaction
{% endtab %}
{% endtabs %}

### get\_prototype\_operation

Returns an uninitialized object representing a given blockchain operation.

This returns a default-initialized object of the given type; it can be used during early development of the wallet when we don’t yet have custom commands for creating all of the operations the blockchain supports.

Any operation the blockchain supports can be created using the transaction builder’s [`add_operation_to_builder_transaction()`](transaction-builder.md#add_operation_to_builder_transaction) , but to do that from the CLI you need to know what the JSON form of the operation looks like. This will give you a template you can fill in. It’s better than nothing.

```cpp
operation graphene::wallet::wallet_api::get_prototype_operation(
    string operation_type)
```

{% tabs %}
{% tab title="Parameters" %}
* **`operation_type`**: the type of operation to return, must be one of the operations defined in `graphene/protocol/operations.hpp` \(e.g., “global\_parameters\_update\_operation”\)
{% endtab %}

{% tab title="Return" %}
A default-constructed operation of the given type.
{% endtab %}
{% endtabs %}

