# NFT Marketplace HRP RBAC related Commands/Usecases

Here we learn some command/usecases related to NFT Marketplace HRP RBAC



#### **1. NFT Metadata Creation** <a id="1.-NFT-Metadata-Creation"></a>

```text
nft_metadata_create(string owner_account_id_or_name, string name, string symbol, string base_uri, optional<string> revenue_partner, optional<uint16_t> revenue_split, bool is_transferable, bool is_sellable, optional<account_role_id_type> role_id, bool broadcast)
```

Example NFT metadata create command without permissions,

```text
nft_metadata_create account01 "AVALON NAME" "AVALON SYMBOL" "AVALON BASE URI" account02 100 false true null true
```



* **account01** is the owner account of the metadata
* **“AVALON NAME”** is the name of the NFT types created from the NFT metadata being created
* **“AVALON SYMBOL”** is the symbol of the NFT types created from the NFT metadata being created. The symbol is reserved for future use.
* **“AVALON BASE URI”** is the URI of the NFT types. Eg: [**avalonmeta.com**](http://avalonmeta.com)
* **account02** is the revenue partner, this can be owner as well. Whenever the minted NFT is sold in the marketplace a split of the selling price goes to the revenue partner. The split can be specified in **revenue\_split** parameter.
* **100** is the revenue\_split on the scale of 10000 being 100 percent. i.e. 100 is 1%
* is\_transferable specifies if the minted NFTs can be transferred by the owner of the NFT to other accounts
* is\_sellable specifies if the minted NFTs can be sold in the marketplace.
* role\_id is the resource permissions specifying the whitelisting accounts among which the NFTs can either be transferred or sold on marketplace.
* broadcast either true or false, keeping it true will include the transaction in the upcoming block on the chain.



#### Example NFT metadata create command with permissions <a id="Example-NFT-metadata-create-command-with-permissions"></a>

First create a permission

```text
create_account_role(string owner_account_id_or_name, string name, string metadata, flat_set<int> allowed_operations, flat_set<account_id_type> whitelisted_accounts, time_point_sec valid_to, bool broadcast)
```

Example Permission,

```text
create_account_role account01 "Avalon Permissions1" "Permission Metadata" [88,89,90,95] [1.2.40, 1.2.41] "2020-11-04T13:43:39" true
```



* **account01** is the owner of the permission being created which can be attached to NFT metadata.
* **“Avalon Permissions1”** is the name of the resource permission being created.
* **“Permission Metadata”** is the metadata, can be a JSON as well.
* **\[88,89,90,95\]** is the list of operations this permission whitelists for accounts. 88 - List Offer operation in marketplace 89 - Bid operation in marketplace 90 - Cancel Offer operation in marketplace 95 - Safe transfer from owner to another NFT
* **\[1.2.40, 1.2.41\]** List of accounts whitelisted for the above operations. Offer, bid and transfer operations can only be among the whitelisted accounts.
* `"2020-11-04T13:43:39"` Expiry date of the permission
* `true` broadcast, keep it true to include the transaction in upcoming block.

Example NFT metadata create command with permissions,

```text
nft_metadata_create account01 "AVALON NAME" "AVALON SYMBOL" "AVALON BASE URI" account02 100 false true 1.32.0 true
```

* 1.32.0 This is specified in the metadata create operation to attach permissions to all the NFTs created from this metadata.



#### **NFT creation** <a id="NFT-creation"></a>

NFT is always minted by the owner of the metadata object this NFT is based on. The fees associated with minting this NFT is just the transaction fee + storage fee which are both collected in core PPY token. After the NFT has been minted this can be sold on marketplace in any token of NFT owner’s choice.

Syntax is,

```text
nft_create(string metadata_owner_account_id_or_name, nft_metadata_id_type metadata_id, string owner_account_id_or_name, string approved_account_id_or_name, string token_uri, bool broadcast)
```

Example NFT creation based on the metadata created above,

```text
nft_create account01 1.30.0 account02 account02 "AVALON NFT URI" true
```



* **account01** is the owner of the metadata this NFT is based on
* **1.30.0** is the metadata ID of the metadata object this NFT is based on \( Currently this works on best guess method by doing get\_object 1.30.0, 1.30.1, 1.30.2 …. so on, in future a new cli command will be introduced that can list all the metadata objects by owner account \).
* **account02** is the owner of the NFT, can be one of the user of the Avalon App.
* **account02** is the approved account of the NFT, can be the owner as well.
* `"AVALON NFT URI"`is the URI that has additional information about this NFT.



#### **NFT Safe Transfer from owner to other account** <a id="NFT-Safe-Transfer-from-owner-to-other-account"></a>

```text
nft_safe_transfer_from(string operator_account_id_or_name, string from_account_id_or_name, string to_account_id_or_name, nft_id_type token_id, string data, bool broadcast)
```

Example command is, Note that NFTs can be transferred only when `is_transferableflag` is set to true, also if permissions are enabled both from and to accounts have to be whitelisted.

```text
nft_safe_transfer_from account02 account02 account03 1.31.0 "Enjoy my NFT" true
```

* **account02** operator or owner account
* **account02** from account
* **account03** to account
* **1.31.0** Id of the NFT
* "Enjoy my NFT"is the metadata of the transfer
* **true** is broadcast option mentioned above. ****

#### **Create Market Listing Offer** <a id="Create-Market-Listing-Offer"></a>

```text
create_offer(set<nft_id_type> item_ids, string issuer_accound_id_or_name, asset minimum_price, asset maximum_price, bool buying_item, time_point_sec offer_expiration_date, optional<memo_data> memo, bool broadcast)
```

Example command is, 

Note that is\_sellable of NFT metadata should be true to make listing of the NFTs created from the metadata. 

Only owner of the NFT can create a listing in auction.

```text
create_offer ["1.31.0","1.31.1"] account02 {"amount":100,"asset_id":"1.3.0"} {"amount":10000,"asset_id":"1.3.0"} false "2020-08-18T11:05:39" null true 
```

* `["1.31.0","1.31.1"]` - List of NFTs to sell or buy
* **account02** is the owner of the NFTs
* `{"amount":100,"asset_id":"1.3.0"}` minimum bid price expected
* `{"amount":10000,"asset_id":"1.3.0"}` maximum bid price expected
* `false` buying\_item denotes if this is an auction or reverse auction
* `"2020-08-18T11:05:39"` Listing expiry date
* `null` Optional metadata, if not null assign a string value
* `true` broadcast value mentioned above



#### **Create Bid Offer** <a id="Create-Bid-Offer"></a>

```text
create_bid(string bidder_account_id_or_name, asset bid_price, offer_id_type offer_id, bool broadcast)
```

Example bid command,

```text
create_bid account04 {"amount":10000,"asset_id":"1.3.0"} 1.29.0 true
```

* **account04** is the bidder
* `{"amount":10000,"asset_id":"1.3.0"}` bid price
* `1.29.0` Offer listing objected created from create\_offer command
* `true` broadcast value mentioned above



#### **Cancel Offer** <a id="Cancel-Offer"></a>

```text
cancel_offer(string issuer_account_id_or_name, offer_id_type offer_id, bool broadcast)
```

Example cancel offer command is,

```text
cancel_offer account02 1.29.0 true
```

* **account02** is the creator of the listing
* `1.29.0` is the offer object
* `true` broadcast value mentioned above



#### **List Offers** <a id="List-Offers"></a>

Returns all the sell and buy listings offer objects in the marketplace.

```text
list_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```text
list_offers 100 1.29.0
```

* `100` Number of items per fetch \(pagination\)
* `1.29.0` Index to start the search for. \(pagination\)

Example command without pagination,

```text
list_offers 100 null
```

\*\*\*\*

#### **List Sell Offers** <a id="List-Sell-Offers"></a>

Returns all the sell listing offer objects in the marketplace.

