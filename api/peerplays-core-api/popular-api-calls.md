# Popular API Calls

## Overview

Some of the most interesting API calls for exchanges and gateways are listed in this document. 

We will now take a look at some sample outputs for some of the API calls.

## API Calls

### list\_account\_balances

List the balances of an account. Each account can have multiple balances, one for each type of asset owned by that account. The returned list will only contain assets for which the account has a nonzero balance.

```cpp
vector<asset> graphene::wallet::wallet_api::list_account_balances(
    const string &id)
```

{% tabs %}
{% tab title="Parameters" %}
* **`id`**: the name or id of the account whose balances you want
{% endtab %}

{% tab title="Return" %}
A list of the given account’s balances.
{% endtab %}

{% tab title="Script" %}
```javascript
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.list_account_balances("dan")
print(json.dumps(res,indent=4))
```
{% endtab %}

{% tab title="Result" %}
```javascript
[
    {
        "asset_id": "1.3.0",
        "amount": "331104701530"
    },
    {
        "asset_id": "1.3.511",
        "amount": 3844848635
    },
    {
        "asset_id": "1.3.427",
        "amount": 8638
    },
    {
        "asset_id": "1.3.536",
        "amount": 31957981
    }
]
```
{% endtab %}
{% endtabs %}

### transfer

Transfer an amount from one account to another.

```cpp
signed_transaction graphene::wallet::wallet_api::transfer(
    string from, 
    string to, 
    string amount, 
    string asset_symbol, 
    string memo, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the name or id of the account sending the funds
* **`to`**: the name or id of the account receiving the funds
* **`amount`**: the amount to send \(in nominal units to send half of a BTS, specify 0.5\)
* **`asset_symbol`**: the symbol or id of the asset to send
* **`memo`**: a memo to attach to the transaction. The memo will be encrypted in the transaction and readable for the receiver. There is no length limit other than the limit imposed by maximum transaction size, but transaction increase with transaction size
* **`broadcast`**: true to broadcast the transaction on the network

The final parameter _True_ states that the signed transaction will be broadcast. If this parameter is _False_ the transaction will be signed but not broadcast, hence not executed.
{% endtab %}

{% tab title="Return" %}
The signed transaction transferring funds.
{% endtab %}

{% tab title="Script" %}
```javascript
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.transfer("fromaccount","toaccount","10", "USD", "$10 gift", True);
print(json.dumps(res,indent=4))
```
{% endtab %}

{% tab title="Result" %}
```javascript
{
  "ref_block_num": 18,
  "ref_block_prefix": 2320098938,
  "expiration": "2015-10-13T13:56:15",
  "operations": [[
      0,{
        "fee": {
          "amount": 2089843,
          "asset_id": "1.3.0"
        },
        "from": "1.2.17",
        "to": "1.2.7",
        "amount": {
          "amount": 10000000,
          "asset_id": "1.3.0"
        },
        "memo": {
          "from": "GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
          "to": "GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
          "nonce": "16430576185191232340",
          "message": "74d0e455e2e5587b7dc85380102c3291"
        },
        "extensions": []
      }
    ]
  ],
  "extensions": [],
  "signatures": [
    "1f147aed197a2925038e4821da54bd7818472ebe25257ac9a7ea66429494e7242d0dc13c55c6840614e6da6a5bf65ae609a436d13a3174fd12f073550f51c8e565"
  ]
}
```
{% endtab %}
{% endtabs %}

### transfer2

This method works just like transfer, except it always broadcasts and returns the transaction ID \(hash\) along with the signed transaction.

```cpp
pair<transaction_id_type, signed_transaction> graphene::wallet::wallet_api::transfer2(
    string from, 
    string to, 
    string amount, 
    string asset_symbol, 
    string memo)
```

{% tabs %}
{% tab title="Parameters" %}
* **`from`**: the name or id of the account sending the funds
* **`to`**: the name or id of the account receiving the funds
* **`amount`**: the amount to send
* **`asset_symbol`**: the symbol or id of the asset to send
* **`memo`**: a memo to attach to the transaction. The memo will be encrypted in the transaction and readable for the receiver. There is no length limit other than the limit imposed by maximum transaction size, but transaction increase with transaction size
{% endtab %}

{% tab title="Return" %}
The transaction ID \(hash\) along with the signed transaction transferring funds
{% endtab %}

{% tab title="Script" %}
```javascript
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.transfer2("fromaccount","toaccount","10", "USD", "$10 gift");
print(json.dumps(res,indent=4))
```
{% endtab %}

{% tab title="Result" %}
```javascript
 [b546a75a891b5c51de6d1aafd40d10e91a717bb3,{
   "ref_block_num": 18,
   "ref_block_prefix": 2320098938,
   "expiration": "2015-10-13T13:56:15",
   "operations": [[
       0,{
         "fee": {
           "amount": 2089843,
           "asset_id": "1.3.0"
         },
         "from": "1.2.17",
         "to": "1.2.7",
         "amount": {
           "amount": 10000000,
           "asset_id": "1.3.0"
         },
         "memo": {
           "from": "GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
           "to": "GPH6MRyAjQq8ud7hVNYcfnVPJqcVpscN5So8BhtHuGYqET5GDW5CV",
           "nonce": "16430576185191232340",
           "message": "74d0e455e2e5587b7dc85380102c3291"
         },
         "extensions": []
       }
     ]
   ],
   "extensions": [],
   "signatures": [
     "1f147aed197a2925038e4821da54bd7818472ebe25257ac9a7ea66429494e7242d0dc13c55c6840614e6da6a5bf65ae609a436d13a3174fd12f073550f51c8e565"
   ]
 }
]
```
{% endtab %}
{% endtabs %}

### get\_account\_history

Returns the most recent operations on the named account.

This returns a list of operation history objects, which describe activity on the account.

```cpp
vector<operation_detail> graphene::wallet::wallet_api::get_account_history(
    string name, 
    int limit)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`name`**: the name or id of the account
