# Market Place





```text
def create_offer(
    self,
    item_ids, #list of items
    issuer_id_or_name,
    minimum_price, #asset type
    maximum_price, #asset type
    buying_item, #bool
    offer_expiration_date, # "2020-09-18T11:05:39"
    memo=None, #optional
    **kwargs
    ):
```





```text
def create_bid(
    self,
    bidder_account_id_or_name,
    bid_price, # asset
    offer_id, # offer_id type, 1.29.x
    **kwargs
    ):
```





```text
def cancel_offer(
    self,
    issuer_account_id_or_name,
    offer_id, # offer_id type, 1.29.x
    **kwargs
    ):
```





```text
  vector<offer_object> list_offers(const offer_id_type lower_id, uint32_t limit) const;
  vector<offer_object> list_sell_offers(const offer_id_type lower_id, uint32_t limit) const;
  vector<offer_object> list_buy_offers(const offer_id_type lower_id, uint32_t limit) const;
  vector<offer_history_object> list_offer_history(const offer_history_id_type lower_id, uint32_t limit) const;
  vector<offer_object> get_offers_by_issuer(const offer_id_type lower_id, const account_id_type issuer_account_id, uint32_t limit) const;
  vector<offer_object> get_offers_by_item(const offer_id_type lower_id, const nft_id_type item, uint32_t limit) const;
  vector<offer_history_object> get_offer_history_by_issuer(const offer_history_id_type lower_id, const account_id_type issuer_account_id, uint32_t limit) const;
  vector<offer_history_object> get_offer_history_by_item(const offer_history_id_type lower_id, const nft_id_type item, uint32_t limit) const;
  vector<offer_history_object> get_offer_history_by_bidder(const offer_history_id_type lower_id, const account_id_type bidder_account_id, uint32_t limit) const;
```

