# Peerplays Technical Summary

## Introducing Peerplays

Peerplays is a decentralized, provably fair global gaming platform, built on the most advanced blockchain technology available today. Peerplays brings a new paradigm of fairness, speed, transparency, and security to the global gaming industry.

Peerplays’ decentralization is based on the Delegated Proof of Stake \(DPoS\) consensus model, meaning that blocks are produced by a group of ‘Witness’ nodes which are elected by stake-weighted token holder voting. 

In addition to the Witnesses are the Advisors, a group of blockchain accounts, likewise elected by token holder voting, which vote to specify configurable blockchain parameters, and vote to include or reject proposed new features and other modifications to the consensus protocol.

Peerplays is a smart contracting platform specifically targeted at gaming contracts. 

{% hint style="warning" %}
Note: Peerplays is not a _Turing complete_ smart contracting platform, meaning that Peerplays does not support arbitrary, user-defined smart contracts; rather, it provides a well-defined set of officially maintained, built-in contracts. This is in contrast to _Turing complete_ smart contracting platforms like EOS or Ethereum, which provide few to no official contracts, but allow users to define and share contracts without any formally defined quality or correctness verification.
{% endhint %}

This document is intended to give new Peerplays developers an introduction to the Peerplays architecture and software. Readers are expected to be familiar with C++ software development in general, but not with Peerplays specifically. 

This document will walk readers through the code repository structure and how to build the software; describe the individual libraries and executables and their purposes; and examine how the contracts work and discuss how to create or modify Peerplays smart contracts.

## Peerplays Repository

The official BitShares repository can be found at:

{% embed url="https://github.com/peerplays-network/peerplays" %}

This repository uses git submodules, so be sure to fetch the submodules when cloning. This can be done by passing the `--recursive` flag when cloning:

```text
$ git clone https://github.com/peerplays-network/peerplays --recursive
```

The most significant subdirectories in the repository are `libraries`, `programs`, and `tests`. The Peerplays implementation is almost entirely defined within various libraries, which are located in the `libraries` subdirectory. 

The `programs` subdirectory contains small wrappers around these libraries, exposing their functionality as executable binaries.

 The `tests` subdirectory contains various tests to verify that essential blockchain features and functionality are working, and to detect regressions should they occur during development.

We'll now look at each of the three subdirectories in greater detail.

### The Peerplays Libraries

Peerplays is implemented in several `libraries` within the libraries subdirectory of the repository. A high level description of each of the libraries is as follows:

* `app` contains the `application` class, which implements the heart of a Peerplays node
* `chain` contains the bulk of the blockchain implementation, including all Peerplays specific functionality
* `db` contains the database functionality, implementing the in-memory database as well as the persistence layer
* `egenesis` is a small library which embeds the genesis block data into the binary
* `fc` is a library implementing many utility functionalities, including serialization, RPC, concurrency, etc.
* `net` contains the peer-to-peer networking layer of Peerplays
* `plugins` contains several plugin libraries which can be utilized within a Peerplays node
* `utilities` contains code and data necessary to Peerplays’ implementation, but not critical to the core functionality
* `wallet` contains the reference command-line wallet implementation

Of these libraries, the bulk of development activity occurs within the `chain` library, and sometimes `fc`. The other libraries remain reasonably stable, seeing comparatively small updates and modifications.

### The Peerplays Programs

Peerplays contains several programs, but only two of these are relevant to modern Peerplays development: `witness_node` and `cli_wallet.` In addition, the code within these folders exists just to expose library functionality in an executable, and is rarely updated. 

The `witness_node` program is the only maintained Peerplays node executable. The name `witness_node` is something of a misnomer, as this executable is really just a full node, but it can provide witness \(i.e., block producer\) functionality by loading the `witness` plugin. 

{% hint style="info" %}
**Tip**: If you wish to sync with the Peerplays blockchain network and maintain a database of the current chain state, this is the program to do it with.
{% endhint %}

