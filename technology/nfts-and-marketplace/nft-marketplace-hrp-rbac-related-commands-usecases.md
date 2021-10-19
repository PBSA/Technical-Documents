# NFT Marketplace HRP RBAC related Commands/Usecases

Here we learn some command/usecases related to NFT Marketplace HRP RBAC



#### **1. NFT Metadata Creation** <a href="1.-nft-metadata-creation" id="1.-nft-metadata-creation"></a>

```
nft_metadata_create(string owner_account_id_or_name, string name, string symbol, string base_uri, optional<string> revenue_partner, optional<uint16_t> revenue_split, bool is_transferable, bool is_sellable, optional<account_role_id_type> role_id, bool broadcast)
```

Example NFT metadata create command without permissions,

```
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



#### Example NFT metadata create command with permissions <a href="example-nft-metadata-create-command-with-permissions" id="example-nft-metadata-create-command-with-permissions"></a>

First create a permission

```
create_account_role(string owner_account_id_or_name, string name, string metadata, flat_set<int> allowed_operations, flat_set<account_id_type> whitelisted_accounts, time_point_sec valid_to, bool broadcast)
```

Example Permission,

```
create_account_role account01 "Avalon Permissions1" "Permission Metadata" [88,89,90,95] [1.2.40, 1.2.41] "2020-11-04T13:43:39" true
```



* **account01** is the owner of the permission being created which can be attached to NFT metadata.
* **“Avalon Permissions1”** is the name of the resource permission being created.
* **“Permission Metadata”** is the metadata, can be a JSON as well.
* **\[88,89,90,95]** is the list of operations this permission whitelists for accounts.\
  88 - List Offer operation in marketplace\
  89 - Bid operation in marketplace\
  90 - Cancel Offer operation in marketplace\
  95 - Safe transfer from owner to another NFT
* **\[1.2.40, 1.2.41]** List of accounts whitelisted for the above operations. Offer, bid and transfer operations can only be among the whitelisted accounts.
* `"2020-11-04T13:43:39"` Expiry date of the permission
* `true` broadcast, keep it true to include the transaction in upcoming block.

Example NFT metadata create command with permissions,

```
nft_metadata_create account01 "AVALON NAME" "AVALON SYMBOL" "AVALON BASE URI" account02 100 false true 1.32.0 true
```

* 1.32.0 This is specified in the metadata create operation to attach permissions to all the NFTs created from this metadata.



#### **NFT creation** <a href="nft-creation" id="nft-creation"></a>

NFT is always minted by the owner of the metadata object this NFT is based on. The fees associated with minting this NFT is just the transaction fee + storage fee which are both collected in core PPY token. After the NFT has been minted this can be sold on marketplace in any token of NFT owner’s choice.

Syntax is,

```
nft_create(string metadata_owner_account_id_or_name, nft_metadata_id_type metadata_id, string owner_account_id_or_name, string approved_account_id_or_name, string token_uri, bool broadcast)
```

Example NFT creation based on the metadata created above,

```
nft_create account01 1.30.0 account02 account02 "AVALON NFT URI" true
```



* **account01** is the owner of the metadata this NFT is based on
* **1.30.0** is the metadata ID of the metadata object this NFT is based on ( Currently this works on best guess method by doing get\_object 1.30.0, 1.30.1, 1.30.2 …. so on, in future a new cli command will be introduced that can list all the metadata objects by owner account ).
* **account02** is the owner of the NFT, can be one of the user of the Avalon App.
* **account02** is the approved account of the NFT, can be the owner as well.
* `"AVALON NFT URI"`is the URI that has additional information about this NFT.



#### **NFT Safe Transfer from owner to other account** <a href="nft-safe-transfer-from-owner-to-other-account" id="nft-safe-transfer-from-owner-to-other-account"></a>

```
nft_safe_transfer_from(string operator_account_id_or_name, string from_account_id_or_name, string to_account_id_or_name, nft_id_type token_id, string data, bool broadcast)
```

Example command is, Note that NFTs can be transferred only when `is_transferableflag` is set to true, also if permissions are enabled both from and to accounts have to be whitelisted.

```
nft_safe_transfer_from account02 account02 account03 1.31.0 "Enjoy my NFT" true
```

* **account02** operator or owner account
* **account02** from account
* **account03** to account
* **1.31.0** Id of the NFT
* "Enjoy my NFT"is the metadata of the transfer
* **true** is broadcast option mentioned above.\
  ****

#### **Create Market Listing Offer** <a href="create-market-listing-offer" id="create-market-listing-offer"></a>

```
create_offer(set<nft_id_type> item_ids, string issuer_accound_id_or_name, asset minimum_price, asset maximum_price, bool buying_item, time_point_sec offer_expiration_date, optional<memo_data> memo, bool broadcast)
```

Example command is,&#x20;

Note that is\_sellable of NFT metadata should be true to make listing of the NFTs created from the metadata.&#x20;

Only owner of the NFT can create a listing in auction.

```
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



