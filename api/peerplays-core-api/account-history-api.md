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

### get\_fill\_order\_history

Get details of order executions occurred most recently in a trading pair.

```cpp
vector<order_history_object> 
graphene::app::history_api::get_fill_order_history(
    std::string a, 
    std::string b, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`a`**: Asset symbol or ID in a trading pair
* **`b`**: The other asset symbol or ID in the trading pair
* **`limit`**: Maximum records to return
{% endtab %}

{% tab title="Return" %}
a list of order\_history objects, in most recent first order
{% endtab %}
{% endtabs %}

### get\_market\_history

Get OHLCV data of a trading pair in a time range.

```cpp
vector<bucket_object> graphene::app::history_api::get_market_history(std::string a, std::string b, uint32_t bucket_seconds, fc::time_point_sec start, fc::time_point_sec end)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`a`**: Asset symbol or ID in a trading pair
* **`b`**: The other asset symbol or ID in the trading pair
* **`bucket_seconds`**: Length of each time bucket in seconds. 

{% hint style="warning" %}
**Note**: It needs to be within result of [get\_market\_history\_buckets\(\)](https://dev.bitshares.works/en/master/api/namespaces/app.html#classgraphene_1_1app_1_1history__api_1a3cb82a7bb879b9f967665d69fd90c67d) API, otherwise no data will be returned
{% endhint %}

* **`start`**: The start of a time range, E.G. “2018-01-01T00:00:00”
* **`end`**: The end of the time range
{% endtab %}

{% tab title="Return" %}
A list of OHLCV data, in least recent first order. 

If there are more than 200 records in the specified time range, the first 200 records will be returned.
{% endtab %}
{% endtabs %}



