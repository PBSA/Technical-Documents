# General Calls

## General Calls

### help

Returns a list of all commands supported by the wallet API.

This lists each command, along with its arguments and return types. For more detailed help on a single command, use [`gethelp()`](general-calls.md#gethelp)``

```cpp
string graphene::wallet::wallet_api::help()const
```

{% tabs %}
{% tab title="Return" %}
A multi-line string suitable for displaying on a terminal.
{% endtab %}
{% endtabs %}

### gethelp

Returns detailed help on a single API command.

```cpp
string graphene::wallet::wallet_api::gethelp(
    const string &method)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`method`**: the name of the API command you want help with
{% endtab %}

{% tab title="Return" %}
A multi-line string suitable for displaying on a terminal.
{% endtab %}
{% endtabs %}

### info

Returns info about head block, chain\_id, maintenance, participation, current active witnesses and committee members.

```cpp
variant graphene::wallet::wallet_api::info()
```

{% tabs %}
{% tab title="Return" %}
Runtime info about the blockchain
{% endtab %}
{% endtabs %}

### about

Returns info such as client version, git version of graphene/fc, version of boost, openssl etc.

```cpp
variant_object graphene::
wallet
::
wallet_api
::about()const
```

{% tabs %}
{% tab title="Return" %}
Compile time info and client and dependencies versions.
{% endtab %}
{% endtabs %}

#### network\_add\_nodes

```cpp
void graphene::wallet::wallet_api::network_add_nodes(
    const vector<string> &nodes)
```

{% tabs %}
{% tab title="Parameters" %}
**`nodes`**: Nodes to be added.\

{% endtab %}
{% endtabs %}

### network\_get\_connected\_peers

```cpp
vector<variant> graphene::wallet::wallet_api::network_get_connected_peers()
```

{% tabs %}
{% tab title="Return" %}
List of connected peers.
{% endtab %}
{% endtabs %}
