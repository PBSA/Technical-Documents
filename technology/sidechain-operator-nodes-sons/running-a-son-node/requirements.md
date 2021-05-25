# Requirements

Depending on the configuration, network, and other installed components on your server, the hardware requirements will vary. Here are the requirements of the most common configurations of SON nodes.

## Peerplays Mainnet

The following table lists what should be considered the minimum requirements for running a son node on **`Mainnet`**:

| Full Node? | SON Node? | Bitcoin node type | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Yes | No | n/a | 8 Cores | 64GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | Self-Hosted, Reduced Storage | 8 Cores | 64GB | 350GB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | Self-Hosted, Full Storage | 8 Cores | 64GB | 1TB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | External Bitcoin node | 8 Cores | 64GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | Self-Hosted, Reduced Storage | 2 Cores | 16GB | 350GB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | Self-Hosted, Full Storage | 2 Cores | 16GB | 1TB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | External Bitcoin node | 2 Cores | 16GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |

A Full node on Mainnet requires a baseline of: 8 CPU Cores, 64GB RAM, and 300GB storage. If you don't intend on running a Full node, but only a SON node, the baseline drops a little to: 2 CPU cores, 16GB RAM, and 300GB storage.

On top of this baseline, if you self-host a Bitcoin node with reduced storage, an additional 50GB storage is required. If you self-host a Bitcoin node with full storage, an additional 700GB storage is required.

Plus other considerations.

\(See notes about Full Nodes, SON Nodes, and Bitcoin node types\)

> **Note:** These requirements are as of the time of writing, so consider deploying a server with specs slightly higher than the ones listed above in order to "future proof" your server in case the minimum requirements grow in the future.

## Peerplays Testnet

The following table lists what should be considered the minimum requirements for running a son node on **`Testnet`**:

| Full Node? | SON Node? | Bitcoin node type | CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Yes | No | n/a | 4 Cores | 8GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | Self-Hosted, Reduced Storage | 4 Cores | 8GB | 350GB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | Self-Hosted, Full Storage | 4 Cores | 8GB | 1TB SSD | 1Gbps | Ubuntu 18.04 |
| Yes | Yes | External Bitcoin node | 4 Cores | 8GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | Self-Hosted, Reduced Storage | 2 Cores | 8GB | 350GB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | Self-Hosted, Full Storage | 2 Cores | 8GB | 1TB SSD | 1Gbps | Ubuntu 18.04 |
| No | Yes | External Bitcoin node | 2 Cores | 8GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |

A Full node on Testnet requires a baseline of: 4 CPU cores, 8GB RAM, and 300GB storage. If you don't intend on running a Full node, but only a SON node, the baseline drops a little to: 2 CPU cores, 8GB RAM, and 300GB storage.

On top of this baseline, if you self-host a Bitcoin node with reduced storage, an additional 50GB storage is required. If you self-host a Bitcoin node with full storage, an additional 700GB storage is required.

Plus [other considerations](requirements.md#considerations).

\(See notes about Full Nodes, SON Nodes, and Bitcoin node types\)

## Other Considerations

### Download and Upload Limits

In addition to the above, if you plan to operate a self-hosted Bitcoin node, you should look into getting an un-metered connection, a connection with high upload limits, or a connection you regularly monitor to ensure it doesn’t exceed its upload limits. It’s common for full Bitcoin nodes on high-speed connections to use 200 gigabytes upload or more per month. Download usage is around 20 gigabytes per month, plus around an additional 350 gigabytes the first time you start your node.

### Installing vs Operating

When installing nodes \(Peerplays or otherwise\) you may find it handy to provision a server with higher resources during the installation. Once your nodes are installed and synced with their networks you can then power the server down and provision it with lower resources to operate with. This is possible with cloud providers like Amazon AWS or Google Cloud. This can help speed up the installation process but cost less to run overall.

Sidechain Operator Node - An independent server operator which facilitates the transfer of off-chain assets \(like Bitcoin or Ethereum tokens\) between the Peerplays chain and the asset's native chain.

Nodes with an open RPC port. They provide complete transaction histories of all accounts accessible through API calls.

Just like Peerplays nodes, Bitcoin nodes can provide different levels of service. _Self-Hosted_ Bitcoin nodes are running on your own server and will therefore have a bigger impact on hardware requirements. _Reduced storage_ means the node doesn't save the entire Bitcoin chain. _Full storage_ means the node stores the whole Bitcoin chain \(almost 400GB and growing daily\). _External_ Bitcoin nodes are running on someone else's server. You may be able to connect to public or private Bitcoin nodes to run your SON node.

The live Peerplays environment, named Alice, is the publicly running blockchain on which all transactions take place.

One of any development environments for the Peerplays blockchain. The official public testnet, named Beatrice, is operated by the Peerplays witnesses. More testnets exist for development purposes like Gladiator for the testing of SONs.

