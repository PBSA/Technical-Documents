# General Calls

### [General Calls](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id2)

#### [help](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id3)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::help`\(\)_const_  


Returns a list of all commands supported by the wallet API.

This lists each command, along with its arguments and return types. For more detailed help on a single command, use [`gethelp()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a21d6b9297891d2a7317bfc3be9e6a917)

**Return**

a multi-line string suitable for displaying on a terminal

#### [gethelp](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id4)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::gethelp`\(_const_ string &_method_\)_const_  


Returns detailed help on a single API command.**Return**

a multi-line string suitable for displaying on a terminal**Parameters**

* `method`: the name of the API command you want help with

#### [info](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id5)

variant `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::info`\(\)  


Returns info about head block, chain\_id, maintenance, participation, current active witnesses and committee members.**Return**

runtime info about the blockchain

#### [about](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id6)

variant\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::about`\(\)_const_  


Returns info such as client version, git version of graphene/fc, version of boost, openssl.**Return**

compile time info and client and dependencies versions

#### [network\_add\_nodes](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id7)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::network_add_nodes`\(_const_ vector&lt;string&gt; &_nodes_\)  


#### [network\_get\_connected\_peers](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id8)

vector&lt;variant&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::network_get_connected_peers`\(\)

