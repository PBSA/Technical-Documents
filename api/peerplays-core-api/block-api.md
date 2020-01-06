# Block API

## Block

### get\_blocks

Get signed blocks.

```cpp
vector<optional<signed_block>> 
graphene::app::block_api::get_blocks(
    uint32_t block_num_from, 
    uint32_t block_num_to)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`block_num_from`**: The lowest block number
* **`block_num_to`**: The highest block number
{% endtab %}

{% tab title="Return" %}
A list of signed blocks from `block_num_from` to `block_num_to`
{% endtab %}
{% endtabs %}

