


Trading
========================

This page will give a very quick introduction on how to interpret the terms used by the DEX and how trading pairs are presented.

.. contents:: Table of Contents
   :local:

   
   
Pairs
-------------------------

In BitShares, almost any asset can be traded with all other assets. Once we have picked two assets, we usually refer to a *market pair*. For instance, we can trade USD against EUR in the USD:EUR pair.

For sake of consistency, we will use the generalized terms *base* and *quote* such that pairs are represented as::

    quote : base

and for instance with *base* being USD and *quote* being EUR, denote the EUR:USD pair.

Market Overview
-------------------------

The market overview that can be access via the explorer, shows a set of predefined default markets. Note that the list of default markets may vary depending on the wallet provider. Further markets can be added using the *Find Markets* tab. Adding a *Star* to your favorite markets will make it appear in your list of default markets.

.. image:: ../images/dex-trading-markets.png
        :alt: Market overview
        :width: 660px
        :align: center

Market
-------------------------

When entering a market, you will presented with either the market depth

.. image:: ../images/dex-trading-overview.png
        :alt: Trading market depth
        :width: 660px
        :align: center

... or the price chart depending on your settings.

.. image:: ../images/dex-trading-price-chart.png
        :alt: Trading price chart
        :width: 660px
        :align: center

You can switch between your views by pressing the corresponding button as highlighted below.

Order Books
-------------------------

The order book consists of an *ask* and a *bid* side. Since trading pairs do not have a preferred orientation, and can be flipped, the following table shall give an overview of ask/bid and the corresponding buy/sell operations for each side:

+------------+---------+---------+
| Side       | Sell    | Buy     | 
+============+=========+=========+
| Ask        | *quote* | *base*  |
+------------+---------+---------+
| Bid        | *base*  | *quote* |
+------------+---------+---------+

Obviously, what is on the bid side of the USD:EUR pair will be on the ask side on the EUR:USD pair. Of course prices are internally represented as fractions, and thus results in both pairs being identical.

Trading
-------------------------

To place a trading order, it is required to fill the form on either the *ask* or the *bid* side (respectively, *buy* or *sell* side). You will need to define a *price* and an *amount* to sell/buy. The cost for this order will be calculated automatically. Note that there will be an additional fee required to actually place the order.

.. image:: ../images/dex-trading-explained.png
        :alt: Individual areas of the trading dialog
        :width: 660px
        :align: center

Once the order is filled (i.e. someone sold/bought your offer), your account will be credited by the corresponding asset.

Unfilled orders can be canceled at any time.

Order Matching
-------------------------

BitShares 2.0 matches orders on a first-come, first-serve basis and gives the buyer the best price possible up to the limit (also known as "walking the book"). Rather than charging *unpredictable fees* from market overlap (as has been in the previous network), the network charges a defined fee based upon the size of the order matched and the assets involved. Each asset issuer gets an opportunity to configure their fees.

------

The decentralized exchange (DEX) of BitShares has a similar look&feel as traditional centralized exchanges. However, trading in the DEX can have many different appearances, depending on what user-interface is used. We here describe the user interface of the official wallet. 

.. _dex-playing-orders:

Playing Orders
-------------------------

Orders can be placed in the same way as everywhere else, by providing

* the amount to buy/sell
* the price at which to buy/sell

.. image:: ../images/dex-buysell.png
        :alt: Placing orders
        :width: 660px
        :align: center

Fees
-------------------------

In contrast to other exchanges, BitShares asks for a tiny **flat fee** for placing an order. This fee can be payed in USD, BTC, or GOLD and is independent of the actual assets that are traded.

If you cancel an order that has not been fully or partially filled, 90% of the fee will be payed back to your account. However, this chargeback will be in ``BTS`` and not in the asset you have originally paid the fee in.

|

|

|
