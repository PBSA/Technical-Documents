---
description: >-
  Hardware requirements for installing and operating one of the various
  Peerplays nodes.
---

# Hardware Requirements

Depending on the configuration, network, and other installed components on your server, the hardware requirements will vary. Here are the requirements of the most common configurations of Peerplays nodes.

## 1. Peerplays Mainnet

The following table lists what should be considered the minimum requirements for running a Peerplays node on **`Mainnet`**:

| Node Type? | CPU | Memory ⚠ | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Witness | 4 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| API \(Full\) | 4 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| BOS | 4 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| Seed | 2 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |
| SON ⚠  | 2 Cores | 16GB | 100GB SSD | 1Gbps | Ubuntu 18.04 |

{% hint style="danger" %}
**For all nodes:** The memory requirements shown in the table above are adequate to operate the node. Building and installing the node from source code \(as with the manual install\) will require more memory. You may run into errors during the build and install process if the system memory is too low. See [Installing vs Operating](hardware-requirements.md#4-2-installing-vs-operating) for more details. Using Docker or GitLab artifacts for installations don't have this limitation because they use pre-built binaries.

**For SONs:** See [section 3](hardware-requirements.md#3-son-sidechain-node-requirements) for details on sidechain node requirements.
{% endhint %}

A Witness node on Mainnet requires a baseline of: 4 CPU Cores, 16GB RAM, and 100GB storage. If you don't intend on running a Witness node, but only a SON or Seed node, the baseline drops a little to: 2 CPU cores, 16GB RAM, and 100GB storage.

As of June 2021, the Peerplays chain requires about 25GB of storage space \(on Mainnet\). The 100GB storage requirement is set high to account for increased chain usage over time.

* Plus [other considerations](requirements.md#other-considerations).
* See [notes](requirements.md#glossary) about Full Nodes, SON Nodes, and Bitcoin node types.

## 2. Peerplays Testnet

The following table lists what should be considered the minimum requirements for running a Witness node on **`Testnet`**:

| Node Type? | CPU | Memory ⚠ | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Witness | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| API \(Full\) | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| BOS | 4 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| Seed | 2 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |
| SON ⚠  | 2 Cores | 8GB | 50GB SSD | 1Gbps | Ubuntu 18.04 |

{% hint style="danger" %}
**For all nodes:** The memory requirements shown in the table above are adequate to operate the node. Building and installing the node from source code \(as with the manual install\) will require more memory. You may run into errors during the build and install process if the system memory is too low. See [Installing vs Operating](hardware-requirements.md#4-2-installing-vs-operating) for more details. Using Docker or GitLab artifacts for installations don't have this limitation because they use pre-built binaries.

**For SONs:** See section 3 for details on sidechain node requirements.
{% endhint %}

A Witness node on Testnet requires a baseline of: 4 CPU cores, 8GB RAM, and 50GB storage. If you don't intend on running a Witness node, but only a SON or Seed node, the baseline drops a little to: 2 CPU cores, 8GB RAM, and 50GB storage.

As of June 2021, the Peerplays chain requires about 20GB of storage space \(on Testnet\). The 50GB storage requirement is set high to account for increased chain usage over time, though not as much as Mainnet.

* Plus [other considerations](requirements.md#other-considerations).
* See [notes](requirements.md#glossary) about Full Nodes, SON Nodes, and Bitcoin node types.

## 3. SON Sidechain Node Requirements

SONs often run nodes for other chains to enable the sidechain functionality. These other nodes, like Bitcoin or Ethereum nodes, will require their own storage on top of what is required for Peerplays. It is recommended to research the requirements of any other nodes you may need to run to operate a SON.

### 3.1. Bitcoin Nodes

#### 3.1.1. Mainnet

The following table lists what should be considered the minimum requirements for running a Bitcoin SON on **`Mainnet`**: \(note the marked increase in storage requirements for self-hosting a Bitcoin node.\)

| Bitcoin Node Type | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Self-Hosted, Reduced Storage | 2 Cores | 16GB | 150GB SSD | 1 Gbps | Ubuntu 18.04 |
| Self-Hosted, Full Storage | 2 Cores | 16GB | 800GB SSD | 1 Gbps | Ubuntu 18.04 |
| External Bitcoin node | 2 Cores | 16GB | 100GB SSD | 1 Gbps | Ubuntu 18.04 |

A SON on Mainnet requires a baseline of: 2 CPU Cores, 16GB RAM, and 100GB storage \(as per section 1, above\). On top of this baseline, if you self-host a Bitcoin node with reduced storage, an additional 50GB storage is required. If you self-host a Bitcoin node with full storage, an additional 700GB storage is required.

#### 3.1.2. Testnet

The following table lists what should be considered the minimum requirements for running a Bitcoin SON on **`Testnet`**: \(note the marked increase in storage requirements for self-hosting a Bitcoin node.\)

| Bitcoin Node Type | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Self-Hosted, Reduced Storage | 2 Cores | 8GB | 100GB SSD | 1 Gbps | Ubuntu 18.04 |
| Self-Hosted, Full Storage | 2 Cores | 8GB | 750GB SSD | 1 Gbps | Ubuntu 18.04 |
| External Bitcoin node | 2 Cores | 8GB | 50GB SSD | 1 Gbps | Ubuntu 18.04 |

A SON on Testnet requires a baseline of: 2 CPU cores, 8GB RAM, and 50GB storage \(as per section 2, above\). On top of this baseline, if you self-host a Bitcoin node with reduced storage, an additional 50GB storage is required. If you self-host a Bitcoin node with full storage, an additional 700GB storage is required.

## 4. Other Considerations

### 4.1. Download and Upload Limits

In addition to the above, if you plan to operate a self-hosted Bitcoin node \(for SONs\), you should look into getting an unmetered connection, a connection with high upload limits, or a connection you regularly monitor to ensure it doesn’t exceed its upload limits. It’s common for full Bitcoin nodes on high-speed connections to use 200 gigabytes upload or more per month. Download usage is around 20 gigabytes per month, plus around an additional 350 gigabytes the first time you start your node.

### 4.2. Installing vs Operating

When installing nodes \(Peerplays or otherwise\) you may find it handy to provision a server with higher resources during the installation. Once your nodes are installed and synced with their networks you can then power the server down and provision it with lower resources to operate with. This is possible with cloud providers like Amazon AWS or Google Cloud. This can help speed up the installation process but cost less to run overall.

As an example, you could shoot the moon and start up a server with 8 CPU cores and 64GB memory to fly through the build and install process, then stop the server and pick an instance with a more reasonable 4 CPU cores and 16GB memory to run the node. In fact, if you have a [backup server](backup-servers.md) to manage such installs or updates, you can use this method of changing resources for all your server maintenance without service outages.

{% hint style="warning" %}
These requirements are as of the time of writing, so consider deploying a server with specs slightly higher than the ones listed above in order to "future proof" your server in case the minimum requirements grow in the future.
{% endhint %}

## 5. Glossary

**Witness:** An independent server operator which validates network transactions.

**Witness Node:** Nodes with a closed RPC port. They don't allow external connections. Instead these nodes focus on processing transactions into blocks.

**API Node:** Nodes with an open RPC port. They provide a gateway to blockchain functions by exposing the API.

**Full Node:** An API node which provides complete transaction histories of all accounts accessible through API calls.

**Seed Node:** Nodes that provide the ability for other nodes to download historical data.

**SON:** Sidechain Operator Node - An independent server operator which facilitates the transfer of off-chain assets \(like Bitcoin or Ethereum tokens\) between the Peerplays chain and the asset's native chain.

**Bitcoin node types:** Just like Peerplays nodes, Bitcoin nodes can provide different levels of service:

* _Self-Hosted_ Bitcoin nodes are running on your own server and will therefore have a bigger impact on hardware requirements.
  * _Reduced storage_ means the node doesn't save the entire Bitcoin chain.
  * _Full storage_ means the node stores the whole Bitcoin chain \(almost 400GB and growing daily\).
* _External_ Bitcoin nodes are running on someone else's server. You may be able to connect to public or private Bitcoin nodes to run your SON.

**Mainnet:** The live Peerplays environment, named **`Alice`**, is the publicly running blockchain on which all transactions take place.

**Testnet:** One of any development environments for the Peerplays blockchain. The official public testnet, named **`Beatrice`**, is operated by the Peerplays witnesses. More testnets exist for development purposes like Gladiator for the testing of SONs.

