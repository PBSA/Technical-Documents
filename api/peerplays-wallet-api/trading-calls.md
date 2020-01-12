# Trading Calls

## Trading Calls

### sell\_asset

Place a limit order attempting to sell one asset for another.

Buying and selling are the same operation on Graphene; if you want to buy BTS with USD, you should sell USD for BTS.

The blockchain will attempt to sell the `symbol_to_sell` for as much `symbol_to_receive` as possible, as long as the price is at least `min_to_receive` / `amount_to_sell`.

In addition to the transaction fees, market fees will apply as specified by the issuer of both the selling asset and the receiving asset as a percentage of the amount exchanged.

If either the selling asset or the receiving asset is whitelist restricted, the order will only be created if the seller is on the whitelist of the restricted asset type.

Market orders are matched in the order they are included in the block chain.

```cpp
signed_transaction graphene::wallet::wallet_api::sell_asset(
    string seller_account, 
    string amount_to_sell, 
    string symbol_to_sell, 
    string min_to_receive, 
    string symbol_to_receive, 
    uint32_t timeout_sec = 0, 
    bool fill_or_kill = false, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`seller_account`**: the account providing the asset being sold, and which will receive the proceeds of the sale.
* **`amount_to_sell`**: the amount of the asset being sold to sell \(in nominal units\)
* **`symbol_to_sell`**: the name or id of the asset to sell
* **`min_to_receive`**: the minimum amount you are willing to receive in return for selling the entire amount\_to\_sell
* **`symbol_to_receive`**: the name or id of the asset you wish to receive
* **`timeout_sec`**: if the order does not fill immediately, this is the length of time the order will remain on the order books before it is cancelled and the un-spent funds are returned to the seller’s account
* **`fill_or_kill`**: if true, the order will only be included in the blockchain if it is filled immediately; if false, an open order will be left on the books to fill any amount that cannot be filled immediately.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction selling the funds.
{% endtab %}
{% endtabs %}

### borrow\_asset

Borrow an asset or update the debt/collateral ratio for the loan.

This is the first step in shorting an asset. 

Call [`sell_asset()`](trading-calls.md#sell_asset) to complete the short.

```cpp
signed_transaction graphene::
wallet
::
wallet_api
::borrow_asset(string borrower_name, string amount_to_borrow, string asset_symbol, string amount_of_collateral, bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`borrower_name`**: the name or id of the account associated with the transaction.
* **`amount_to_borrow`**: the amount of the asset being borrowed. Make this value negative to pay back debt.
* **`asset_symbol`**: the symbol or id of the asset being borrowed.
* **`amount_of_collateral`**: the amount of the backing asset to add to your collateral position. Make this negative to claim back some of your collateral. The backing asset is defined in the **`bitasset_options`** for the asset being borrowed.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction borrowing the asset
{% endtab %}
{% endtabs %}

#### [cancel\_order](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id45)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::cancel_order`\(object\_id\_type _order\_id_, bool _broadcast_ = false\)  


Cancel an existing order

**Return**

the signed transaction canceling the order**Parameters**

* `order_id`: the id of order to be cancelled
* `broadcast`: true to broadcast the transaction on the network

#### [settle\_asset](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id46)

signed\_transaction `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::settle_asset`\(string _account\_to\_settle_, string _amount\_to\_settle_, string _symbol_, bool _broadcast_ = false\)  


Schedules a market-issued asset for automatic settlement.

Holders of market-issued assests may request a forced settlement for some amount of their asset. This means that the specified sum will be locked by the chain and held for the settlement period, after which time the chain will choose a margin posision holder and buy the settled asset using the margin’s collateral. The price of this sale will be based on the feed price for the market-issued asset being settled. The exact settlement price will be the feed price at the time of settlement with an offset in favor of the margin position, where the offset is a blockchain parameter set in the global\_property\_object.

**Return**

the signed transaction settling the named asset**Parameters**

* `account_to_settle`: the name or id of the account owning the asset
* `amount_to_settle`: the amount of the named asset to schedule for settlement
* `symbol`: the name or id of the asset to settle
* `broadcast`: true to broadcast the transaction on the network

#### [get\_market\_history](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id47)

vector&lt;bucket\_object&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_market_history`\(string _symbol_, string _symbol2_, uint32\_t _bucket_, fc::time\_point\_sec _start_, fc::time\_point\_sec _end_\)_const_  


Get OHLCV data of a trading pair in a time range.

**Return**

A list of OHLCV data, in “least recent first” order.**Parameters**

* `symbol`: name or ID of the base asset
* `symbol2`: name or ID of the quote asset
* `bucket`: length of each time bucket in seconds.
* `start`: the start of a time range, E.G. “2018-01-01T00:00:00”
* `end`: the end of the time range

#### [get\_limit\_orders](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id48)

vector&lt;limit\_order\_object&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_limit_orders`\(string _a_, string _b_, uint32\_t _limit_\)_const_  


Get limit orders in a given market.

**Return**

The limit orders, ordered from least price to greatest**Parameters**

* `a`: symbol or ID of asset being sold
* `b`: symbol or ID of asset being purchased
* `limit`: Maximum number of orders to retrieve

#### [get\_call\_orders](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id49)

vector&lt;call\_order\_object&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_call_orders`\(string _a_, uint32\_t _limit_\)_const_  


Get call orders \(aka margin positions\) for a given asset.

**Return**

The call orders, ordered from earliest to be called to latest**Parameters**

* `a`: symbol name or ID of the debt asset
* `limit`: Maximum number of orders to retrieve

#### [get\_settle\_orders](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=set_voting_proxy#id50)

vector&lt;force\_settlement\_object&gt; `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_settle_orders`\(string _a_, uint32\_t _limit_\)_const_  


Get forced settlement orders in a given asset.

**Return**

The settle orders, ordered from earliest settlement date to latest**Parameters**

* `a`: Symbol or ID of asset being settled
* `limit`: Maximum number of orders to retrieve