* **`limit`**: the number of entries to return \(starting from the most recent\)
{% endtab %}

{% tab title="Return" %}
A list of `operation_history_objects.`
{% endtab %}

{% tab title="Script" %}
```javascript
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.get_account_history("dan", 1)
print(json.dumps(res,indent=4))
```
{% endtab %}

{% tab title="Result" %}
```javascript
[
     {
         "description": "fill_order_operation dan fee: 0 CORE",
         "op": {
             "block_num": 28672,
             "op": [
                 4,
                 {
                     "pays": {
                         "asset_id": "1.3.536",
                         "amount": 20000
                     },
                     "fee": {
                         "asset_id": "1.3.0",
                         "amount": 0
                     },
                     "order_id": "1.7.1459",
                     "account_id": "1.2.21532",
                     "receives": {
                         "asset_id": "1.3.0",
                         "amount": 50000000
                     }
                 }
             ],
             "id": "1.11.213277",
             "trx_in_block": 0,
             "virtual_op": 47888,
             "op_in_trx": 0,
             "result": [
                 0,
                 {}
             ]
         },
         "memo": ""
     }
 ]
```
{% endtab %}
{% endtabs %}

### get\_object

Returns the blockchain object corresponding to the given id.

This generic function can be used to retrieve any object from the blockchain that is assigned an ID. Certain types of objects have specialized convenience functions to return their objects e.g., assets have [`get_asset()`](asset-api.md), accounts have [`get_account()`](https://dev.bitshares.works/en/master/api/wallet_api.html#classgraphene_1_1wallet_1_1wallet__api_1ae4133a2fe8f63695385c20d327a88ff9), but this function will work for any object.

```cpp
variant graphene::wallet::wallet_api::get_object(
    object_id_type id)const
```

{% tabs %}
{% tab title="Parameters" %}
* **`id`**: the id of the object to return
{% endtab %}

{% tab title="Return" %}
The requested object.
{% endtab %}

{% tab title="Script" %}
```javascript
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.get_object("1.11.213277")
print(json.dumps(res,indent=4))
```
{% endtab %}

{% tab title="Result" %}
```javascript
{
    "trx_in_block": 0,
    "id": "1.11.213277",
    "block_num": 28672,
    "op": [
        4,
        {
            "fee": {
                "asset_id": "1.3.0",
                "amount": 0
            },
            "receives": {
                "asset_id": "1.3.0",
                "amount": 50000000
            },
            "pays": {
                "asset_id": "1.3.536",
                "amount": 20000
            },
            "account_id": "1.2.21532",
            "order_id": "1.7.1459"
        }
    ],
    "result": [
        0,
        {}
    ],
    "op_in_trx": 0,
    "virtual_op": 47888
}
```
{% endtab %}
{% endtabs %}

#### [`get_asset <USD>`](https://dev.bitshares.works/en/master/api/often-used-calls.html#id8)

**Script**

```text
import json
from grapheneapi import GrapheneAPI
client = GrapheneAPI("localhost", 8092, "", "")
res = client.get_asset("USD")
print(json.dumps(res,indent=4))
```

**Result**

```text
{
    "symbol": "USD",
    "issuer": "1.2.1",
    "options": {
        "description": "1 United States dollar",
        "whitelist_authorities": [],
        "flags": 0,
        "extensions": [],
        "core_exchange_rate": {
            "quote": {
                "asset_id": "1.3.536",
                "amount": 11
            },
            "base": {
                "asset_id": "1.3.0",
                "amount": 22428
            }
        },
        "whitelist_markets": [],
        "max_supply": "1000000000000000",
        "blacklist_markets": [],
        "issuer_permissions": 79,
        "market_fee_percent": 0,
        "max_market_fee": "1000000000000000",
        "blacklist_authorities": []
    },
    "dynamic_asset_data_id": "2.3.536",
    "bitasset_data_id": "2.4.32",
    "id": "1.3.536",
    "precision": 4
}
```

**Reference**extended\_asset\_object `graphene::`[`wallet`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6walletE)`::`[`wallet_api`](https://dev.bitshares.works/en/master/api/namespaces/wallet.html#_CPPv4N8graphene6wallet10wallet_apiE)`::get_asset`\(string _asset\_name\_or\_id_\)_const_  


Returns information about the given asset.**Return**

the information about the asset stored in the block chain**Parameters**

* `asset_name_or_id`: the symbol or id of the asset in question

