# Asset API

## Asset

### get\_asset\_holders

Get asset holders for a specific asset.

```cpp
vector<account_asset_balance> 
graphene::app::asset_api::get_asset_holders(
    std::string asset, 
    uint32_t start, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`asset`**: The specific asset id or symbol
* **`start`**: The start index
* **`limit`**: Maximum limit must not exceed 100
{% endtab %}

{% tab title="Return" %}
A list of asset holders for the specified asset.
{% endtab %}
{% endtabs %}

### get\_all\_asset\_holders

Get all asset holders.

```cpp
vector<asset_holders> 
graphene::app::asset_api::get_all_asset_holders()const
```

{% tabs %}
{% tab title="Return" %}
A list of all asset holders.
{% endtab %}
{% endtabs %}

