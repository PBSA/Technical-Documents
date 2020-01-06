# Account History API

The history API is available from the full node via websockets.

## Account History

### get\_account\_history

Get operations relevant to the specified account.

```cpp
vector<operation_history_object> 
graphene::app::history_api::get_account_history(
    const std::string account_id_or_name, 
    operation_history_id_type stop = operation_history_id_type(), 
    unsigned limit = 100, 
    operation_history_id_type start = operation_history_id_type())const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_id_or_name`**: The account ID or name whose history should be queried
* **`stop`**: ID of the earliest operation to retrieve
* **`limit`**: Maximum number of operations to retrieve \(must not exceed 100\)
* **`start`**: ID of the most recent operation to retrieve
{% endtab %}

{% tab title="Return" %}
A list of operations performed by account, ordered from most recent to oldest.
{% endtab %}
{% endtabs %}

### get\_account\_history\_operations

Get only asked operations relevant to the specified account.

```cpp
vector<operation_history_object> 
graphene::app::history_api::get_account_history_operations(
    const std::string account_id_or_name, 
    int operation_type, 
    operation_history_id_type start = operation_history_id_type(), 
    operation_history_id_type stop = operation_history_id_type(), 
    unsigned limit = 100)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_id_or_name`**: The account ID or name whose history should be queried
* **`operation_type`**: The type of the operation we want to get operations in the account \( 0 = transfer , 1 = limit order create, …\)
* **`stop`**: ID of the earliest operation to retrieve
* **`limit`**: Maximum number of operations to retrieve \(must not exceed 100\)
* **`start`**: ID of the most recent operation to retrieve
{% endtab %}

{% tab title="Return" %}
A list of operations performed by account, ordered from most recent to oldest.
{% endtab %}
{% endtabs %}

### get\_relative\_account\_history

Get operations relevant to the specified account referenced by an event numbering specific to the account. The current number of operations for the account can be found in the account statistics \(or use 0 for start\).

```cpp
vector<operation_history_object> 
graphene::app::history_api::get_relative_account_history(
    const std::string account_id_or_name, 
    uint64_t stop = 0, 
    unsigned limit = 100, 
    uint64_t start = 0)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_id_or_name`**: The account ID or name whose history should be queried
* **`stop`**: Sequence number of earliest operation. 0 is default and will query ‘limit’ number of operations.
* **`limit`**: Maximum number of operations to retrieve \(must not exceed 100\)
* **`start`**: Sequence number of the most recent operation to retrieve. 0 is default, which will start querying from the most recent operation.
{% endtab %}

{% tab title="Return" %}
A list of operations performed by account, ordered from most recent to oldest.
{% endtab %}
{% endtabs %}

## Market History



