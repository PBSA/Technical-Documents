---
description: An explanation of the many types of Peerplays nodes.
---

# Peerplays Node Types

## 1. Node Types

All Peerplays nodes keep updating an internal database by applying the transactions as they arrive in incoming blocks. The difference between the node types lies in the amount of history they keep track of, and in the functionality they provide.

### 1.1. Witness nodes

As the name implies, this is a node run by a Witness. Each Witness node validates all blocks and transactions it receives. The nodes of elected Witnesses take turns in bundling new transactions into blocks and broadcasting them to the network.

### 1.2. API nodes

API nodes provide network services to client applications. They usually have account transaction histories accessible through API calls, but can vary in the amount of available history. These nodes have an open RPC port to expose the API.

#### 1.2.1. Full nodes

A type of API node with a complete transaction history of all accounts.

### 1.3. Seed nodes

Seed nodes accept incoming P2P connections. They are the first nodes contacted by a freshly started node; the entry point into the network. Once a node has entered the network it will receive additional node addresses from its peers, so all nodes can connect to each other. A seed node can also be an API node. Seed nodes are not mandatory, but highly recommended.

### 1.4. BOS nodes

_Bookie Oracle System nodes_ - BOS nodes are required to operate the Bookie Oracle System to ensure the accuracy and decentralization of the data fed into the BookiePro application. The BOS node must be run on a separate server to the Witness node.

### 1.5. SONs

_Sidechain Operator Nodes_ - SONs facilitate the transfer of off-chain assets \(like Bitcoin, Hive, or Ethereum tokens\) between the Peerplays chain and the asset's native chain. These nodes often run the Peerplays node software and node software of other chains.

{% hint style="info" %}
The software used to run Witness, API \(full\), Seed, and SON nodes is named `witness_node`. All these node types are run with the same software. What makes these nodes different is how that software is configured and how it's used.

SONs will also require the use of software supplied by other chains, like Bitcoin Core for example.

BOS nodes use a collection of software known as the Bookie Oracle Suite.
{% endhint %}

## 2. Summary

| Node Type | Description | Open Ports | Can run together with |
| ---: | :--- | :---: | :---: |
| Witness | Elected by the community to produce blocks of validated transactions. | None | None |
| API \(Full\) | Provides an API gateway for apps to interact with the Peerplays chain. Full nodes offer the whole transaction history for all accounts. | RPC | Seed |
| Seed | Opening a P2P port allows new nodes to more readily perform the initial download of the Peerplays chain. | P2P | API \(Full\) |
| BOS | BOS nodes are whitelisted by Witnesses to feed data to the BookiePro app. | SSL | None |
| SON | Elected by the community to facilitate asset transfers between the Peerplays chain and sidechains. | Likely \(see note\) | None |

{% hint style="warning" %}
SONs most likely will be running other nodes \(like a Bitcoin node\) which may require opening ports to operate on the sidechain. It is because of this that SON nodes should not be run in parallel \(i.e. the same server\) with Witness nodes.
{% endhint %}

## 3. Node Requirements

Every Witness is required to run nodes on both Public Mainnet \(Alice\) and Public Testnet \(Beatrice\) environments.

The minimum node requirements include a Witness node and BOS node for both Testnet and Mainnet. If you also run a Seed Node, API Node, and SON Node then the number of required servers could be as many as ten.

## 4. References

* Node server [hardware requirements](hardware-requirements.md).
* Details about [Witness Nodes](../witnesses/installation-guides/).
* Details about [SON Nodes](../sidechain-operator-nodes-sons/installation-guides/).
* Details about [Joining the Public Testnet \(Beatrice\)](joining-the-public-testnet.md).

