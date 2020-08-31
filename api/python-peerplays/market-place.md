# Market Place

### Operations



#### Create Offer

```text
p.create_offer(
    item_ids,                  # list of items
    issuer_id_or_name,
    minimum_price,             # asset type
    maximum_price,             # asset type
    buying_item,               # bool
    offer_expiration_date,     # "2020-09-18T11:05:39"
    memo=None,                 # optional
    )
```

For example

```text
p.create_offer(["1.31.5"], "1.2.9", {"amount":5,"asset_id":"1.3.0"}, {"amount":15,"asset_id":"1.3.0"}, False, "2030-09-18T11:05:39", "") 
```



#### Bid

```text
p.create_bid(
    bidder_account_id_or_name,
    bid_price,                     # asset
    offer_id,                      # offer_id type, 1.29.x
    )
```

For example

```text
p.create_bid("1.2.10", {"amount":8,"asset_id":"1.3.0"}, offer["id"]) 
```



#### Cancel Offer

```text
p.cancel_offer(
    issuer_account_id_or_name,
    offer_id,                     # offer_id type, 1.29.x
    )
```

For example

```text
p.cancel_offer("1.2.9", offer["id"]) 
```

#### 

#### Calls for info

```text
p.rpc.list_offers(lower_id, limit)   # limt is number of entries requested as integer
p.rpc.list_sell_offers(lower_id, limit)
p.rpc.list_buy_offers(lower_id, limit)
p.rpc.list_offer_history(lower_id, limit)
p.rpc.get_offers_by_issuer(lower_id, issuer_account_id, limit)
p.rpc.get_offers_by_item(lower_id, nft_id_type_item, limit)
p.rpc.get_offer_history_by_issuer(lower_id, issuer_account_id, limit)
p.rpc.get_offer_history_by_item(lower_id, item, limit)
p.rpc.get_offer_history_by_bidder(lower_id, bidder_account_id, limit)
```



#### Additional Info

For examples, refer to  `tests/test_market_place.py`  