#### **Create Bid Offer** <a href="create-bid-offer" id="create-bid-offer"></a>

```
create_bid(string bidder_account_id_or_name, asset bid_price, offer_id_type offer_id, bool broadcast)
```

Example bid command,

```
create_bid account04 {"amount":10000,"asset_id":"1.3.0"} 1.29.0 true
```

* **account04** is the bidder
* `{"amount":10000,"asset_id":"1.3.0"}` bid price
* `1.29.0` Offer listing objected created from create\_offer command
* `true` broadcast value mentioned above



#### **Cancel Offer** <a href="cancel-offer" id="cancel-offer"></a>

```
cancel_offer(string issuer_account_id_or_name, offer_id_type offer_id, bool broadcast)
```

Example cancel offer command is,

```
cancel_offer account02 1.29.0 true
```

* **account02** is the creator of the listing
* `1.29.0` is the offer object
* `true` broadcast value mentioned above



#### **List Offers** <a href="list-offers" id="list-offers"></a>

Returns all the sell and buy listings offer objects in the marketplace.

```
list_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```
list_offers 100 1.29.0
```

* `100` Number of items per fetch (pagination)
* `1.29.0` Index to start the search for. (pagination)

Example command without pagination,

```
list_offers 100 null
```

****

#### **List Sell Offers** <a href="list-sell-offers" id="list-sell-offers"></a>

Returns all the sell listing offer objects in the marketplace.

```
list_sell_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```
list_sell_offers 10 1.29.6
```

* `10` Number of items per fetch (pagination)
* `1.29.6` Index to start the search for. (pagination)

Example command without pagination,

```
list_sell_offers 10 null
```





#### **List Buy Offers** <a href="list-buy-offers" id="list-buy-offers"></a>

Returns all the buy listing offer objects in the marketplace. (Reverse Auction offer objects).

