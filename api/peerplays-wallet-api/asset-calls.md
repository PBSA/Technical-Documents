# Asset Calls

## Asset Calls

### list\_assets

Lists all assets registered on the blockchain.

To list all assets, pass the empty string `""` for the `lowerbound` to start at the beginning of the list, and iterate as necessary.

```cpp
vector<extended_asset_object> graphene::wallet::wallet_api::list_assets(
    const string &lowerbound, 
    uint32_t limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`lowerbound`**: the symbol of the first asset to include in the list.
* **`limit`**: the maximum number of assets to return (max: 100)
{% endtab %}

{% tab title="Return" %}
The list of asset objects, ordered by symbol.
{% endtab %}
{% endtabs %}

### create\_asset

Creates a new user-issued or market-issued asset.

Many options can be changed later using [`update_asset()`](asset-calls.md#update\_asset)``

{% hint style="warning" %}
**Note**: Right now this function is difficult to use because you must provide raw JSON data structures for the options objects, and those include prices and asset ids.
{% endhint %}

```cpp
signed_transaction graphene::wallet::wallet_api::create_asset(
    string issuer, 
    string symbol, 
    uint8_t precision, 
    asset_options common, 
    fc::optional<bitasset_options> bitasset_opts, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`issuer`**: the name or id of the account who will pay the fee and become the issuer of the new asset. This can be updated later
* **`symbol`**: the ticker symbol of the new asset
* `precision`: the number of digits of precision to the right of the decimal point, must be less than or equal to 12
* **`common`**: asset options required for all new assets. Note that core\_exchange\_rate technically needs to store the asset ID of this new asset. Since this ID is not known at the time this operation is created, create this price as though the new asset has instance ID 1, and the chain will overwrite it with the new asset’s ID.
* **`bitasset_opts`**: options specific to BitAssets. This may be null unless the `market_issued` flag is set in common.flags
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction creating a new asset.
{% endtab %}
{% endtabs %}

### update\_asset

Update the core options on an asset. There are a number of options which all assets in the network use. These options are enumerated in the `asset_object::asset_options` struct.&#x20;

This command is used to update these options for an existing asset.

```cpp
signed_transaction graphene::wallet::wallet_api::update_asset(
    string symbol, 
    optional<string> new_issuer, 
    asset_options new_options, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`symbol`**: the name or id of the asset to update
* **`new_issuer`**: if changing the asset’s issuer, the name or id of the new issuer. null if you wish to remain the issuer of the asset
* **`new_options`**: the new asset\_options object, which will entirely replace the existing options.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction updating the asset
{% endtab %}
{% endtabs %}

### update\_bitasset

Update the options specific to a BitAsset.

BitAssets have some options which are not relevant to other asset types. This operation is used to update those options an an existing BitAsset.

**See** [update\_asset()](asset-calls.md#update\_asset)

```cpp
signed_transaction graphene::wallet::wallet_api::update_bitasset(
    string symbol, 
    bitasset_options new_options, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`symbol`**: the name or id of the asset to update, which must be a market-issued asset
* **`new_options`**: the new `bitasset_options` object, which will entirely replace the existing options.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction updating the Bitasset
{% endtab %}
{% endtabs %}

### update\_asset\_feed\_producers

Update the set of feed-producing accounts for a BitAsset.

BitAssets have price feeds selected by taking the median values of recommendations from a set of feed producers. This command is used to specify which accounts may produce feeds for a given BitAsset.

```cpp
signed_transaction graphene::wallet::wallet_api::update_asset_feed_producers(
    string symbol, 
    flat_set<string> new_feed_producers, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`symbol`**: the name or id of the asset to update
* **`new_feed_producers`**: a list of account names or ids which are authorized to produce feeds for the asset. this list will completely replace the existing list
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction updating the BitAsset’s feed producers
{% endtab %}
{% endtabs %}

### publish\_asset\_feed

Publishes a price feed for the named asset.

Price feed providers use this command to publish their price feeds for market-issued assets. A price feed is used to tune the market for a particular market-issued asset. For each value in the feed, the median across all committee\_member feeds for that asset is calculated and the market for the asset is configured with the median of that value.

The feed object in this command contains three prices:&#x20;

* A call price limit
* A short price limit,
* A settlement price

The call limit price is structured as (collateral asset) / (debt asset) and the short limit price is structured as (asset for sale) / (collateral asset).&#x20;

{% hint style="warning" %}
**Note**:  The asset IDs are opposite to each other, so if we’re publishing a feed for USD, the call limit price will be CORE/USD and the short limit price will be USD/CORE.&#x20;
{% endhint %}

The settlement price may be flipped either direction, as long as it is a ratio between the market-issued asset and its collateral.

```cpp
signed_transaction graphene::wallet::wallet_api::publish_asset_feed(
    string publishing_account, 
    string symbol, 
    price_feed feed, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`publishing_account`**: the account publishing the price feed
* **`symbol`**: the name or id of the asset whose feed we’re publishing
* **`feed`**: the price\_feed object containing the three prices making up the feed
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction updating the price feed for the given asset.
{% endtab %}
{% endtabs %}

### issue\_asset

Issue new shares of an asset.

```cpp
signed_transaction graphene::wallet::wallet_api::issue_asset(
    string to_account, 
    string amount, 
    string symbol, 
    string memo, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`to_account`**: the name or id of the account to receive the new shares
* **`amount`**: the amount to issue, in nominal units
* **`symbol`**: the ticker symbol of the asset to issue
* **`memo`**: a memo to include in the transaction, readable by the recipient
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction issuing the new shares
{% endtab %}
{% endtabs %}

### get\_asset

Returns information about the given asset.

```cpp
extended_asset_object graphene::wallet::wallet_api::get_asset(
    string asset_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`asset_name_or_id`**: the symbol or id of the asset in question
{% endtab %}

{% tab title="Return" %}
The information about the asset stored in the block chain.
{% endtab %}
{% endtabs %}

### **get\_bitasset\_data**

Returns the BitAsset-specific data for a given asset. Market-issued assets’s behaviour are determined both by their “BitAsset Data” and their basic asset data, as returned by [`get_asset()`](asset-calls.md#get\_asset)``

```cpp
asset_bitasset_data_object graphene::wallet::wallet_api::get_bitasset_data(
    string asset_name_or_id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`asset_name_or_id`**: the symbol or id of the BitAsset in question
{% endtab %}

{% tab title="Return" %}
The BitAsset-specific data for this asset
{% endtab %}
{% endtabs %}

### **fund\_asset\_fee\_pool**

Pay into the fee pool for the given asset.

User-issued assets can optionally have a pool of the core asset which is automatically used to pay transaction fees for any transaction using that asset (using the asset’s core exchange rate).

This command allows anyone to deposit the core asset into this fee pool.

```cpp
signed_transaction graphene::wallet::wallet_api::fund_asset_fee_pool(
    string from, 
    string symbol, 
    string amount, 
    bool broadcast = false)

```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the name or id of the account sending the core asset
* **`symbol`**: the name or id of the asset whose fee pool you wish to fund
* **`amount`**: the amount of the core asset to deposit
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
**T**he signed transaction funding the fee pool.
{% endtab %}
{% endtabs %}

### reserve\_asset

Burns an amount of given asset.

This command burns an amount of given asset to reduce the amount in circulation.

{% hint style="warning" %}
**Note: Y**ou can't burn market-issued assets.
{% endhint %}

```cpp
signed_transaction graphene::
wallet
::
wallet_api
::reserve_asset(string from, string amount, string symbol, bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the account containing the asset you wish to burn
* **`amount`**: the amount to burn, in nominal units
* **`symbol`**: the name or id of the asset to burn
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction burning the asset
{% endtab %}
{% endtabs %}

### global\_settle\_asset

Forces a global settling of the given asset (black swan or prediction markets).

In order to use this operation, `asset_to_settle` must have the `global_settle` flag set

When this operation is executed all open margin positions are called at the settle price. A pool will be formed containing the collateral got from the margin positions. Users owning an amount of the asset may use [`settle_asset()`](https://dev.bitshares.works/en/master/api/wallet\_api.html?highlight=set\_voting\_proxy#classgraphene\_1\_1wallet\_1\_1wallet\_\_api\_1a95a3baa4b0c83c1fce14827acbbddd62) to claim collateral instantly at the settle price from the pool.&#x20;

If this asset is used as backing for other BitAssets, those BitAssets will not be affected.

{% hint style="warning" %}
**Note: T**his operation is used only by the asset issuer.
{% endhint %}

```cpp
signed_transaction graphene::
wallet
::
wallet_api
::global_settle_asset(string symbol, price settle_price, bool broadcast = false)

```

{% tabs %}
{% tab title="Parameters" %}
* **`symbol`**: the name or id of the asset to globally settle
* **`settle_price`**: the price at which to settle
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction settling the named asset
{% endtab %}
{% endtabs %}
