# quick-tips-for-devs

## Quick Tips for BitShares Developers

This guide is intended for developers who will be using the existing BitShares Core platform. Detailed documentation can be found at the following resources.

* [Developer documentation](https://dev.bitshares.works)
* [General Documentation](https://how.bitshares.works/)

Quick tips for common developer actions are linked to below.

**Table of Contents**

* [Accounts, Permissions, and Authorities](quick-tips-for-devs.md#accounts)
* [Standard Transactions](quick-tips-for-devs.md#standard-transactions)
* [Proposed Transactions](quick-tips-for-devs.md#proposed-transactions)
* [User Issued Assets](quick-tips-for-devs.md#user-issued-assets)
* [Vesting Balances](quick-tips-for-devs.md#vesting-balances)
* [Hashed Time-Lock Contracts](quick-tips-for-devs.md#htlc)
* [Software Tools](quick-tips-for-devs.md#software-tools)
* [Configuration for Testnet](quick-tips-for-devs.md#how-to-testnet)

## Accounts, Permissions, and Authorities

* [About Accounts](https://dev.bitshares.works/en/master/bts_guide/accounts/index_account.html)
* [About Account Permissions](https://dev.bitshares.works/en/master/bts_guide/accounts/bts_permissions.html)
* [About Dynamic Account Permissions](https://bitshares.org/technology/dynamic-account-permissions/)
* [About Multi-Signature Authorities](https://dev.bitshares.works/en/master/bts_guide/accounts/bts_multi-sign.html#bts-multi-sign)
* [How to change account authority thresholds \(GUI\)](https://dev.bitshares.works/en/master/bts_guide/accounts/bts_permissions.html#permissions-in-wallet-settings)
* [How to register an account](https://dev.bitshares.works/en/master/bts_guide/accounts/account-create.html#create-account-dev-cli)
* [How to import a GUI-wallet account into the CLI-wallet](https://dev.bitshares.works/en/master/bts_guide/tutorials/cli_import_guiwallet_account.html#howto-import-gui-wallet-account-cli)
* [How to generate a random key in the CLI](https://dev.bitshares.works/en/master/api/namespaces/wallet.html?highlight=suggest#classgraphene_1_1wallet_1_1utility_1a2c813fc0587d67ed483372ff38bb5273)

## Standard Transactions

Transactions allow multiple operations to be performed atomically within BitShares

* [How to construct a transaction](https://dev.bitshares.works/en/master/bts_guide/tutorials/construct-transaction.html#manually-construct-transaction) in the [CLI wallet](quick-tips-for-devs.md#CLI)
* [Tips about transactions](https://dev.bitshares.works/en/master/bts_guide/tutorials/index.html#transfer-transactions)

## Proposed Transactions

Proposed transaction are useful for performing actions on the blockchain that require approval of multiple accounts.

* [About Proposed Transactions](https://dev.bitshares.works/en/master/knowledge_base/trn_proposed_transactions.html#proposed-tran)
* [How to create a proposed transaction](https://dev.bitshares.works/en/master/bts_guide/tutorials/construct-transaction.html#manually-construct-transaction)
* [How to approve a proposed transaction](https://dev.bitshares.works/en/master/bts_guide/tutorials/propose-transaction.html#approving-a-proposal)

## User-Issued Assets \(UIA\)

* [About User-Issued Assets](https://dev.bitshares.works/en/master/bts_guide/tokens/uia.html)
* [How to create a UIA in the GUI](https://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-gui.html#creating-new-uia-gui)
* [How to create a UIA in the CLI](https://dev.bitshares.works/en/master/bts_guide/tutorials/uia-create-manual.html#uia-create-manual) \([Example](https://github.com/bitshares/bitshares-core/wiki/Testing-HF-1268:-Market-Fee-Sharing#create-and-issue-the-uia)\)
* [How to create parent.child assets](https://dev.bitshares.works/en/master/bts_guide/index_faq.html#what-about-parent-and-child-assets)
* [How to update an existing UIA](https://dev.bitshares.works/en/master/bts_guide/tutorials/uia-update-manual.html#uia-update-manual)
* [How can the issuer issue a UIA](https://github.com/bitshares/bitshares-core/wiki/Testing-HF-1268:-Market-Fee-Sharing#asset-owner-issue-the-asset)
* [How can the issuer retract a UIA](https://steemit.com/bitshares/@xeroc/how-the-issuer-of-an-iouuia-can-transfer-assets-back-to-himself)
* [How can the issuer burn a UIA \(CLI\)](https://dev.bitshares.works/en/master/api/wallet_api.html?highlight=burn#reserve-asset)

## Vesting Balances

* [About vesting balances](https://dev.bitshares.works/en/master/bts_guide/accounts/vesting_balances.html?highlight=vesting)
* [Example of vesting balances with CryptoBridge](https://crypto-bridge.org/2018/10/09/what-it-means-to-stake/)
* [How to create vesting balance](https://dev.bitshares.works/en/master/components/lib_operations.html?highlight=vesting#vesting-balance-create-operation)
* [How to check a vesting balance](https://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-list.html#list-vesting-balances)
* [Example of supplementing payouts for vesting balances](https://cryptobridge.freshdesk.com/support/solutions/articles/35000061225-how-will-funds-be-distributed-)
* [How to claim a vesting balance \(CLI\)](https://dev.bitshares.works/en/master/bts_guide/tutorials/vesting-claim.html#claiming-vesting-balance)
* [How to claim a vesting balance \(GUI Wallet\)](https://dev.bitshares.works/en/master/bts_guide/accounts/vesting_balances.html?highlight=vesting#claiming-a-vesting-balance)

## Hashed Time-Lock Contracts \(HTLC\)

* [How to create and redeem an HTLC \(CLI\)](https://github.com/bitshares/bitshares-core/wiki/HTLC)

## Software Tools

### API Calls

* [How to make one-off API calls](https://github.com/bitshares/bitshares-core/wiki/API)
* [About Remote Procedure Call API](https://dev.bitshares.works/en/master/api/rpc.html#rpc)
* [How to subscribe to object data with websockets](https://github.com/bitshares/bitshares-core/wiki/Websocket-Subscriptions)

### Software Clients and Libraries

* [Open Source GUI Wallets](https://github.com/bitshares/awesome-bitshares#opensource-wallets)
* Open Source Command Line Wallets
  * [CLI Wallet \(C++\)](https://dev.bitshares.works/en/master/development/index_cli.html)
  * [Uptick \(Python\)](https://github.com/bitshares/uptick)
* [Open Source Libraries](https://github.com/bitshares/awesome-bitshares#libraries)
* [Open Source Tools](https://github.com/bitshares/awesome-bitshares#tools-and-scripts)

## How to Configure Software for Testnet

### Reference Wallet

The [GUI reference wallet](https://github.com/bitshares/bitshares-ui), which is also [hosted by several parties](https://github.com/bitshares/awesome-bitshares#hosted-wallets), can be connected to any testnode under Settings.

### Command Line Interface Wallet \(CLI\)

* The [CLI Wallet \(C++\)](https://dev.bitshares.works/en/master/development/index_cli.html) must
  * have been built from the [testnet branch](https://github.com/bitshares)
  * be configured to point to a public API node by using the `-s` switch

```text
cli_wallet -s ws://<HOST_NAME_OR_IP>:<HOST_PORT>
```

or if using a secure connection

```text
cli_wallet -s wss://<HOST_NAME_OR_IP>:<HOST_PORT>
```

### Python Wallet

[Uptick](https://github.com/bitshares/uptick) is a Python-based CLI tool set for BitShares blockchain. Documentation can be found [here](http://uptick.rocks/).

It can be configured to connect to the public testnet by setting any testnet API node. For example

```text
uptick set node wss://testnet.nodes.bitshares.ws
```

### DEXBot

[DEXBot](http://dexbot.info/) is a market maker bot that can be configured to point to a testnet API node by leaving it as the only API node in the configuration file.

