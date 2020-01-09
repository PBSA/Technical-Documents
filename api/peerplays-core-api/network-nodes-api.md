# Network Nodes API

The network node API is available from the full node via web-sockets.

## Obtain Network Information

### get\_info

Return general network information, such as p2p port.

```cpp
fc::variant_object graphene::app::network_node_api:get_info()const
```

### get\_connected\_peers

Get status of all current connections to peers.

```cpp
std::vector<net::peer_status> graphene::app::network_node_api::get_connected_peers()const
```

### get\_potential\_peers

Return list of potential peers.

```cpp
std::vector<net::potential_peer_record> graphene::app::network_node_api::get_potential_peers()const
```

### get\_advanced\_node\_parameters

Get advanced node parameters, such as desired and max number of connections.

```cpp
fc::variant_object graphene::app::network_node_api::get_advanced_node_parameters()const
```

## Change Network Settings

### add\_node

Connect to a new peer

```cpp
void graphene::app::network_node_api::add_node(
    const fc::ip::endpoint &ep)
```

{% tabs %}
{% tab title="Parameters" %}
* **`ep`**: The IP/Port of the peer to connect to
{% endtab %}
{% endtabs %}

### **set\_advanced\_node\_parameters**

Set advanced node parameters, such as desired and max number of connections.

```cpp
void graphene::app::network_node_api::set_advanced_node_parameters(
    const fc::variant_object &params)
```

{% tabs %}
{% tab title="Parameters" %}
* **`params`**: a JSON object containing the name/value pairs for the parameters to set
{% endtab %}
{% endtabs %}

