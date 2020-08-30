# NFT



```text
def nft_metadata_create(
    self,
    owner_account_id_or_name,
    name,
    symbol,
    base_uri,
    revenue_partner=None,
    revenue_split=200,
    is_transferable=True,
    is_sellable=True,
    **kwargs
    ):
```





```text
def nft_metadata_update(
    self,
    owner_account_id_or_name,
    nft_metadata_id,
    name,
    symbol,
    base_uri,
    revenue_partner=None,
    revenue_split=200,
    is_transferable=True,
    is_sellable=True,
    **kwargs
    ):
```





```text
def nft_mint(
    self,
    metadata_owner_account_id_or_name,
    metadata_id,
    owner_account_id_or_name,
    approved_account_id_or_name,
    approved_operators,
    token_uri,
    **kwargs
    ):
```





```text
def nft_safe_transfer_from(
    self,
    operator_,
    from_,
    to_,
    token_id,
    data,
    **kwargs
    ):
```





```text
def nft_approve(
    self,
    operator_,
    approved,
    token_id,
    **kwargs
    ):
```





```text
def nft_set_approval_for_all(
    self,
    owner,
    operator_,
    approved,
    **kwargs
    ):
```



nft\_get\_balance

uint64\_t database\_api::nft\_get\_balance\(const account\_id\_type owner\) const

optional database\_api::nft\_owner\_of\(const nft\_id\_type token\_id\) const



optional database\_api::nft\_get\_approved\(const nft\_id\_type token\_id\) const

bool database_api::nft\_is\_approved\_for\_all\(const account\_id\_type owner, const account\_id\_type operator_\) const

string database\_api::nft\_get\_name\(const nft\_metadata\_id\_type nft\_metadata\_id\) const

string database\_api::nft\_get\_symbol\(const nft\_metadata\_id\_type nft\_metadata\_id\) const

string database\_api::nft\_get\_token\_uri\(const nft\_id\_type token\_id\) const

uint64\_t database\_api::nft\_get\_total\_supply\(const nft\_metadata\_id\_type nft\_metadata\_id\) const

nft\_object database\_api::nft\_token\_by\_index\(const nft\_metadata\_id\_type nft\_metadata\_id, const uint64\_t token\_idx\) const

nft\_object database\_api\_impl::nft\_token\_of\_owner\_by\_index\(const nft\_metadata\_id\_type nft\_metadata\_id, const account\_id\_type owner, const uint64\_t token\_idx\) const

vector database\_api::nft\_get\_all\_tokens\(\) const

vector database\_api::nft\_get\_tokens\_by\_owner\(const account\_id\_type owner\) const