The `cli_wallet` program implements a command-line wallet for Peerplays. It requires a network connection to a running `witness_node` to provide chain state information to it. This program provides a basic UI for all Peerplays functionality.

### The Peerplays Tests

Peerplays uses the Boost testing framework for its tests. Most of the Peerplays tests use the `database_fixture`, defined in `tests/common/database_fixture.hpp`, as the basis of the tests. This file also defines many macros and functions to reduce the boilerplate of test writing.

The bulk of the tests are written in the `tests/tests` folder, and are run by the `chain_test` binary. All tests of core functionality should be included in this directory and binary.

## Peerplays Smart Contracts

This section provides a high-level overview of the architecture of smart contracts in Peerplays, how they work, and how they are created.

At its essence, a Peerplays smart contract is comprised of three main types of object: 

* `operation`
* `evaluator` 
* `object` 

The Peerplays protocol defines a set of actions a user can take within the blockchain ecosystem, called `operation` s. All interactions with the blockchain take place through `operation` s, and in a sense, they are the blockchain’s API. 

Each `operation` has an `evaluator`, which implements that operation’s functionality within the Peerplays software implementation. Thus an `operation` is like a function prototype, whereas an `evaluator` is the function definition. 

Finally, all data persistently stored by the blockchain is contained within database `object` s. Each `object` defines a group of fields, analogous to columns of a relational database table.

### Operations

All `operation` s charge a fee to execute, and each must specify an account to pay the fee. This account’s ID must be returned by the `fee_payer()` method on the `operation`. Each `operation` must also provide a stateless consistency check which examines the `operation`’s fields and throws an exception if anything is invalid. 

Finally, `operation` s must provide a `calculate_fee()` method which examines the `operation` and calculates the fee to execute it. This method may not reference blockchain state, however, each `operation` defines a `fee_parameters_type` struct containing settings for the fee calculation defined at runtime, and an instance of this struct is passed to the `calculate_fee()` method.

All `operation` s automatically require the authorization of their fee paying account, but an `operation` may additionally specify other accounts which must authorize their execution by defining the `get_required_active_authorities()` and/or `get_required_owner_authorities()` methods. 

{% hint style="warning" %}
**Note**: If a transaction contains an `operation` which requires a given account’s authorization, signatures sufficient to satisfy that account’s authority must be provided on the transaction.
{% endhint %}

### Evaluators

Each `operation` has an `evaluator` which implements that `operation`’s modifications to the blockchain database. Each `evaluator` most provide two methods: `do_evaluate()` and `do_apply()`. 

The evaluate step examines the `operation` with read-only access to the database, and verifies that the `operation` can be applied successfully. The apply step then modifies the database. 

Each `evaluator` must also define a type alias, `evaluator::operation_type`, which aliases the specific `operation` implemented by that evaluator.

### Objects

The Peerplays software implementation utilizes a custom, in-memory relational-style database to track the blockchain state as new blocks and transactions are applied, containing `operation` s which modify the database. 

This database is implemented in the `libraries/db` folder, and it provides persistence to disk as well as undo functionality allowing the rewinding of changes, such as when a partially-applied transaction fails to execute, or blocks are popped due to a chain reorganization \(i.e. when switching forks\).

The Peerplays database tracks various `object` types, each of which defines the columns of a table. The rows of this table represent the individual object instances in the database. Along with each `object` type is an index type, which, in relational database terms, defines the primary and secondary keys, which can be used to look up object instances. 

The primary key is always an `object_id` type, a unique numerical ID for each object instance known to the blockchain. All `objects` inherit an `id` field from their base class which contains this ID. This field is set by the database automatically and does not need to be modified manually.

### Summary

Peerplays smart contracts are defined as a set of `operation` s which are analogous to API calls provided by the contract. 

These `operation` s are implemented by `evaluator` s, which provide code to verify that the operation can execute successfully, and then to perform the requisite modifications to database `object` s. 

All `object` s specify an index, which defines keys which can be used to look up an object instance within the database.

## Peerplays API\(s\)



