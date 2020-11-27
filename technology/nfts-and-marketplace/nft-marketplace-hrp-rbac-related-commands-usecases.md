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

```text
list_sell_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```text
list_sell_offers 10 1.29.6
```

* `10` Number of items per fetch \(pagination\)
* `1.29.6` Index to start the search for. \(pagination\)

Example command without pagination,

```text
list_sell_offers 10 null
```





#### **List Buy Offers** <a id="List-Buy-Offers"></a>

Returns all the buy listing offer objects in the marketplace. \(Reverse Auction offer objects\).

```text
list_buy_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```text
list_buy_offers 10 1.29.6
```

* `10` Number of items per fetch \(pagination\)
* `1.29.6` Index to start the search for. \(pagination\)

Example command without pagination,

```text
list_buy_offers 10 null
```





#### **List Offer History** <a id="List-Offer-History"></a>

Returns all the closed auction and reverse auction listings. \(offers\)

```text
list_offer_history(uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example command,

```text
list_offer_history 10 2.24.2
```

* `10`Number of items per fetch \(pagination\)
* `2.24.2`Index to start the search for. \(pagination\)

Example command without pagination,

```text
list_offer_history 10 null
```



#### Get Offers by Issuer <a id="Get-Offers-by-Issuer"></a>

Returns all the offers listed by an account.

```text
get_offers_by_issuer(string issuer_account_id_or_name, uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```text
get_offers_by_issuer account02 10 1.29.0
```

* `account02` issuer
* `10` Number of items per fetch \(Pagination\)
* `1.29.0` Index to start the search for. \(pagination\)

Example command without pagination,

```text
get_offers_by_issuer account02 10 null
```



#### Get Offers by NFT Item <a id="Get-Offers-by-NFT-Item"></a>

```text
get_offers_by_item(const nft_id_type item, uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```text
get_offers_by_item 1.31.0 10 1.29.1
```

* `1.31.0` NFT Id
* `10` Number of items per fetch \(Pagination\)
* `1.29.1` Index to start the search for. \(pagination\)

Example command without pagination,

```text
get_offers_by_item 1.31.0 10 null
```



#### Get Offer History by Issuer <a id="Get-Offer-History-by-Issuer"></a>

```text
get_offer_history_by_issuer(string issuer_account_id_or_name, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example command,

```text
get_offer_history_by_issuer account06 10 2.24.1
```

* account06 Issuer
* 10 Number of items per fetch \(Pagination\)
* 2.24.1 Index to start the search for. \(pagination\)

Example command without pagination,

```text
get_offer_history_by_issuer account06 10 null
```





#### Get Offer History by NFT Item <a id="Get-Offer-History-by-NFT-Item"></a>

```text
get_offer_history_by_item(const nft_id_type item, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example Command,

```text
get_offer_history_by_item 1.31.0 10 2.24.1
```

* `1.31.0` NFT Id
* `10` Number of items per fetch \(Pagination\)
* `2.24.1` Index to start the search for. \(pagination\)

Example command without pagination,

```text
get_offer_history_by_item 1.31.0 10 null
```





#### Get Offer History by Bidder <a id="Get-Offer-History-by-Bidder"></a>

```text
get_offer_history_by_bidder(string bidder_account_id_or_name, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example Command,

```text
get_offer_history_by_bidder account07 10 2.24.1
```

* account07 Bidder
* 10 Number of items per fetch \(Pagination\)
* 2.24.1 Index to start the search for. \(pagination\) ****

Example command without pagination,

```text
get_offer_history_by_bidder account07 10 null
```





## HRP User Authorities

> Hierarchical role based permissions is a feature of Peerplays blockchain which helps in increasing the security of user accounts.
>
> Users don’t have to use their active and owner keys for everything they do on the chain.
>
> They can create role based custom permissions and map them to different keys other than active and owner keys.
>
> They can then use these custom keys to sign transactions.



#### Create Custom Permissions <a id="Create-Custom-Permissions"></a>

```text
 create_custom_permission(string owner, string permission_name, authority auth, bool broadcast)
```

* owner Owner of the account who is creating the custom permission
* permission\_name Permission name eg. nftcreatepermission
* auth authority is account authority, more info at [permissions](https://peerplays.atlassian.net/wiki/spaces/EP/pages/197466275/Reverse+Engineering+-+Peerplaysjs-lib), [public/private keys](https://peerplays.atlassian.net/wiki/spaces/EKB/pages/197460303), [multi authority](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/66519041/Compare+of+Master5050+and+Hardfork+Code)

Example Command,

```text
create_custom_permission account01 perm1 { "weight_threshold": 1,  "account_auths": [["1.2.52",1]], "key_auths": [["TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto",1]], "address_auths": [] } true
```

`{ "weight_threshold": 1, "account_auths": [["1.2.52",1]], "key_auths": [["TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto",1]], "address_auths": [] }`

> This represents an authority structure, `account_auths` represent the amount of weight each accounts have on our account, in this example 1.2.52 has weight 1
>
> `key_auths` represent the amount of weight each public key has on this account, in this example `TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto` has weight 1
>
> `Weight_threshold` represent the required weight for a transaction to be signed successfully.
>
> In this example either `1.2.52` can sign with his active key or `TEST71ADtL4fzjGKErk9nQJrABmCPUR8QCjkCUNfdmgY5yDzQGhwto` can be used to sign a transaction successfully.



#### Get Custom Permissions <a id="Get-Custom-Permissions"></a>

```text
get_custom_permissions(string owner)
```

Example Command,

```text
get_custom_permissions account01
```

####  <a id="Update-Custom-Permissions"></a>

#### Update Custom Permissions <a id="Update-Custom-Permissions"></a>

```text
update_custom_permission(string owner, custom_permission_id_type permission_id, fc::optional<authority> new_auth, bool broadcast)
```

Example Command,

```text
update_custom_permission account01 1.27.0 { "weight_threshold": 1,  "account_auths": [["1.2.53",1]], "key_auths": [], "address_auths": [] } true
```

Here we removed the `key_auths` and added `1.2.53` with weight 1, which is equal to `weight_threshold` , so `1.2.53` can alone sign the transaction successfully.



#### Create Custom Account Authority <a id="Create-Custom-Account-Authority"></a>

> Creating custom authority maps the created custom permissions with the actual operations present on the blockchain.
>
> It also has expiry time by when this custom permission is no more valid on any given account and operation combination.

```text
create_custom_account_authority(string owner, custom_permission_id_type permission_id, int operation_type, fc::time_point_sec valid_from, fc::time_point_sec valid_to, bool broadcast)
```

Example Command,

```text
create_custom_account_authority account01 1.27.0 0 "2020-11-02T17:53:25" "2020-12-03T17:53:25" true
```

`account01` is the owner of the account and the one who created a permission `1.27.0`

`1.27.0` is the custom permission created

`0` is the operation number, refer to operations at [operations](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/329351829/Peerplays+Alice+Chain+API+Endpoints+in+Development), here `0` is `transfer_operation`

`"2020-11-02T17:53:25"` valid from timestamp

`"2020-12-03T17:53:25"` valid to timestamp

`true` broadcast

Basically this represents a full HRP where transfer operation on `account01` can be done by authorities present in `1.27.0` instead of account owner `account01`



#### Update Custom Account Authority <a id="Update-Custom-Account-Authority"></a>

Can be used to update existing `valid_from` and `valid_to` times,

```text
update_custom_account_authority(string owner, custom_account_authority_id_type auth_id, fc::optional<fc::time_point_sec> new_valid_from, fc::optional<fc::time_point_sec> new_valid_to, bool broadcast)
```

Example command,

```text
update_custom_account_authority account01 1.28.0 "2020-06-02T17:52:25" "2020-06-03T17:52:25" true
```





#### Delete Custom Permission <a id="Delete-Custom-Permission"></a>

Used to delete the existing custom permission, this will delete all the custom account authorities linked to this permission as well.\( cascading delete \)

```text
delete_custom_permission(string owner, custom_permission_id_type permission_id, bool broadcast)
```

Example command,

```text
delete_custom_permission account01 1.27.0 true
```





#### Delete Custom Account Authority <a id="Delete-Custom-Account-Authority"></a>

Used to delete an account authority attached to a permission.

```text
delete_custom_account_authority(string owner, custom_account_authority_id_type auth_id, bool broadcast)
```

Example command,

```text
delete_custom_account_authority account01 1.28.0 true
```





### Resource Permissions \( Account Roles \) <a id="Resource-Permissions-(-Account-Roles-)"></a>

As opposed to HRP mentioned above, resource permissions are controlled by an owner of a resource \(eg. NFT metadata is a resource\)

These are similar to IAM permissions in AWS Cloud environment.



#### Create Account Role <a id="Create-Account-Role"></a>

Used to create an account role.

```text
create_account_role(string owner_account_id_or_name, string name, string metadata, flat_set<int> allowed_operations, flat_set<account_id_type> whitelisted_accounts, time_point_sec valid_to, bool broadcast)
```

`owner_account_id_or_name` resource owner Eg. account creating an NFT Metadata resource

`name` name of the account role Eg. Movie Interstellar Permissions

`metadata` metadata for additional info Eg. Some JSON struct or an external URL with info

`allowed_operations` allowed operations that `whitelisted_accounts` can perform on this resource.

`whitelisted_accounts` All the accounts that can perform any `allowed_operations` on a resource

`valid_to` exports time of the account role, valid from is the time of creation of the account role

`broadcast` broadcast mentioned above

Currently valid `allowed_operations` are

`offer_operation`**\(88\),** Checks if the user who is listing an NFT for sale is in `whitelisted_accounts` , if not the user can’t list the NFTs in marketplace.

`bid_operation`**\(89\),** Checks if the user who is bidding for an NFT on sale is in `whitelisted_accounts` , if not the user can’t bid / buy the NFT from marketplace.

 `nft_safe_transfer_from_operation` **\(95\),** Checks if the user who transferring i.e. owner is in `whitelisted_accounts`, if yes it checks the to-account is also in the `whitelisted_accounts`. If any of the two checks fail, the transfer fails.



More operations like NFT Lottery, RNG are to be attached to account roles.

Example Command,

```text
create_account_role account01 ar1 ar1 [89,95] [1.2.40, 1.2.41, 1.2.43] "2020-09-04T13:43:39" true
```

This command effectively limits any NFT to be sold or transferred between only three accounts, `1.2.40, 1.2.41, 1.2.43`

For attaching NFT metadata to an account role, please refer to the above NFT sections.

