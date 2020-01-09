# Orders API

## Orders

### get\_grouped\_limit\_orders

Get grouped limit orders in given market.

```cpp
vector<limit_order_group> graphene::app::orders_api::get_grouped_limit_orders(
    std::string base_asset, 
    std::string quote_asset, 
    uint16_t group, 
    optional<price> start, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`base_asset`**: ID or symbol of asset being sold
* **`quote_asset`**: ID or symbol of asset being purchased
* **`group`**: Maximum price diff within each order group, have to be one of configured values
* **`start`**: Optional price to indicate the first order group to retrieve
* **`limit`**: Maximum number of order groups to retrieve \(must not exceed 101\)
{% endtab %}

{% tab title="Return" %}
The grouped limit orders, ordered from best offered price to the worst.
{% endtab %}
{% endtabs %}

