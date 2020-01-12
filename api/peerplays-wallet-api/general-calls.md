# General Calls

## General Calls

### help

Returns a list of all commands supported by the wallet API.

This lists each command, along with its arguments and return types. For more detailed help on a single command, use [`gethelp()`](general-calls.md#gethelp)\`\`

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

#### [about](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id6)

variant\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::about`\(\)_const_  


Returns info such as client version, git version of graphene/fc, version of boost, openssl.**Return**

compile time info and client and dependencies versions

#### [network\_add\_nodes](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id7)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::network_add_nodes`\(_const_ vector&lt;string&gt; &_nodes_\)  


#### [network\_get\_connected\_peers](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id8)

vector&lt;variant&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::network_get_connected_peers`\(\)

