# Asset Calls

### [Asset Calls](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id51)

#### [list\_assets](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id52)

vector&lt;extended\_asset\_object&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::list_assets`\(_const_ string &_lowerbound_, uint32\_t _limit_\)_const_  


Lists all assets registered on the blockchain.

To list all assets, pass the empty string `""` for the lowerbound to start at the beginning of the list, and iterate as necessary.

**Return**

the list of asset objects, ordered by symbol**Parameters**

* `lowerbound`: the symbol of the first asset to include in the list.
* `limit`: the maximum number of assets to return \(max: 100\)

#### [create\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id53)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::create_asset`\(string _issuer_, string _symbol_, uint8\_t _precision_, asset\_options _common_, fc::optional&lt;bitasset\_options&gt; _bitasset\_opts_, bool _broadcast_ = false\)  


Creates a new user-issued or market-issued asset.

Many options can be changed later using [`update_asset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a6c2a57593b39390b286efeecca2702d6)

Right now this function is difficult to use because you must provide raw JSON data structures for the options objects, and those include prices and asset ids.

**Return**

the signed transaction creating a new asset**Parameters**

* `issuer`: the name or id of the account who will pay the fee and become the issuer of the new asset. This can be updated later
* `symbol`: the ticker symbol of the new asset
* `precision`: the number of digits of precision to the right of the decimal point, must be less than or equal to 12
* `common`: asset options required for all new assets. Note that core\_exchange\_rate technically needs to store the asset ID of this new asset. Since this ID is not known at the time this operation is created, create this price as though the new asset has instance ID 1, and the chain will overwrite it with the new asset’s ID.
* `bitasset_opts`: options specific to BitAssets. This may be null unless the `market_issued` flag is set in common.flags
* `broadcast`: true to broadcast the transaction on the network

#### [update\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id54)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::update_asset`\(string _symbol_, optional&lt;string&gt; _new\_issuer_, asset\_options _new\_options_, bool _broadcast_ = false\)  


Update the core options on an asset. There are a number of options which all assets in the network use. These options are enumerated in the asset\_object::asset\_options struct. This command is used to update these options for an existing asset.

**Note**

This operation cannot be used to update BitAsset-specific options. For these options, [`update_bitasset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1aebe4459f45a748739595939d60b95b6b) instead.**Return**

the signed transaction updating the asset**Parameters**

* `symbol`: the name or id of the asset to update
* `new_issuer`: if changing the asset’s issuer, the name or id of the new issuer. null if you wish to remain the issuer of the asset
* `new_options`: the new asset\_options object, which will entirely replace the existing options.
* `broadcast`: true to broadcast the transaction on the network

#### [update\_bitasset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id55)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::update_bitasset`\(string _symbol_, bitasset\_options _new\_options_, bool _broadcast_ = false\)  


Update the options specific to a BitAsset.

BitAssets have some options which are not relevant to other asset types. This operation is used to update those options an an existing BitAsset.

**See**

[update\_asset\(\)](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a6c2a57593b39390b286efeecca2702d6)**Return**

the signed transaction updating the bitasset**Parameters**

* `symbol`: the name or id of the asset to update, which must be a market-issued asset
* `new_options`: the new bitasset\_options object, which will entirely replace the existing options.
* `broadcast`: true to broadcast the transaction on the network

#### [update\_asset\_feed\_producers](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id56)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::update_asset_feed_producers`\(string _symbol_, flat\_set&lt;string&gt; _new\_feed\_producers_, bool _broadcast_ = false\)  


Update the set of feed-producing accounts for a BitAsset.

BitAssets have price feeds selected by taking the median values of recommendations from a set of feed producers. This command is used to specify which accounts may produce feeds for a given BitAsset.**Return**

the signed transaction updating the bitasset’s feed producers**Parameters**

* `symbol`: the name or id of the asset to update
* `new_feed_producers`: a list of account names or ids which are authorized to produce feeds for the asset. this list will completely replace the existing list
* `broadcast`: true to broadcast the transaction on the network

#### [publish\_asset\_feed](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id57)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::publish_asset_feed`\(string _publishing\_account_, string _symbol_, price\_feed _feed_, bool _broadcast_ = false\)  


Publishes a price feed for the named asset.

Price feed providers use this command to publish their price feeds for market-issued assets. A price feed is used to tune the market for a particular market-issued asset. For each value in the feed, the median across all committee\_member feeds for that asset is calculated and the market for the asset is configured with the median of that value.

The feed object in this command contains three prices: a call price limit, a short price limit, and a settlement price. The call limit price is structured as \(collateral asset\) / \(debt asset\) and the short limit price is structured as \(asset for sale\) / \(collateral asset\). Note that the asset IDs are opposite to eachother, so if we’re publishing a feed for USD, the call limit price will be CORE/USD and the short limit price will be USD/CORE. The settlement price may be flipped either direction, as long as it is a ratio between the market-issued asset and its collateral.

**Return**

the signed transaction updating the price feed for the given asset**Parameters**

* `publishing_account`: the account publishing the price feed
* `symbol`: the name or id of the asset whose feed we’re publishing
* `feed`: the price\_feed object containing the three prices making up the feed
* `broadcast`: true to broadcast the transaction on the network

#### [issue\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id58)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::issue_asset`\(string _to\_account_, string _amount_, string _symbol_, string _memo_, bool _broadcast_ = false\)  


Issue new shares of an asset.

**Return**

the signed transaction issuing the new shares**Parameters**

* `to_account`: the name or id of the account to receive the new shares
* `amount`: the amount to issue, in nominal units
* `symbol`: the ticker symbol of the asset to issue
* `memo`: a memo to include in the transaction, readable by the recipient
* `broadcast`: true to broadcast the transaction on the network

#### [get\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id59)

extended\_asset\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_asset`\(string _asset\_name\_or\_id_\)_const_  


Returns information about the given asset.**Return**

the information about the asset stored in the block chain**Parameters**

* `asset_name_or_id`: the symbol or id of the asset in question

#### [get\_bitasset\_data](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id60)

asset\_bitasset\_data\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_bitasset_data`\(string _asset\_name\_or\_id_\)_const_  


Returns the BitAsset-specific data for a given asset. Market-issued assets’s behavior are determined both by their “BitAsset Data” and their basic asset data, as returned by [`get_asset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1aae54080626cf4e4b24572f4836e8dfdd).**Return**

the BitAsset-specific data for this asset**Parameters**

* `asset_name_or_id`: the symbol or id of the BitAsset in question

#### [fund\_asset\_fee\_pool](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id61)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::fund_asset_fee_pool`\(string _from_, string _symbol_, string _amount_, bool _broadcast_ = false\)  


Pay into the fee pool for the given asset.

User-issued assets can optionally have a pool of the core asset which is automatically used to pay transaction fees for any transaction using that asset \(using the asset’s core exchange rate\).

This command allows anyone to deposit the core asset into this fee pool.

**Return**

the signed transaction funding the fee pool**Parameters**

* `from`: the name or id of the account sending the core asset
* `symbol`: the name or id of the asset whose fee pool you wish to fund
* `amount`: the amount of the core asset to deposit
* `broadcast`: true to broadcast the transaction on the network

#### [reserve\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id62)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::reserve_asset`\(string _from_, string _amount_, string _symbol_, bool _broadcast_ = false\)  


Burns an amount of given asset.

This command burns an amount of given asset to reduce the amount in circulation.**Note**

you cannot burn market-issued assets.**Return**

the signed transaction burning the asset**Parameters**

* `from`: the account containing the asset you wish to burn
* `amount`: the amount to burn, in nominal units
* `symbol`: the name or id of the asset to burn
* `broadcast`: true to broadcast the transaction on the network

#### [global\_settle\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id63)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::global_settle_asset`\(string _symbol_, price _settle\_price_, bool _broadcast_ = false\)  


Forces a global settling of the given asset \(black swan or prediction markets\).

In order to use this operation, asset\_to\_settle must have the global\_settle flag set

When this operation is executed all open margin positions are called at the settle price. A pool will be formed containing the collateral got from the margin positions. Users owning an amount of the asset may use [`settle_asset()`](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#classgraphene_1_1wallet_1_1wallet__api_1a95a3baa4b0c83c1fce14827acbbddd62) to claim collateral instantly at the settle price from the pool. If this asset is used as backing for other bitassets, those bitassets will not be affected.

**Note**

this operation is used only by the asset issuer.**Return**

the signed transaction settling the named asset**Parameters**

* `symbol`: the name or id of the asset to globally settle
* `settle_price`: the price at which to settle
* `broadcast`: true to broadcast the transaction on the network

