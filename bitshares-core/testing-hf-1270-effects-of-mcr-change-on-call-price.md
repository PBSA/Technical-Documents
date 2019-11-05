# Testing-HF-1270:-Effects-of-MCR-Change-on-Call-Price

## Overview

The instructions below are intended to assist with manual testing of changes made to address the issue of [inconsistent call price when MCR is changed](https://github.com/bitshares/bitshares-core/issues/1270) in [BitShares Core v.3.0.0](https://github.com/bitshares/bitshares-core/milestone/16?closed=1).

These test instructions should be executed from a command line interface wallet \(CLI\) that has been **built for your test environment**. For example, testing performed with the public testnet requires the CLI built for the [BitShares Public Testnet](https://testnet.bitshares.eu). The following instructions were executed on a private testing environment where BTS was the core token. These exact instructions may differ on your test environment in the following ways:

* The core token may be different than "BTS" \(e.g. "TEST"\).  Modify the commands to use your core token symbol.
* The account names that are created might already exist in your test environment.  Check for their existence by running `get_account ACCOUNT_NAME`.  Modify the commands to use alternate account names\).
* the asset names that are created might already exist in your test environment.  Check for the existence by running `get_asset ACCOUNT_NAME`.  Modify the commands to use alternate account names\).
* **The output shown in the results below are from a testnet from before HF-1270. The sample output often contains the undesired behavior that is being fixed.** Testing in a "fixed" environment should display the results that reflect the changed MCR as suggested at the end of Step 3 of [Scenario 1](testing-hf-1270-effects-of-mcr-change-on-call-price.md#scenario1-step3) and [Scenario 2](testing-hf-1270-effects-of-mcr-change-on-call-price.md#scenario2-step3).

### Unit Tests

For reference, the following unit tests that have been prepared to test the fixes \([1324](https://github.com/bitshares/bitshares-core/pull/1324),[1469](https://github.com/bitshares/bitshares-core/pull/1469),[1493](https://github.com/bitshares/bitshares-core/pull/1493)\) for the issue \([1270](https://github.com/bitshares/bitshares-core/issues/1270),[1423](https://github.com/bitshares/bitshares-core/issues/1423)\).

* [mcr\_bug\_increase\_before1270](https://github.com/bitshares/bitshares-core/blob/bf7ff54d9a17aa43f4663521e371b8c0ddfc2284/tests/tests/market_tests.cpp#L1541)
* [mcr\_bug\_increase\_after1270](https://github.com/bitshares/bitshares-core/blob/bf7ff54d9a17aa43f4663521e371b8c0ddfc2284/tests/tests/market_tests.cpp#L1606)
* [mcr\_bug\_decrease\_before1270](https://github.com/bitshares/bitshares-core/blob/bf7ff54d9a17aa43f4663521e371b8c0ddfc2284/tests/tests/market_tests.cpp#L1672)
* [mcr\_bug\_decrease\_after1270](https://github.com/bitshares/bitshares-core/blob/bf7ff54d9a17aa43f4663521e371b8c0ddfc2284/tests/tests/market_tests.cpp#L1742)

## Initialize the Test Environment

The following test scenarios portray the interaction of five actors: an account registrar issuer \("nathan"\), an asset issuer \("nathan"\), a feed publisher \("publisher-a"\), and two asset borrowers/traders \("trader-a" and "trader-b"\). Each will require their own wallet with their own keys. Certain steps must be performed by specific actors from their respective wallet. Each step of the instructions describe which actor is performing that step. The reader should use the respective actor's wallet.

| Tip |
| :--- |
| Testers who prefer convenience over strict separation of keys may prefer to keep the private keys of every actor in a single wallet. |

### 1. Registrar: Create a feed publisher

| Note |
| :--- |
| This step assumes that the registrar account is already created, and that the tester already has the keys of the registrar. |

Create the account of a publisher called "publisher-a"

```text
unlocked >>> suggest_brain_key
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...,
  "pub_key": "BTS5RP4bVx2utcnoq1J9eSDFFZu3izmMqvZtGors4ctL1ar9Hy8q9"
}
unlocked >>> suggest_brain_key 
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...",
  "pub_key": "BTS7HbpzSg89bKkuWhyCpd3V57QBTtRRWupLgVgDGwisikd9W4XD2"
}

register_account(string name, public_key_type owner, public_key_type active, string registrar_account, string referrer_account, uint32_t referrer_percent, bool broadcast)

register_account publisher-a BTS5RP4bVx2utcnoq1J9eSDFFZu3izmMqvZtGors4ctL1ar9Hy8q9 BTS7HbpzSg89bKkuWhyCpd3V57QBTtRRWupLgVgDGwisikd9W4XD2 nathan nathan 0 true
```

Give the single feed publisher some core tokens

```text
transfer nathan publisher-a 1000 BTS "" true
```

### 2. Issuer: Create the asset

Create a bitasset as described in the [reference instructions](bitshares-core.wiki/docs.bitshares.org/bitshares/tutorials/mpa-create-manual.html)

```text
create_asset "nathan" "TESTUSDA" 4 {"max_supply":"1000000000000000","market_fee_percent":0,"max_market_fee":"1000000000000000","issuer_permissions":161,"flags":0,"core_exchange_rate":{"base":{"amount":1,"asset_id":"1.3.0"},"quote":{"amount":1,"asset_id":"1.3.1"}},"whitelist_authorities":[],"blacklist_authorities":[],"whitelist_markets":[],"blacklist_markets":[],"description":"","extensions":[]} {} false
```

Specify who can publish feeds

```text
update_asset_feed_producers TESTUSDA ["publisher-a"] false

unlocked >>> update_asset_feed_producers TESTUSDA ["publisher-a"] true
update_asset_feed_producers TESTUSDA ["publisher-a"] true
{
  "ref_block_num": 23131,
  "ref_block_prefix": 1301299163,
  "expiration": "2019-03-12T21:18:55",
  "operations": [[
      13,{
        "fee": {
          "amount": 50000000,
          "asset_id": "1.3.0"
        },
        "issuer": "1.2.17",
        "asset_to_update": "1.3.1",
        "new_feed_producers": [
          "1.2.22"
        ],
        "extensions": []
      }
    ]
  ],
  "extensions": [],
    "signatures": [
        "201cd18934e5bfece04ca2f88f66e7832998db8f9214810eefdb6662d2519b17b73a469a5d404c90f3df1eacdcdb219882039756ea2e1ffdcc2ab52f9dccbde052"
  ]
}


unlocked >>> get_bitasset_data TESTUSDA
get_bitasset_data TESTUSDA
{
  "id": "2.4.0",
  "feeds": [[
      "1.2.22",[
        "1970-01-01T00:00:00",{
          "settlement_price": {
            "base": {
              "amount": 0,
              "asset_id": "1.3.0"
            },
            "quote": {
              "amount": 0,
              "asset_id": "1.3.0"
            }
          },
          "maintenance_collateral_ratio": 1750,
          "maximum_short_squeeze_ratio": 1500,
          "core_exchange_rate": {
            "base": {
              "amount": 0,
              "asset_id": "1.3.0"
            },
            "quote": {
              "amount": 0,
              "asset_id": "1.3.0"
            }
          }
        }
      ]
    ]
  ],
  "current_feed": {
    "settlement_price": {
      "base": {
        "amount": 0,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 0,
        "asset_id": "1.3.0"
      }
    },
    "maintenance_collateral_ratio": 1750,
    "maximum_short_squeeze_ratio": 1500,
    "core_exchange_rate": {
      "base": {
        "amount": 0,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 0,
        "asset_id": "1.3.0"
      }
    }
  },
  "current_feed_publication_time": "2019-03-12T21:18:25",
  "options": {
    "feed_lifetime_sec": 86400,
    "minimum_feeds": 1,
    "force_settlement_delay_sec": 86400,
    "force_settlement_offset_percent": 0,
    "maximum_force_settlement_volume": 2000,
    "short_backing_asset": "1.3.0",
    "extensions": []
  },
  "force_settled_volume": 0,
  "is_prediction_market": false,
  "settlement_price": {
    "base": {
      "amount": 0,
      "asset_id": "1.3.0"
    },
    "quote": {
      "amount": 0,
      "asset_id": "1.3.0"
    }
  },
  "settlement_fund": 0
}


unlocked >>> get_object 2.4.0
get_object 2.4.0
[{
    "id": "2.4.0",
    "asset_id": "1.3.1",
    "feeds": [[
        "1.2.22",[
          "1970-01-01T00:00:00",{
            "settlement_price": {
              "base": {
                "amount": 0,
                "asset_id": "1.3.0"
              },
              "quote": {
                "amount": 0,
                "asset_id": "1.3.0"
              }
            },
            "maintenance_collateral_ratio": 1750,
            "maximum_short_squeeze_ratio": 1500,
            "core_exchange_rate": {
              "base": {
                "amount": 0,
                "asset_id": "1.3.0"
              },
              "quote": {
                "amount": 0,
                "asset_id": "1.3.0"
              }
            }
          }
        ]
      ]
    ],
    "current_feed": {
      "settlement_price": {
        "base": {
          "amount": 0,
          "asset_id": "1.3.0"
        },
        "quote": {
          "amount": 0,
          "asset_id": "1.3.0"
        }
      },
      "maintenance_collateral_ratio": 1750,
      "maximum_short_squeeze_ratio": 1500,
      "core_exchange_rate": {
        "base": {
          "amount": 0,
          "asset_id": "1.3.0"
        },
        "quote": {
          "amount": 0,
          "asset_id": "1.3.0"
        }
      }
    },
    "current_feed_publication_time": "2019-03-12T21:18:25",
    "options": {
      "feed_lifetime_sec": 86400,
      "minimum_feeds": 1,
      "force_settlement_delay_sec": 86400,
      "force_settlement_offset_percent": 0,
      "maximum_force_settlement_volume": 2000,
      "short_backing_asset": "1.3.0",
      "extensions": []
    },
    "force_settled_volume": 0,
    "is_prediction_market": false,
    "settlement_price": {
      "base": {
        "amount": 0,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 0,
        "asset_id": "1.3.0"
      }
    },
    "settlement_fund": 0,
    "asset_cer_updated": false,
    "feed_cer_updated": false
  }
]

unlocked >>> get_object 2.3.1
get_object 2.3.1
[{
    "id": "2.3.1",
    "current_supply": 0,
    "confidential_supply": 0,
    "accumulated_fees": 0,
    "fee_pool": 250000000
  }
]
```

### 3. Feed Publisher: Publish initial feed

20 BTS per 1 USD, MCR 1.75, MSSR of 1.10 \(mimicing mcr\_bug\_increase\_before1270\)

= 20_10^5 satoshis of BTS per 1_  10^4 satoshis of USD = 2000000 BTS-satoshis per 10000 USD-satoshis

asset.cpp price\_feed::is\_for\(\) insists that the prices have the asset as the base asset

```text
publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":1750,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":1900000,"asset_id":"1.3.0"}}} true
{
  "ref_block_num": 23950,
  "ref_block_prefix": 437340707,
  "expiration": "2019-03-12T22:34:10",
  "operations": [[
      19,{
        "fee": {
          "amount": 100000,
          "asset_id": "1.3.0"
        },
        "publisher": "1.2.22",
        "asset_id": "1.3.1",
        "feed": {
          "settlement_price": {
            "base": {
              "amount": 10000,
              "asset_id": "1.3.1"
            },
            "quote": {
              "amount": 2000000,
              "asset_id": "1.3.0"
            }
          },
          "maintenance_collateral_ratio": 1750,
          "maximum_short_squeeze_ratio": 1100,
          "core_exchange_rate": {
            "base": {
              "amount": 10000,
              "asset_id": "1.3.1"
            },
            "quote": {
              "amount": 1900000,
              "asset_id": "1.3.0"
            }
          }
        },
        "extensions": []
      }
    ]
  ],
  "extensions": [],
  "signatures": [
        "1f7190ea8b60a9c3c22d43917c426abbd4134a5b7a95e65353eaa2b46d57c452916e9f1db9ffe4f7b81506e5eb0b01b6f163061c49ba8b477027c201d1b10443a6"
  ]
}

unlocked >>> get_bitasset_data TESTUSDA
get_bitasset_data TESTUSDA
{
  "id": "2.4.0",
  "feeds": [[
      "1.2.22",[
        "2019-03-12T22:33:40",{
          "settlement_price": {
            "base": {
              "amount": 10000,
              "asset_id": "1.3.1"
            },
            "quote": {
              "amount": 2000000,
              "asset_id": "1.3.0"
            }
          },
          "maintenance_collateral_ratio": 1750,
          "maximum_short_squeeze_ratio": 1100,
          "core_exchange_rate": {
            "base": {
              "amount": 10000,
              "asset_id": "1.3.1"
            },
            "quote": {
              "amount": 1900000,
              "asset_id": "1.3.0"
            }
          }
        }
      ]
    ]
  ],
  "current_feed": {
    "settlement_price": {
      "base": {
        "amount": 10000,
        "asset_id": "1.3.1"
      },
      "quote": {
        "amount": 2000000,
        "asset_id": "1.3.0"
      }
    },
    "maintenance_collateral_ratio": 1750,
    "maximum_short_squeeze_ratio": 1100,
    "core_exchange_rate": {
      "base": {
        "amount": 10000,
        "asset_id": "1.3.1"
      },
      "quote": {
        "amount": 1900000,
        "asset_id": "1.3.0"
      }
    }
  },
  "current_feed_publication_time": "2019-03-12T22:33:40",
  "options": {
    "feed_lifetime_sec": 86400,
    "minimum_feeds": 1,
    "force_settlement_delay_sec": 86400,
    "force_settlement_offset_percent": 0,
    "maximum_force_settlement_volume": 2000,
    "short_backing_asset": "1.3.0",
    "extensions": []
  },
  "force_settled_volume": 0,
  "is_prediction_market": false,
  "settlement_price": {
    "base": {
      "amount": 0,
      "asset_id": "1.3.0"
    },
    "quote": {
      "amount": 0,
      "asset_id": "1.3.0"
    }
  },
  "settlement_fund": 0
}
```

### 4. Create Borrowers/Traders

Give some accounts enough collateral asset to borrow the asset into existence, and to place orders for the asset

#### Trader A

**Trader A: Create keys**

```text
unlocked >>> suggest_brain_key 
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...",
  "pub_key": "BTS7B52d2ToTbG7hLqtCXznCrHfB1uHj1tfi3RnPX39MXDVcQvaxY"
}
unlocked >>> suggest_brain_key 
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...",
  "pub_key": "BTS7ubVaqr9sCcEn7RRE714UriqMXWxouu5AteEcYzgBg2XDJrWL1"
}
```

**Registrar: Register Trader A**

Register the account of trader-a

```text
register_account trader-a BTS7B52d2ToTbG7hLqtCXznCrHfB1uHj1tfi3RnPX39MXDVcQvaxY BTS7ubVaqr9sCcEn7RRE714UriqMXWxouu5AteEcYzgBg2XDJrWL1 nathan nathan 0 true
```

Transfer core tokens to trader-a

```text
transfer nathan trader-a 10000 BTS "" true
```

#### Create Trader B

**Trader B: Create keys**

```text
unlocked >>> suggest_brain_key 
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...",
  "pub_key": "BTS6fBqA4pGBXoorHQMcikspLwuZDRMdoXuZeUgsuRx6Ui6Rd3oVw"
}

unlocked >>> suggest_brain_key 
suggest_brain_key 
{
  "brain_priv_key": "...",
  "wif_priv_key": "...",
  "pub_key": "BTS5ez6PFW7gSnZmX2x18cX5swKL4S2TH3zPH51HUKXhVbNVD9UCk"
}
```

**Registrar: Register Trader B**

Register the account of trader-b

```text
register_account trader-b BTS6fBqA4pGBXoorHQMcikspLwuZDRMdoXuZeUgsuRx6Ui6Rd3oVw BTS5ez6PFW7gSnZmX2x18cX5swKL4S2TH3zPH51HUKXhVbNVD9UCk nathan nathan 0 true
```

Transfer core tokens to trader-b

```text
transfer nathan trader-b 5000 BTS "" true
```

## Scenario 1: Increased MCR should initiate a margin call

This scenario is similar to mcr\_bug\_increase\_before1270 and mcr\_bug\_increase\_after1270 where an increase of the MCR should make outstanding loans, which were initially well collateralized, to become undercollateralized and available for automatic sale of the collateral by the blockchain.

### 1. Trader A: Trader A Borrows at CR of 1.8

Trader A will borrow at CR of 1.8 using 1000 BTS. Given the published feed price of 20 BTS for 1 USD, 1000 BTS can purchase 27.78 USD --&gt; 1000000 BTS-satoshis for 277800 USD-satoshis.

```text
unlocked >>> borrow_asset trader-a 27.78 TESTUSDA 1000 true
borrow_asset trader-a 27.78 TESTUSDA 1000 true
1517481ms th_a       wallet.cpp:1975               borrow_asset         ] broadcast: true 
{
  "ref_block_num": 24514,
  "ref_block_prefix": 3807515098,
  "expiration": "2019-03-12T23:25:45",
  "operations": [[
      3,{
        "fee": {
          "amount": 2000000,
          "asset_id": "1.3.0"
        },
        "funding_account": "1.2.23",
        "delta_collateral": {
          "amount": 100000000,
          "asset_id": "1.3.0"
        },
        "delta_debt": {
          "amount": 277800,
          "asset_id": "1.3.1"
        },
        "extensions": []
      }
    ]
  ],
  "extensions": [],
  "signatures": [
"1f5cec798432e5297f1b26023506a77a1af6e6d7c8c63223adcb633d59e8f9e3706294f25b85a9e88b00ac10f2a18a91a3f312db56ce526a02e2f016a1a6c44593"
  ]
}

unlocked >>> list_account_balances trader-a

list_account_balances trader-a
8980 BTS
27.7800 TESTUSDA
```

### 2. Trader B: Trader B Borrows at CR of 2.0

Trader B will borrow at CR of 2.0 using 200 BTS. Given the published feed price of 20 BTS for 1 USD, 200 BTS can purchase 5 USD --&gt; 1000000 BTS-satoshis for 277800 USD-satoshis.

```text
unlocked >>> borrow_asset trader-b 5.0 TESTUSDA 200 true
borrow_asset trader-b 5.0 TESTUSDA 200 true
2139269ms th_a       wallet.cpp:1975               borrow_asset         ] broadcast: true 
{
  "ref_block_num": 24628,
  "ref_block_prefix": 1706587157,
  "expiration": "2019-03-12T23:36:05",
  "operations": [[
  3,{
    "fee": {
      "amount": 2000000,
      "asset_id": "1.3.0"
    },
    "funding_account": "1.2.24",
    "delta_collateral": {
      "amount": 20000000,
      "asset_id": "1.3.0"
    },
    "delta_debt": {
      "amount": 50000,
      "asset_id": "1.3.1"
    },
    "extensions": []
  }
]
  ],
  "extensions": [],
  "signatures": [
"1f4ee4b7bfb12db78c4f11554c5234613d73aca4439e11023cd84a1398d1e492d4604712a60cce8afffaac937c12b014e4fc4db5d253da7acb3c5d80761c320328"
  ]
}

unlocked >>> list_account_balances trader-b
list_account_balances trader-b
4780 BTS
5 TESTUSDA
```

### 3. Feed Publisher: Changes MCR to 2.0

The feed producer changes the MCR to 2.0 while not changing the price feed

```text
publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":2000,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":1900000,"asset_id":"1.3.0"}}} true
```

No changes will be visible by inspecting the order book with `get_order_book`

**Before HF1270**, no changes to the debt nor collateral of loans will be apparent by inspecting the loans with `get_call_orders`.

**After post-HF1270**, no changes to the debt nor collateral of loans will be apparent by inspecting the loans with `get_call_orders`. The `call_price` field of every loan will always appear with an effective price of 1.0 after the hardfork. The call\_price of the bitasset is now left as a calculation to client software to calculate. The call price of the bitasset denominated in the collateral token \(e.g. bitCNY/BTS\) should be calculated as

call\_price = \(collateral × MCR\) ÷ debt

### 4. Feed Publisher: Reset the MCR to 1.75

```text
publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":1750,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":1900000,"asset_id":"1.3.0"}}} true
```

## Scenario 2: Triggered Margin Calls Should Disappear because of MCR decrease

### Overview

Decreasing the MPA-price of the collateral is the same as increasing the Collateral-Price of the MPA

This scenario is similar to mcr\_bug\_decrease\_before1270 and mcr\_bug\_decrease\_after1270: the example calls for a two-steps:

1. The original to change from 1 USD exchanging for 1 BTS to 1 USD exchanging for 1.5 BTS \(i.e. the collateral loses 50% of its original USD-value\).  **This should trigger the initiation of a margin call.**

**Example**

If the original price is 20 BTS for 1 TESTUSDA then decreasing the collateral by 50% means that the new price feed should be 30 BTS for 1 TESTUSDA.

The first loan from the blockchain had a CR of \(1000 BTS  _{1 TESTUSDA / 20 BTS}\) / \(27.78 TESTUSDA\) = 1.80. After the price feed up from Step 1, it will only have a collateral ratio of \(1000 BTS_  {1 TESTUSDA / 30 BTS}\) / \(27.78 TESTUSDA\) = 1.20. Since the CR &lt; Stage 1 MCR of 1.75, a margin call should be triggered.

The second loan from the blockchain had a CR of \(200 BTS  _{1 TESTUSDA / 20 BTS}\) / \(5 TESTUSDA\) = 2.00. After the price feed up from Step 1, it will only have a collateral ratio of \(200 BTS_  {1 TESTUSDA / 30 BTS}\) / \(5 TESTUSDA\) = 1.33. Since the CR &lt; Stage 1 MCR of 1.75, a margin call should be triggered.

1. The original MCR is reduced from 1.75 to 1.1.  **This should cancel the initiated margin call.**

**Example**

The first loan's CR of 1.20 &gt; Stage 2 MCR which should cancel the unfilled margin call.

The second loan's CR of 1.33 &gt; Stage 2 MCR which should cancel the unfilled margin call.

### 1. Feed Publisher: Change the feed price

The initial published price is 20 BTS for 1 TESTUSDA. This should be changed such that the collateral is worth 50% less which means that the new price feed should be 30 BTS for 1 TESTUSDA.

30 BTS per 1 USD = 30_10^5 satoshis of BTS per 1_  10^4 satoshis of USD = 3000000 BTS-satoshis per 10000 USD-satoshis

The CER will also be adjusted to 95% of the feed price: 28.5 BTS per 1 USD = 2850000 BTS-satoshis per 10000 USD-satoshis

```text
publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":3000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":1750,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2850000,"asset_id":"1.3.0"}}} true
```

Check the order book to see for order arising from the initiation of margin calls on the two loans

```text
unlocked >>> get_call_orders TESTUSDA 3
get_call_orders TESTUSDA 3
[{
    "id": "1.8.0",
    "borrower": "1.2.23",
    "collateral": 100000000,
    "debt": 277800,
    "call_price": {
      "base": {
        "amount": 2000000,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 9723,
        "asset_id": "1.3.1"
      }
    }
  },{
    "id": "1.8.1",
    "borrower": "1.2.24",
    "collateral": 20000000,
    "debt": 50000,
    "call_price": {
      "base": {
        "amount": 1600,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 7,
        "asset_id": "1.3.1"
      }
    }
  }
]
```

**Before**

```text
list_account_balances trader-a
8980 BTS
27.7800 TESTUSDA


unlocked >>> list_account_balances trader-b
list_account_balances trader-b
4780 BTS
5 TESTUSDA
```

**After**

```text
list_account_balances trader-a
8980 BTS
27.7800 TESTUSDA


list_account_balances trader-b
4940 BTS
0 TESTUSDA


get_call_orders TESTUSDA 5
[{
    "id": "1.8.0",
    "borrower": "1.2.23",
    "collateral": 83500000,
    "debt": 227800,
    "call_price": {
      "base": {
        "amount": 1670000,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 7973,
        "asset_id": "1.3.1"
      }
    }
  },{
    "id": "1.8.1",
    "borrower": "1.2.24",
    "collateral": 20000000,
    "debt": 50000,
    "call_price": {
      "base": {
        "amount": 1600,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 7,
        "asset_id": "1.3.1"
      }
    }
```

The order had a fee of 5 BTS

```text
2019-03-13T12:54:05 fill_order_operation trader-b fee: 0 BTS 
2019-03-13T12:54:05 limit_order_create_operation trader-b fee: 5 BTS   result: 1.7.0 


100000000 BTS-satoshis - 83500000 BTS-satoshis = 1000.00000 - 835.00000 BTS = 165 BTS --> 33 BTS per 1 USD which is the median published price of 30 BTS per 1 USD times the MSSR of 1.1
```

The sale completed and is not sitting on the book. Trader B sold his MPA \(5 TESTUSDA\) in exchange for 160 BTS + 5 BTS \(fee\).

### 2. Feed Publisher: Change the MCR ratio

```text
publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":3000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":1100,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2850000,"asset_id":"1.3.0"}}} true
```

### 3. Trader B: Place an order

```text
unlocked >>> list_account_balances trader-a
list_account_balances trader-a
8980 BTS
27.7800 TESTUSDA

unlocked >>> list_account_balances trader-b
list_account_balances trader-b
4720 BTS
5 TESTUSDA
```

Place a 25-second fill-or-kill order to sell 5 USD at the current feed price of 30 BTS for 1 USD

```text
sell_asset trader-b 5 TESTUSDA 150 BTS 25 true false
```

Checking the account history of trader-b

```text
get_account_history trader-b 5
2019-03-13T12:54:05 fill_order_operation trader-b fee: 0 BTS 
2019-03-13T12:54:05 limit_order_create_operation trader-b fee: 5 BTS   result: 1.7.0
```

The limit order, which had a fee of 5 BTS, was filled immediately. Yet this alone does not indicate how much of the order was filled.

Check the balances after the sell order is placed.

```text
unlocked >>> list_account_balances trader-a                      
list_account_balances trader-a
8980 BTS
27.7800 TESTUSDA
```

There is no change to the liquid balance of `trader-a`.

```text
unlocked >>> list_account_balances trader-b
list_account_balances trader-b
4880 BTS
0 TESTUSDA
```

The account of trader-b has 0 TESTUSDA and his balance of BTS has increased to 4880. The net increase of BTS is 160 after accounting for the order fee fee of 5 BTS. The sale was for 165 BTS for 5 TESTUSDA which is equivalent to 33 BTS for 1 TESTUSDA. That 33 BTS per 1 TESTUSDA is the MSSR price which equals the median feed price of 30 BTS per 1 TESTUSDA multiplied by the MSSR of 1.1. The 165 BTS were withdrawn not from the liquid account of tester-a but instead from tester-a's loan collateral which was previously at 835 BTS and backing 22.78 TESTUSDA. Its equivalent is 83500000 BTS-satoshis and 227800 TESTUSDA-satoshis. After the margin call is partly filled by matching with the limit order that was introduced by tester-b, that loan collateral is reduced to 670 BTS and with the outstanding debt reduced to 17.78 TESTUSDA. See Loan 1.8.0 in the output of `get_call_orders`.

```text
unlocked >>> get_call_orders TESTUSDA 5                          
get_call_orders TESTUSDA 5
[{
    "id": "1.8.1",
    "borrower": "1.2.24",
    "collateral": 40000000,
    "debt": 100000,
    "call_price": {
      "base": {
        "amount": 1600,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 7,
        "asset_id": "1.3.1"
      }
    }
  },{
    "id": "1.8.0",
    "borrower": "1.2.23",
    "collateral": 67000000,
    "debt": 177800,
    "call_price": {
      "base": {
        "amount": 3350000,
        "asset_id": "1.3.0"
      },
      "quote": {
        "amount": 9779,
        "asset_id": "1.3.1"
      }
    }
  }
]
```

**Under pre-HF1270 code**, the order by trader-b was matched against an open margin call order against trader-a's loan even though it should not have been margin called because the MCR had been reduced and should have cancelled the margin call for that loan. If it had been cancelled it would not have been matched against trader-b's limit order.

**Under post-HF1270 code**, the order by trader-b should not be matched against trader-a's loan because trader-a's loan should no longer be callable.

### 4. Feed Publisher: Reset the MCR to 1.75 and the feed price to the initial feed price

Before exiting this scenario, clean up the state of the blockchain by re-publishing the initial feed price and MCR values.

```text
 publish_asset_feed publisher-a TESTUSDA {"settlement_price":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":2000000,"asset_id":"1.3.0"}},"maintenance_collateral_ratio":1750,"maximum_short_squeeze_ratio":1100,"core_exchange_rate":{"base":{"amount":10000,"asset_id":"1.3.1"},"quote":{"amount":1900000,"asset_id":"1.3.0"}}} true
```

Note that the loans by tester-a and tester-b are still outstanding. Cleaning them up will require the paying back of their loans.

