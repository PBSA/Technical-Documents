# Privacy Mode

### [Privacy Mode](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id80)

#### [set\_key\_label](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id81)

bool `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::set_key_label`\(public\_key\_type _key_, string _label_\)  


These methods are used for stealth transfers This method can be used to set a label for a public key

**Note**

No two keys can have the same label.**Return**

true if the label was set, otherwise false**Parameters**

* `key`: a public key
* `label`: a user-defined string as label

#### [get\_key\_label](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id82)

string `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_key_label`\(public\_key\_type _key_\)_const_  


Get label of a public key.**Return**

the label if already set by [`set_key_label()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a33146d388f60a77c23ee4d85f949859e), or an empty string if not set**Parameters**

* `key`: a public key

#### [get\_public\_key](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id83)

public\_key\_type `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_public_key`\(string _label_\)_const_  


Get the public key associated with a given label.**Return**

the public key associated with the given label**Parameters**

* `label`: a label

#### [get\_blind\_accounts](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id84)

map&lt;string, public\_key\_type&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_blind_accounts`\(\)_const_  


Get all blind accounts.**Return**

all blind accounts

#### [get\_my\_blind\_accounts](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id85)

map&lt;string, public\_key\_type&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_my_blind_accounts`\(\)_const_  


Get all blind accounts for which this wallet has the private key.**Return**

all blind accounts for which this wallet has the private key

#### [get\_blind\_balances](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id86)

vector&lt;asset&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_blind_balances`\(string _key\_or\_label_\)  


Return the total balances of all blinded commitments that can be claimed by the given account key or label.**Return**

the total balances of all blinded commitments that can be claimed by the given account key or label**Parameters**

* `key_or_label`: a public key in Base58 format or a label

#### [create\_blind\_account](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id87)

public\_key\_type `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::create_blind_account`\(string _label_, string _brain\_key_\)  


Generates a new blind account for the given brain key and assigns it the given label.**Return**

the public key of the new account**Parameters**

* `label`: a label
* `brain_key`: the brain key to be used to generate a new blind account

#### [transfer\_to\_blind](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id88)

[blind\_confirmation](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet18blind_confirmationE)`graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::transfer_to_blind`\(string _from\_account\_id\_or\_name_, string _asset\_symbol_, vector&lt;pair&lt;string, string&gt;&gt; _to\_amounts_, bool _broadcast_ = false\)  


Transfers a public balance from `from_account_id_or_name` to one or more blinded balances using a stealth transfer.**Return**

a blind confirmation**Parameters**

* `from_account_id_or_name`: ID or name of an account to transfer from
* `asset_symbol`: symbol or ID of the asset to be transferred
* `to_amounts`: map from key or label to amount
* `broadcast`: true to broadcast the transaction on the network

#### [transfer\_from\_blind](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id89)

[blind\_confirmation](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet18blind_confirmationE)`graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::transfer_from_blind`\(string _from\_blind\_account\_key\_or\_label_, string _to\_account\_id\_or\_name_, string _amount_, string _asset\_symbol_, bool _broadcast_ = false\)  


Transfers funds from a set of blinded balances to a public account balance.**Return**

a blind confirmation**Parameters**

* `from_blind_account_key_or_label`: a public key in Base58 format or a label to transfer from
* `to_account_id_or_name`: ID or name of an account to transfer to
* `amount`: the amount to be transferred
* `asset_symbol`: symbol or ID of the asset to be transferred
* `broadcast`: true to broadcast the transaction on the network

#### [blind\_transfer](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id90)

[blind\_confirmation](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet18blind_confirmationE)`graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::blind_transfer`\(string _from\_key\_or\_label_, string _to\_key\_or\_label_, string _amount_, string _symbol_, bool _broadcast_ = false\)  


Transfer from one set of blinded balances to another.**Return**

a blind confirmation**Parameters**

* `from_key_or_label`: a public key in Base58 format or a label to transfer from
* `to_key_or_label`: a public key in Base58 format or a label to transfer to
* `amount`: the amount to be transferred
* `symbol`: symbol or ID of the asset to be transferred
* `broadcast`: true to broadcast the transaction on the network

#### [blind\_history](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id91)

vector&lt;blind\_receipt&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::blind_history`\(string _key\_or\_account_\)  


Get all blind receipts to/form a particular account**Return**

all blind receipts to/form the account**Parameters**

* `key_or_account`: a public key in Base58 format or an account

#### [receive\_blind\_transfer](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id92)

blind\_receipt `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::receive_blind_transfer`\(string _confirmation\_receipt_, string _opt\_from_, string _opt\_memo_\)  


Given a confirmation receipt, this method will parse it for a blinded balance and confirm that it exists in the blockchain. If it exists then it will report the amount received and who sent it.

**Return**

a blind receipt**Parameters**

* `confirmation_receipt`: a base58 encoded stealth confirmation
* `opt_from`: if not empty and the sender is a unknown public key, then the unknown public key will be given the label `opt_from`
* `opt_memo`: a self-defined label for this transfer to be saved in local wallet file

