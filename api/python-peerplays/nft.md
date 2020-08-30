---
description: Non Fungible Tokens
---

# NFT

#### Create NFT Meta

```text
p.nft_metadata_create(
    owner_account_id_or_name,        # owner of nft meta
    name,                            # nft meta name
    symbol,                          # nft symbol
    base_uri,                        # nft uri
    is_transferable=True,
    is_sellable=True,
    )
```



#### Update NFT Meta

```text
p.nft_metadata_update(
    owner_account_id_or_name,
    nft_metadata_id,
    name,
    symbol,
    base_uri,
    is_transferable=True,
    is_sellable=True,
    )
```



#### Mint NFT

```text
p.nft_mint(
    metadata_owner_account_id_or_name,
    metadata_id, 
    owner_account_id_or_name,
    approved_account_id_or_name,
    approved_operators,                     # list of operators
    token_uri,
    )
```



#### Transfer NFT

```text
p.nft_safe_transfer_from(
    operator_,                # operator account name or id
    from_,
    to_,
    token_id,
    data,                     # notes as string
    )
```



#### Approve Control over NFT

```text
def nft_approve(
    operator_,
    approved,         # approved account id or name
    token_id,
    )
```



#### Approve for all the tokens owned

```text
def nft_set_approval_for_all(
    owner,
    operator_,
    approved,
    )
```



#### Info calls

`p.rpc.nft_get_balance(owner)` 

`p.rpc.nft_owner_of(token_id)`

`p.rpc.nft_get_approved(token_id)`

`p.rpc.nft_is_approved_for_all(owner, operator)`

`p.rpc.nft_get_name(nft_metadata_id)`

`p.rpc.nft_get_symbol(nft_metadata_id)`

`p.rpc.nft_get_token_uri(token_id)`

`p.rpc.nft_get_total_supply(nft_metadata_id)`

`p.rpc.nft_token_by_index(nft_metadata_id, token_idx)`

`p.rpc.nft_token_of_owner_by_index(nft_metadata_id, owner, token_idx)`

`p.rpc.nft_get_all_tokens()`

`p.rpc.nft_get_tokens_by_owner(owner)`

