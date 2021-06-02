---
description: Hardware requirements for installing and operating a Witness node.
---

# Witness Node Hardware Requirements

Depending on the configuration, network, and other installed components on your server, the hardware requirements will vary. Here are the requirements of the most common configurations of Witness nodes.

## Peerplays Mainnet

The following table lists what should be considered the minimum requirements for running a witness node on **`Mainnet`**:

| Node Type? | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Full | 8 Cores | 64GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| API | 8 Cores | 64GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| Witness | 8 Cores | 64GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| Seed | 2 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| Son \(see note\) | 2 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |

A Full node on Mainnet requires a baseline of: 8 CPU Cores, 64GB RAM, and 100GB storage. If you don't intend on running a Full node, but only a SON or Seed node, the baseline drops a little to: 2 CPU cores, 16GB RAM, and 100GB storage.

As of June 2021, the Peerplays chain requires about 25GB of storage space \(on Mainnet\). The 100GB storage requirement is set high to account for increased chain usage over time.

> **For SONs:** SON nodes often run nodes for other chains to enable the sidechain functionality. These other nodes, like Bitcoin or Ethereum nodes, will require their own storage on top of what is required for Peerplays. It is recommended to research the requirements of any other nodes you may need to run to operate a SON.

Plus [other considerations](requirements.md#other-considerations).

\(See [notes](requirements.md#glossary) about Full Nodes, SON Nodes, and Bitcoin node types\)

> **Note:** These requirements are as of the time of writing, so consider deploying a server with specs slightly higher than the ones listed above in order to "future proof" your server in case the minimum requirements grow in the future.

## Peerplays Testnet

The following table lists what should be considered the minimum requirements for running a Witness node on **`Testnet`**:

| Node Type? | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Full | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| API | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| Witness | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| Seed | 2 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| Son \(see note\) | 2 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |

A Full node on Testnet requires a baseline of: 4 CPU cores, 8GB RAM, and 50GB storage. If you don't intend on running a Full node, but only a SON or Seed node, the baseline drops a little to: 2 CPU cores, 8GB RAM, and 50GB storage.

As of June 2021, the Peerplays chain requires about 20GB of storage space \(on Testnet\). The 50GB storage requirement is set high to account for increased chain usage over time, though not as much as Mainnet.

> **For SONs:** SON nodes often run nodes for other chains to enable the sidechain functionality. These other nodes, like Bitcoin or Ethereum nodes, will require their own storage on top of what is required for Peerplays. It is recommended to research the requirements of any other nodes you may need to run to operate a SON.

Plus [other considerations](requirements.md#other-considerations).

\(See [notes](requirements.md#glossary) about Full Nodes, SON Nodes, and Bitcoin node types\)

## Other Considerations

### Download and Upload Limits

In addition to the above, if you plan to operate a self-hosted Bitcoin node \(for SONs\), you should look into getting an unmetered connection, a connection with high upload limits, or a connection you regularly monitor to ensure it doesn’t exceed its upload limits. It’s common for full Bitcoin nodes on high-speed connections to use 200 gigabytes upload or more per month. Download usage is around 20 gigabytes per month, plus around an additional 350 gigabytes the first time you start your node.

### Installing vs Operating

When installing nodes \(Peerplays or otherwise\) you may find it handy to provision a server with higher resources during the installation. Once your nodes are installed and synced with their networks you can then power the server down and provision it with lower resources to operate with. This is possible with cloud providers like Amazon AWS or Google Cloud. This can help speed up the installation process but cost less to run overall.

## Glossary

**Witness:** An independent server operator which validates network transactions.

**Witness Node:** Nodes with a closed RPC port. They don't allow external connections. Instead these nodes focus on processing transactions into blocks.

**API Node:** Nodes with an open RPC port. They provide a gateway to blockchain functions by exposing the API.

**Full Node:** Nodes with an open RPC port. They provide complete transaction histories of all accounts accessible through API calls.

**Seed Node:** Nodes that provide the ability for other nodes to download historical data.

**SON Node:** Sidechain Operator Node - An independent server operator which facilitates the transfer of off-chain assets \(like Bitcoin or Ethereum tokens\) between the Peerplays chain and the asset's native chain.

**Mainnet:** The live Peerplays environment, named Alice, is the publicly running blockchain on which all transactions take place.

**Testnet:** One of any development environments for the Peerplays blockchain. The official public testnet, named Beatrice, is operated by the Peerplays witnesses. More testnets exist for development purposes like Gladiator for the testing of SONs.

