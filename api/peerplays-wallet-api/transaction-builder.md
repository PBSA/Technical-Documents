# Transaction Builder

### [Transaction Builder](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id99)

#### [begin\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id100)

[transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)`graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::begin_builder_transaction`\(\)  


Create a new transaction builder.**Return**

handle of the new transaction builder

#### [add\_operation\_to\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id101)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::add_operation_to_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_transaction\_handle_, _const_ operation &_op_\)  


Append a new operation to a transaction builder.**Parameters**

* `transaction_handle`: handle of the transaction builder
* `op`: the operation in JSON format

#### [replace\_operation\_in\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id102)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::replace_operation_in_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_, unsigned _operation\_index_, _const_ operation &_new\_op_\)  


Replace an operation in a transaction builder with a new operation.**Parameters**

* `handle`: handle of the transaction builder
* `operation_index`: the index of the old operation in the builder to be replaced
* `new_op`: the new operation in JSON format

#### [set\_fees\_on\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id103)

asset `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::set_fees_on_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_, string _fee\_asset_ = GRAPHENE\_SYMBOL\)  


Calculate and update fees for the operations in a transaction builder.**Return**

total fees**Parameters**

* `handle`: handle of the transaction builder
* `fee_asset`: name or ID of an asset that to be used to pay fees

#### [preview\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id104)

transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::preview_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_\)  


Show content of a transaction builder.**Return**

a transaction**Parameters**

* `handle`: handle of the transaction builder

#### [sign\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id105)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::sign_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_transaction\_handle_, bool _broadcast_ = true\)  


Sign the transaction in a transaction builder and optionally broadcast to the network.**Return**

a signed transaction**Parameters**

* `transaction_handle`: handle of the transaction builder
* `broadcast`: whether to broadcast the signed transaction to the network

#### [propose\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id106)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::propose_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_, time\_point\_sec _expiration_ = time\_point::now\(\) + fc::minutes\(1\), uint32\_t _review\_period\_seconds_ = 0, bool _broadcast_ = true\)  


Create a proposal containing the operations in a transaction builder \(create a new proposal\_create operation, then replace the transaction builder with the new operation\), then sign the transaction and optionally broadcast to the network.

Note: this command is buggy because unable to specify proposer. It will be deprecated in a future release. Please use [`propose_builder_transaction2()`](https://dev.bitshares.works/en/master/bts_guide/tutorials/propose-transaction.html#classgraphene_1_1wallet_1_1wallet__api_1ad33bc4056cefd13bca5d74f4cc0c017f) instead.

**Return**

a signed transaction**Parameters**

* `handle`: handle of the transaction builder
* `expiration`: when the proposal will expire
* `review_period_seconds`: review period of the proposal in seconds
* `broadcast`: whether to broadcast the signed transaction to the network

#### [propose\_builder\_transaction2](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id107)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::propose_builder_transaction2`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_, string _account\_name\_or\_id_, time\_point\_sec _expiration_ = time\_point::now\(\) + fc::minutes\(1\), uint32\_t _review\_period\_seconds_ = 0, bool _broadcast_ = true\)  


Create a proposal containing the operations in a transaction builder \(create a new proposal\_create operation, then replace the transaction builder with the new operation\), then sign the transaction and optionally broadcast to the network.

**Return**

a signed transaction**Parameters**

* `handle`: handle of the transaction builder
* `account_name_or_id`: name or ID of the account who would pay fees for creating the proposal
* `expiration`: when the proposal will expire
* `review_period_seconds`: review period of the proposal in seconds
* `broadcast`: whether to broadcast the signed transaction to the network

#### [remove\_builder\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id108)

void `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::remove_builder_transaction`\([transaction\_handle\_type](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet23transaction_handle_typeE)_handle_\)  


Destroy a transaction builder.**Parameters**

* `handle`: handle of the transaction builder

#### [serialize\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id109)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::serialize_transaction`\(signed\_transaction _tx_\)_const_  


Converts a signed\_transaction in JSON form to its binary representation.

**Return**

the binary form of the transaction. It will not be hex encoded, this returns a raw string that may have null characters embedded in it**Parameters**

* `tx`: the transaction to serialize

#### [sign\_transaction](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id110)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::sign_transaction`\(signed\_transaction _tx_, bool _broadcast_ = false\)  


Signs a transaction.

Given a fully-formed transaction that is only lacking signatures, this signs the transaction with the necessary keys and optionally broadcasts the transaction**Return**

the signed version of the transaction**Parameters**

* `tx`: the unsigned transaction
* `broadcast`: true if you wish to broadcast the transaction

#### [get\_prototype\_operation](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id111)

operation `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_prototype_operation`\(string _operation\_type_\)  


Returns an uninitialized object representing a given blockchain operation.

This returns a default-initialized object of the given type; it can be used during early development of the wallet when we don’t yet have custom commands for creating all of the operations the blockchain supports.

Any operation the blockchain supports can be created using the transaction builder’s [`add_operation_to_builder_transaction()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1ab5cd568be3fd1c283e0ed2c1fd3c5469) , but to do that from the CLI you need to know what the JSON form of the operation looks like. This will give you a template you can fill in. It’s better than nothing.

**Return**

a default-constructed operation of the given type**Parameters**

* `operation_type`: the type of operation to return, must be one of the operations defined in `graphene/protocol/operations.hpp` \(e.g., “global\_parameters\_update\_operation”\)

