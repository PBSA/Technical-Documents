# Blockchain Inspection

### [Blockchain Inspection](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id93)

#### [get\_block](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id94)

optional&lt;signed\_block\_with\_info&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_block`\(uint32\_t _num_\)  


Returns info about a specified block.**Return**

info about the block, or null if not found**Parameters**

* `num`: height of the block to retrieve

#### [get\_account\_count](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id95)

uint64\_t `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_account_count`\(\)_const_  


Returns the number of accounts registered on the blockchain**Return**

the number of registered accounts

#### [get\_global\_properties](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id96)

global\_property\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_global_properties`\(\)_const_  


Returns the block chain’s slowly-changing settings. This object contains all of the properties of the blockchain that are fixed or that change only once per maintenance interval \(daily\) such as the current list of witnesses, committee\_members, block interval, etc.**See**

[`get_dynamic_global_properties()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1aaa8f1ab2e2e5fe7a414ada5375e14566) for frequently changing properties**Return**

the global properties

#### [get\_dynamic\_global\_properties](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id97)

dynamic\_global\_property\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_dynamic_global_properties`\(\)_const_  


Returns the block chain’s rapidly-changing properties. The returned object contains information that changes every block interval such as the head block number, the next witness, etc.**See**

[`get_global_properties()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a9f44d453c99ffc99bbd99caa9e065a95) for less-frequently changing properties**Return**

the dynamic global properties

#### [get\_object](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id98)

variant `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_object`\(object\_id\_type _id_\)_const_  


Returns the blockchain object corresponding to the given id.

This generic function can be used to retrieve any object from the blockchain that is assigned an ID. Certain types of objects have specialized convenience functions to return their objects e.g., assets have [`get_asset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1aae54080626cf4e4b24572f4836e8dfdd), accounts have [`get_account()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1ae4133a2fe8f63695385c20d327a88ff9), but this function will work for any object.

**Return**

the requested object**Parameters**

* `id`: the id of the object to return

