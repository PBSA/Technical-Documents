# User Issued Assets \(UIAs\)

To create a UIA, run the cli\_wallet and enter the following command. This was used to create our BTCTEST UIA on Charlie, adjust as you see fit:

```text
create_asset <creator_account_id> <asset_symbol> <precision> <asset_details> <price_feed> <broadcast>
```

**creator\_account\_id**: Account ID Object in the form of 1.2.x

**asset\_symbol:** The trading symbol you wish your asset to have, ex. "BTC"

**precision:** The precision of the asset \(max 15\)

**asset\_details:** _see below command for more details. There are many components to the details of an asset_

**price\_feed:** The price feed can be set by populating this object. Use {} to tell the blockchain to use the default price feed values. If you use {}, or provide a price feed, the asset you are creating will be a MIA \(market issued asset\). Use 'null' if you want to control the issuing of this asset yourself.

**broadcast:** true/false whether or not you want to broadcast the transaction

### **Market Issued Asset Command** <a id="UserIssuedAssets(UIAs)-MarketIssuedAssetCommand"></a>

```text
create_asset "1.2.18" "BTF" 8 {"max_supply" : 1000000000000000,"market_fee_percent" : 0,"max_market_fee" : 1000000000000000,"issuer_permissions" : 79,"flags" : 0,"core_exchange_rate" : {"base": {"amount": 1,"asset_id": "1.3.0"},"quote": {"amount": 1,"asset_id": "1.3.1"}},"whitelist_authorities" : [],"blacklist_authorities" : [],"whitelist_markets" : [],"blacklist_markets" : [],"description" : "Bitcoin with precision 8"} {} true
```

{% hint style="info" %}
Make sure to include the precision with the value given to max\_supply. i.e, if you want a max supply of 10 thousand, and you have a precision of two, give a value of 1000000.
{% endhint %}

### **User Issued Asset Command** <a id="UserIssuedAssets(UIAs)-UserIssuedAssetCommand"></a>

```text
create_asset "1.2.18" "BTF" 8 {"max_supply" : 1000000000000000,"market_fee_percent" : 0,"max_market_fee" : 1000000000000000,"issuer_permissions" : 79,"flags" : 0,"core_exchange_rate" : {"base": {"amount": 1,"asset_id": "1.3.0"},"quote": {"amount": 1,"asset_id": "1.3.1"}},"whitelist_authorities" : [],"blacklist_authorities" : [],"whitelist_markets" : [],"blacklist_markets" : [],"description" : "Bitcoin with precision 8"} null true
```

{% hint style="info" %}
Make sure to include the precision with the value given to max\_supply. i.e, if you want a max supply of 10 thousand, and you have a precision of two, give a value of 1000000.
{% endhint %}

Note that the core asset has id 1.3.0, and you cant not "skip" id's. This means the first UIA you create must have id 1.3.1.

### **Issue Asset Command** <a id="UserIssuedAssets(UIAs)-IssueAssetCommand"></a>

```text
issue_asset 1.2.18 10000000 BTF "" true
```

Do this to claim the UIA that you just created

**Important Note:** The {} before the boolean broadcast value at the end of command will make the asset a "Market Issued Asset". If you want to create a "User Issued Asset" \(UIA\) replace the "{}" with "null".



#### What are the Flags? <a id="UserIssuedAssets(UIAs)-WhataretheFlags?"></a>

* `charge_market_fee`: an issuer-specified percentage of all market trades in this asset is paid to the issuer
* `white_list`: accounts must be white-listed in order to hold this asset
* `override_authority`: issuer may transfer asset back to himself
* `transfer_restricted`: require the issuer to be one party to every transfer
* `disable_force_settle`: disable force settling
* `global_settle`: \(only for bitassets\) allows bitasset issuer to force a global settling - this may be set in permissions, but should not be set as flag unless, for instance, a prediction market has to be resolved. If this flag has been enabled, no further shares can be borrowed!
* `disable_confidential`: allow the asset to be used with confidential transactions
* `witness_fed_asset`: allow the asset to be fed by witnesses
* `committee_fed_asset`: allow the asset to be fed by the committee

The permissions/flags in the asset\_details are ints and are a sum of the following mapping:

```text
"charge_market_fee" = 0x01 (1)
"white_list" = 0x02 (2)
"override_authority" = 0x04 (4)
"transfer_restricted" = 0x08 (8)
"disable_force_settle" = 0x10 (16)
"global_settle" = 0x20 (32)
"disable_confidential" = 0x40 (64)
"witness_fed_asset" = 0x80 (128)
"committee_fed_asset" = 0x100 (256)

```

So in our case, we set the flags to 0, which means all of these are disabled initially. The permissions is set to 79, which means that "charge\_market\_fee", "white\_list", "override\_authority", and "disable\_confidential" are able to be modified later. The other properties are immutable since they were set to False initially.



Claim the funds through the following command\(follow the precision\)

```text
issue_asset 1.2.18 10000000 BTF "" true
```







\*\*\*\*

