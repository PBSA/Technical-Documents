# Network Broadcast API

The network broadcast API is available from the full node via web-sockets.

## Transactions

### broadcast\_transaction

Broadcast a transaction to the network.

The transaction will be checked for validity in the local database prior to broadcasting. If it fails to apply locally, an error will be thrown and the transaction will not be broadcast

```cpp
void graphene::app::network_broadcast_api::broadcast_transaction(
    const precomputable_transaction &trx)
```

{% tabs %}
{% tab title="Parameters" %}
* **`trx`**: The transaction to broadcast
{% endtab %}
{% endtabs %}

### broadcast\_transaction\_with\_callback

This version of broadcast transaction registers a callback method that will be called when the transaction is included into a block. The callback method includes the transaction id, block number, and transaction number in the block.

```cpp
void graphene::app::network_broadcast_api::broadcast_transaction_with_callback(
    confirmation_callback cb, 
    const precomputable_transaction &trx)
```

{% tabs %}
{% tab title="Parameters" %}
* **`cb`**: the callback method
* **`trx`**: the transaction
{% endtab %}
{% endtabs %}

## Block

### broadcast\_block

Broadcast a signed block to the network.

```cpp
void graphene::app::network_broadcast_api::broadcast_block(
    const signed_block &block)
```

{% tabs %}
{% tab title="Parameters" %}
* **`block`**: The signed block to broadcast.
{% endtab %}
{% endtabs %}

