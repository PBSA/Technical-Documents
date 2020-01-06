# Database API

The database API is available from the full node via websockets.

## Objects

### get\_objects

Get the objects corresponding to the provided IDs.

If any of the provided IDs does not map to an object, a null variant is returned in its position.

```cpp
fc::variants 
graphene::app::database_api::get_objects(
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

