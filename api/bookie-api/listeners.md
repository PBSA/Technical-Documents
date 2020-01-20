# Listeners

### **subscribe\_to\_market**

Subscribe a listener to a betting market.

```javascript
Apis.instance().db_api().exec( "subscribe_to_market", [updateListener, "1.3.0", "1.3.19"])
```

{% tabs %}
{% tab title="Parameters" %}
* `updateListener`: An object of type updateListener.
* `1.3.0:` Start version.
* `1.3.19:` End version.
{% endtab %}
{% endtabs %}

### **unsubscribe\_from\_market**

Unsubscribe a listener from a betting market.

```javascript
Apis.instance().db_api().exec( "unsubscribe_from_market", [updateListener, "1.3.0", "1.3.19"])
```

{% tabs %}
{% tab title="Parameters" %}
* `updateListener`: An object of type updateListener.
* `1.3.0:` Start version.
* `1.3.19:` End version.
{% endtab %}
{% endtabs %}

