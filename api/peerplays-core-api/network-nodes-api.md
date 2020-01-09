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

### [Change Network Settings](https://dev.bitshares.works/en/master/api/blockchain_api/network_node.html#id6)

#### [add\_node](https://dev.bitshares.works/en/master/api/blockchain_api/network_node.html#id7)

void `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`network_node_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app16network_node_apiE)`::add_node`\(_const_ fc::ip::endpoint &_ep_\)  


add\_node Connect to a new peer

**Parameters**

* `ep`: The IP/Port of the peer to connect to

#### [set\_advanced\_node\_parameters](https://dev.bitshares.works/en/master/api/blockchain_api/network_node.html#id8)

void `graphene::`[`app`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3appE)`::`[`network_node_api`](https://dev.bitshares.works/en/master/api/namespaces/app.html#_CPPv4N8graphene3app16network_node_apiE)`::set_advanced_node_parameters`\(_const_ fc::variant\_object &_params_\)  


Set advanced node parameters, such as desired and max number of connections.

**Parameters**

* `params`: a JSON object containing the name/value pairs for the parameters to set