```
list_buy_offers(uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```
list_buy_offers 10 1.29.6
```

* `10` Number of items per fetch (pagination)
* `1.29.6` Index to start the search for. (pagination)

Example command without pagination,

```
list_buy_offers 10 null
```





#### **List Offer History** <a href="list-offer-history" id="list-offer-history"></a>

Returns all the closed auction and reverse auction listings. (offers)

```
list_offer_history(uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example command,

```
list_offer_history 10 2.24.2
```

* `10`Number of items per fetch (pagination)
* `2.24.2`Index to start the search for. (pagination)

Example command without pagination,

```
list_offer_history 10 null
```



#### Get Offers by Issuer <a href="get-offers-by-issuer" id="get-offers-by-issuer"></a>

Returns all the offers listed by an account.

```
get_offers_by_issuer(string issuer_account_id_or_name, uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```
get_offers_by_issuer account02 10 1.29.0
```

* `account02` issuer
* `10` Number of items per fetch (Pagination)
* `1.29.0` Index to start the search for. (pagination)

Example command without pagination,

```
get_offers_by_issuer account02 10 null
```



#### Get Offers by NFT Item <a href="get-offers-by-nft-item" id="get-offers-by-nft-item"></a>

```
get_offers_by_item(const nft_id_type item, uint32_t limit, optional<offer_id_type> lower_id)
```

Example command,

```
get_offers_by_item 1.31.0 10 1.29.1
```

* `1.31.0` NFT Id
* `10` Number of items per fetch (Pagination)
* `1.29.1` Index to start the search for. (pagination)

Example command without pagination,

```
get_offers_by_item 1.31.0 10 null
```



#### Get Offer History by Issuer <a href="get-offer-history-by-issuer" id="get-offer-history-by-issuer"></a>

```
get_offer_history_by_issuer(string issuer_account_id_or_name, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example command,

```
get_offer_history_by_issuer account06 10 2.24.1
```

* account06 Issuer
* 10 Number of items per fetch (Pagination)
* 2.24.1 Index to start the search for. (pagination)

Example command without pagination,

```
get_offer_history_by_issuer account06 10 null
```





#### Get Offer History by NFT Item <a href="get-offer-history-by-nft-item" id="get-offer-history-by-nft-item"></a>

```
get_offer_history_by_item(const nft_id_type item, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example Command,

```
get_offer_history_by_item 1.31.0 10 2.24.1
```

* `1.31.0` NFT Id
* `10` Number of items per fetch (Pagination)
* `2.24.1` Index to start the search for. (pagination)

Example command without pagination,

```
get_offer_history_by_item 1.31.0 10 null
```





#### Get Offer History by Bidder <a href="get-offer-history-by-bidder" id="get-offer-history-by-bidder"></a>

```
get_offer_history_by_bidder(string bidder_account_id_or_name, uint32_t limit, optional<offer_history_id_type> lower_id)
```

Example Command,

```
get_offer_history_by_bidder account07 10 2.24.1
```

* account07 Bidder
* 10 Number of items per fetch (Pagination)
* 2.24.1 Index to start the search for. (pagination)\
  ****

Example command without pagination,

```
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



#### Create Custom Permissions <a href="create-custom-permissions" id="create-custom-permissions"></a>

```
 create_custom_permission(string owner, string permission_name, authority auth, bool broadcast)
```

* owner Owner of the account who is creating the custom permission
* permission\_name Permission name eg. nftcreatepermission
* auth authority is account authority, more info at [permissions](https://peerplays.atlassian.net/wiki/spaces/EP/pages/197466275/Reverse+Engineering+-+Peerplaysjs-lib), [public/private keys](https://peerplays.atlassian.net/wiki/spaces/EKB/pages/197460303), [multi authority](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/66519041/Compare+of+Master5050+and+Hardfork+Code)

Example Command,

```
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



#### Get Custom Permissions <a href="get-custom-permissions" id="get-custom-permissions"></a>

```
get_custom_permissions(string owner)
```

Example Command,

```
get_custom_permissions account01
```

#### &#x20;<a href="update-custom-permissions" id="update-custom-permissions"></a>

#### Update Custom Permissions <a href="update-custom-permissions" id="update-custom-permissions"></a>

```
update_custom_permission(string owner, custom_permission_id_type permission_id, fc::optional<authority> new_auth, bool broadcast)
```

Example Command,

```
update_custom_permission account01 1.27.0 { "weight_threshold": 1,  "account_auths": [["1.2.53",1]], "key_auths": [], "address_auths": [] } true
```

Here we removed the `key_auths` and added `1.2.53` with weight 1, which is equal to `weight_threshold` , so `1.2.53` can alone sign the transaction successfully.



#### Create Custom Account Authority <a href="create-custom-account-authority" id="create-custom-account-authority"></a>

> Creating custom authority maps the created custom permissions with the actual operations present on the blockchain.
>
> It also has expiry time by when this custom permission is no more valid on any given account and operation combination.

```
create_custom_account_authority(string owner, custom_permission_id_type permission_id, int operation_type, fc::time_point_sec valid_from, fc::time_point_sec valid_to, bool broadcast)
```

Example Command,

```
create_custom_account_authority account01 1.27.0 0 "2020-11-02T17:53:25" "2020-12-03T17:53:25" true
```

`account01` is the owner of the account and the one who created a permission `1.27.0`

`1.27.0` is the custom permission created

`0` is the operation number, refer to operations at [operations](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/329351829/Peerplays+Alice+Chain+API+Endpoints+in+Development), here `0 `is `transfer_operation`

`"2020-11-02T17:53:25" `valid from timestamp

`"2020-12-03T17:53:25"` valid to timestamp

`true` broadcast

Basically this represents a full HRP where transfer operation on `account01` can be done by authorities present in `1.27.0` instead of account owner `account01`



#### Update Custom Account Authority <a href="update-custom-account-authority" id="update-custom-account-authority"></a>

Can be used to update existing `valid_from` and `valid_to` times,

```
update_custom_account_authority(string owner, custom_account_authority_id_type auth_id, fc::optional<fc::time_point_sec> new_valid_from, fc::optional<fc::time_point_sec> new_valid_to, bool broadcast)
```

Example command,

```
update_custom_account_authority account01 1.28.0 "2020-06-02T17:52:25" "2020-06-03T17:52:25" true
```





#### Delete Custom Permission <a href="delete-custom-permission" id="delete-custom-permission"></a>

Used to delete the existing custom permission, this will delete all the custom account authorities linked to this permission as well.( cascading delete )

```
delete_custom_permission(string owner, custom_permission_id_type permission_id, bool broadcast)
```

Example command,

```
delete_custom_permission account01 1.27.0 true
```





#### Delete Custom Account Authority <a href="delete-custom-account-authority" id="delete-custom-account-authority"></a>

Used to delete an account authority attached to a permission.

```
delete_custom_account_authority(string owner, custom_account_authority_id_type auth_id, bool broadcast)
```

Example command,

```
delete_custom_account_authority account01 1.28.0 true
```





### Resource Permissions ( Account Roles ) <a href="resource-permissions-account-roles" id="resource-permissions-account-roles"></a>

As opposed to HRP mentioned above, resource permissions are controlled by an owner of a resource (eg. NFT metadata is a resource)

These are similar to IAM permissions in AWS Cloud environment.



#### Create Account Role <a href="create-account-role" id="create-account-role"></a>

Used to create an account role.

```
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

`offer_operation`**(88),** Checks if the user who is listing an NFT for sale is in `whitelisted_accounts` , if not the user can’t list the NFTs in marketplace.

`bid_operation`**(89),** Checks if the user who is bidding for an NFT on sale is in `whitelisted_accounts` , if not the user can’t bid / buy the NFT from marketplace.

&#x20;`nft_safe_transfer_from_operation` **(95),** Checks if the user who transferring i.e. owner is in `whitelisted_accounts`, if yes it checks the to-account is also in the `whitelisted_accounts`. If any of the two checks fail, the transfer fails.



More operations like NFT Lottery, RNG are to be attached to account roles.

Example Command,

```
create_account_role account01 ar1 ar1 [89,95] [1.2.40, 1.2.41, 1.2.43] "2020-09-04T13:43:39" true
```

This command effectively limits any NFT to be sold or transferred between only three accounts, `1.2.40, 1.2.41, 1.2.43`

For attaching NFT metadata to an account role, please refer to the above NFT sections.



#### Get Account Roles <a href="get-account-roles" id="get-account-roles"></a>

```
get_account_roles_by_owner(string owner_account_id_or_name)
```

Example command,

```
get_account_roles_by_owner account01
```



#### Update Account Role <a href="update-account-role" id="update-account-role"></a>



As a resource owner, one can update the operations and whitelisted accounts present in an account role.

This helps in blacklisting any users from selling or transferring NFTs or any resources.

```
update_account_role(string owner_account_id_or_name, account_role_id_type role_id, optional<string> name, optional<string> metadata, flat_set<int> operations_to_add, flat_set<int> operations_to_remove, flat_set<account_id_type> accounts_to_add, flat_set<account_id_type> accounts_to_remove, optional<time_point_sec> valid_to, bool broadcast)
```

`operations_to_add` new operations to add to the account role

`operations_to_remove` existing operations to remove from the account role

`accounts_to_add` new accounts to add to the whitelist

`accounts_to_remove` existing accounts to remove from the whitelist



Example command,

```
update_account_role account01 1.32.0 null null [88] [95] [1.2.42] [1.2.40] "2020-09-04T13:52:38" true
```

`88` `offer_operation`**`(88)`** is added

`95` `nft_safe_transfer_from_operation` is removed

`1.2.42` account to the whitelist

`1.2.40` account removed from the whitelist





#### Delete Account Role <a href="delete-account-role" id="delete-account-role"></a>

Once account role is deleted, restrictions on resource access no longer work.

```
delete_account_role(string owner_account_id_or_name, account_role_id_type role_id, bool broadcast)
```

Example command,

```
delete_account_role account01 1.32.0 true
```



### Fee Considerations <a href="fee-considerations" id="fee-considerations"></a>



**PPY** is the core token of peerplays blockchain. Blockchain users can issue their own tokens with a conversion rate to core token.

In peerplays blockchain, every operation executed has a fee associated with it.

Majority of this fee goes as payment to witnesses that run the blockchain.

This fee is called transaction fee. Transaction fee is collected in core tokens only.

There is also a fee for storing any data on the blockchain like the metadata or names.

Currently for all the operations related to **NFT, Marketplace, HRP, Account roles,** the fee is equivalent to **1 PPY (Transaction) + 1 PPY per Kbyte (Storage)** of data stored on blockchain.

But these values are subject to change by committee members.



### NFT Notes <a href="nft-notes" id="nft-notes"></a>



* In order to create an NFT, a metadata has to be created first.
* Only the owner of the metadata can mint NFTs
* These NFTs can be assigned to any user of metadata owner’s choice.
* **NFT Metadata owner** is the owner of the metadata that mints the NFTs, **NFT owner** is the one who is assigned an NFT by metadata owner after minting.
* User who is not the metadata owner cannot mint NFTs to himself
* `is_transferable` and `is_sellable` flags configured on metadata control the transfer and selling properties of all the NFTs minted from the metadata.
* If `is_transferable` is false and `is_sellable` is true, then the NFT owner can only list it in marketplace and sell it. He can’t transfer to another user.
* If `is_transferable` is true and `is_sellable` is false, then the NFT owner cannot list it in marketplace and he can freely transfer it to other users.
* If `is_transferable` is false and `is_sellable` is false, then the NFT owner can neither be transfer nor sell the NFT. It remains with him forever.



### Marketplace Notes <a href="marketplace-notes" id="marketplace-notes"></a>



* Peerplays blockchain operates the marketplace through `offer_operation`, `bid_operation`, `cancel_offer_operation`, `finalize_offer_operation` operations/smart contracts.
* Offers operate as an auction where max bid before the expiry time owns the NFT
* If max\_price is equal to min\_price in offer\_operation it acts as normal listing. Whoever bids the max\_price gets the NFT instantly.
* For a successful bid, `revenue_partner` gets the `revenue_split` percent of the bid. This is configured during NFT metadata creation. So for all the NFT sales that are based on the metadata, `revenue_partner` gets the cut.
* Currently peerplays stakeholders don’t get the cut, this will be implemented in the future.
* Currently there is only one marketplace i.e. Peerplays blockchain is providing a marketplace where any owner can sell his NFT based on the configuration of the parent metadata.
* Metadata owner can customise this marketplace in the UI to show only the NFTs minted from his own metadata to his DAPP users.
* So there are no multiple marketplaces on peerplays blockchain at the moment, only UI customisations are possible.
* Price can be set in terms of custom assets (tokens) in market place. In this case both offers and bids should be in the same asset (token). Eg. Offer price is in XCOIN token, then bid price should also be in XCOIN token.





### Usecases <a href="usecases" id="usecases"></a>

#### Movie Tickets (For Blockchain Savvy Users) <a href="movie-tickets-for-blockchain-savvy-users" id="movie-tickets-for-blockchain-savvy-users"></a>



* I’m the owner of a movie theatre where there is a premiere show for a movie Interstellar.
* I create NFT metadata for Interstellar movie that has basic info about the genre, running time, movie posters etc
* To maximise my profit and take a cut from the black market ticket sales, I created the metadata of the movie `is_transferable` to false and` is_sellable` to true
* I accept payment on the blockchain in multiple currencies namely PPY, XCOIN and BTC (or some off chain mechanism in FIAT currencies like USD)
* I mint the tickets as NFTs to the users who sent me the amount in the above currencies. Users can gain entry to the movie only with these NFT tickets.
* Users cannot transfer the tickets among themselves, they can only list them on marketplace.
* As the demand for tickets increase, listing price increase, so I get more cut for every ticket sold.



#### NFT Lottery with Geolocation restrictions (Account Roles/ Resource Permission use case) <a href="nft-lottery-with-geolocation-restrictions-account-roles-resource-permission-use-case" id="nft-lottery-with-geolocation-restrictions-account-roles-resource-permission-use-case"></a>



* I’m the issuer of the lottery which has geo restrictions saying the lottery can only be sold to residents of the state New South Wales(NSW) in Australia.
* I do the KYC check off chain and list down all the users on the blockchain that are residents of the state.
* While creating NFT metadata I attach the account roles with the whitelist of users along with `is_transferable` to true and `is_sellable` to false.
* I create a smart contract for minting NFTs from the metadata created.
* This smart contract accesses the account role associated with the metadata.
* If any user is not whitelisted he can’t purchase the NFT lottery ticket from the smart contract.
* If the user is whitelisted and minted a lottery ticket, he can’t transfer it to non-whitelisting users who are not residents of NSW.
* I create a draw and declare the owner of NFT as winner.





#### University Degrees <a href="university-degrees" id="university-degrees"></a>



* I’m the university issuing degrees to students that complete masters degree in two streams science and arts.
* I create two NFT metadata one each for both Science and Arts.
* I make both `is_transferable` to false and `is_sellable` to false.
* I mint NFT accordingly based on the stream to each student.
* Each NFT (degree) has the info about the student grades, courses etc.
* These NFTs (degrees) stays with the students (users) forever, they can’t be transferred or sold.



#### Minting NFTs (For not so Blockchain Savvy Users) <a href="minting-nfts-for-not-so-blockchain-savvy-users" id="minting-nfts-for-not-so-blockchain-savvy-users"></a>



* I’m the owner of an NFT Metadata and my DAPP users are not blockchain savvy.
* I mint the NFTs to my account itself.
* I modify the URI of NFTs accordingly off chain to denote the ownership.

\


### Example NFT creation <a href="example-nft-creation" id="example-nft-creation"></a>

Core coin : PPY for fee

Purchase/Sell/Transaction token for NFTs : ava-token

PPY:ava-token = 1:10000
