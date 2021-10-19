# Peerplays Disaster Recovery Plan

The objective of this disaster recovery (DR) plan is to ensure that anyone can respond to a disaster or other emergency that affects the Peerplays Blockchain and minimize the effect on the operation of the blockchain. This document should be stored in a safe, accessible location off site.

## 1. Goals

The major goals of the disaster recovery plan:

* To minimize interruptions to normal operations.
* To limit the extent of disruption and damage.
* To minimize the economic impact of the interruption.
* To establish alternative means of operation in advance.
* To provide for smooth and rapid restoration of service.

## 2. Personnel

Node operators, Witnesses in particular, hold the power to keep the blockchain up and running. Given the decentralized nature of blockchain technology, active and standby node operators may not be known or reachable for communication. It is advisable to keep an up-to-date list of node operators and information on how to reach them to coordinate DR activities. The level of community participation of any particular node operator should be kept in mind while voting for active node operators. More communicative node operators help to ensure the chain can continue to run in the event of disaster.

For this DR plan, it is advisable to create and maintain a register of node operators. This register should be maintained and regularly updated with node operators, a reliable means of communicating with each of them, and running seed node addresses.

PBSA employees and contractors should be registered with their screen name, PBSA issued email, and communication channel handles as a communications roster. This roster should be publicly available and used for DR related communications and notifications.

## 3. Application Profiles

### 3.1. Peerplays Mainnet

#### Source Code

The Peerplays chain and all its plugins is stored in GitLab and GitHub. Build artifacts are also stored in GitLab.

#### Witness Nodes

The witness\_node program is distributed across many private (node operator) servers. Each node operator is responsible for maintaining their own copy of the Peerplays source code and builds. This is most often done on GitHub though individuals may store local copies and follow their own procedures.

The Peerplays database is synchronized among all running (active and inactive) witness nodes. This decentralized and distributed approach makes the database integrity and security extremely robust. Even if the mainnet has an outage, the database remains intact and duplicated across potentially dozens of individual servers globally.

### 3.2. Documentation

Documentation is stored on GitHub and GitBook. This includes all public documentation for community docs, developer docs, and infrastructure docs. Both GitHub and GitBook are third-party SaaS solutions. Other private or internal documentation for PBSA is stored in Confluence workspaces and Google Drive. These are also third-party SaaS solutions. For the purposes of this DR plan, it is assumed that these providers have sufficient recovery capabilities on their own and that recovery of our documentation is covered under their DR plans and protocols.

Documentation that is currently being worked on is sometimes stored on personal computers or workstations. In these cases, it is recommended to store or back-up such documents on a cloud solution such as Google Drive, GitBook, GitHub, etc.

#### GitBook.com Resources

* [https://docs.gitbook.com/resources/faq#where-and-how-is-my-data-stored](https://docs.gitbook.com/resources/faq#where-and-how-is-my-data-stored)
* Gitbook Support is available in the gitbook app. Once logged in, go to Spaces > "?" menu (lower left) > Contact Support

### 3.3. Source Code

Peerplays source code is stored and hosted on GitLab.com (a SaaS solution). The source code is then mirrored to GitHub.com (another SaaS solution). Since both GitLab and GitHub are third-party providers, for the purposes of this DR plan, it is assumed that these providers have sufficient recovery capabilities on their own and that recovery of our source code is covered under their DR plans and protocols.

#### GitLab.com Resources

* [https://about.gitlab.com/handbook/engineering/infrastructure/production/architecture/](https://about.gitlab.com/handbook/engineering/infrastructure/production/architecture/)
* [https://about.gitlab.com/handbook/engineering/infrastructure/faq/](https://about.gitlab.com/handbook/engineering/infrastructure/faq/)
* [https://status.gitlab.com/](https://status.gitlab.com)
* [https://about.gitlab.com/support/#contact-support](https://about.gitlab.com/support/#contact-support)

#### GitHub.com Resources

* [https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository](https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository)
* [https://www.githubstatus.com/](https://www.githubstatus.com)
* [https://support.github.com/request](https://support.github.com/request)

### 3.4. Peerplays.tech

#### Source Code

The website Peerplays.tech has its source code stored in GitHub. The website is static and requires no database or dynamic data. No data backup is required.

#### Hosting

Peerplays.tech is hosted on a PBSA VM. The following software is used to serve the website:

* **Nginx**: The server software.
* **Certbot**: Manages the SSL certificates.
* **Node.js**: Node.js serves the website through Nginx.
* **NPM**: is used to install required packages.
* **Next.js**: used to build the website from its source code.

### 3.6. Communication Channels

The following is a list of all the communication channels, where they are hosted, and what they are primarily used for.

| Communication Channel |   Hosting   | Uses                                                            |
| --------------------- | :---------: | --------------------------------------------------------------- |
| Email                 | Third-Party | Daily internal communications                                   |
| Telegram              | Third-Party | Community discussions, Group discussions (witnesses)            |
| Rocket Chat           |   On-Prem   | Internal and Community discussions, Group discussions, Meetings |
| Jitsi                 |   On-Prem   | Meetings                                                        |
| Twitter               | Third-Party | Social Media                                                    |
| Facebook              | Third-Party | Social Media                                                    |
| Instagram             | Third-Party | Social Media                                                    |

#### Telegram Resources

* [https://telegram.org/faq](https://telegram.org/faq)
* [https://telegram.org/faq#telegram-support](https://telegram.org/faq#telegram-support)

## 4. Hardware Profiles

### 4.1. Witness Nodes

Each witness has their own hardware required to run the witness\_node software, whether on prem or remote. For the purposes of this DR plan, we will assume that each witness has acquired the hardware to run backup servers in addition to their main production server. In general the hardware requirements for a production witness node are as follows:

* CPU = 4 Cores
* RAM = 16GB
* Storage = 100GB
* Bandwidth = 1Gbps
* Operating System = Ubuntu 18.04

**Note**: Building Peerplays on the machine from source code will require higher levels of RAM. It is recommended to download GitLab Artifacts for witness node installation.

### 4.2. Super-Node

A super-node is used to bootstrap the network if an outage occurs. The super-node needs to be able to handle block production for all active nodes and handle a limited number of transactions. Here is the minimum hardware requirements for a super-node:

* CPU = 32 Cores
* RAM = 50GB
* Storage = 400GB
* Bandwidth = 1Gbps
* Operating System = Ubuntu 18.04

### 4.3. Node Hardware Considerations

Server hardware may be on-prem or hosted by a cloud provider. Using a cloud-based virtual machine, like with Amazon EC2 or Google Cloud Platform, can be faster to provision in a DR scenario. Since super-nodes are only used temporarily to handle unexpected outages, going cloud-based is preferred and will be used in the protocols of this DR plan. DR scenarios that impact site operations might also make the use of on-prem hardware impractical or impossible.

## 5. Backup Procedures

All source code stored in GitLab is backed up by being mirrored to GitHub. Additionally, Peerplays is forked by witnesses on GitHub. The Peerplays database is synchronized and distributed among all node operators in the network. Witnesses maintain backup servers as per PBSA recommended guidelines in the witness node operator documentation.

## 6. Disaster Recovery Procedures

### 6.1. Mainnet Outage Recovery Using a Super-Node

If a scenario occurs which halts the chain, such as low node operator network participation, temporary witnesses must be used until the network regains stability. In cases like this, the Super-Node solution has been proven to work. Here are the steps required to recover the network:

1. A suitable (not corrupted or unintentionally forked) copy of the Peerplays database should be collected.
2. Provision a Super-Node (SN) server using a cloud provider VM.
   1. The SN should be firewalled such that no incoming nodes can sync with the SN.
   2. The database copy should be loaded to the SN.
   3. The latest Peerplays chain code should be installed on the SN.
   4. The SN should be configured with as many witness nodes as required to recover the network. (Init accounts and/or collaborating witness accounts)
   5. The SN should be replayed using the database.
   6. The SN should be producing blocks for most or all witnesses at this point.
3. Seed nodes should be created on the SN machine and synced with the block producing SN node.
4. The `cli_wallet` program on the SN can be used to update witness votes by collaborating voters. It can also allow witnesses to generate new signing keys.
5. Witnesses should be allowed through the firewall to begin syncing their independent nodes with the seed nodes.
   1. Restarted witness nodes should configure their nodes to use only seed nodes from the SN.
   2. They should also be configured with new signing keys so as not to interfere with block signing once synced.
   3. Block checkpoints should be established and configured.
   4. Witnesses can also use the backed up database from the SN to run replays.
6. Once the witnesses are synced, they can resume block production by swapping their signing keys with `update_witness`.

### 6.2. Communication Channel Recovery Using Telegram

If the on-prem servers go down, the Rocket Chat and Jitsi communication channels will be temporarily unavailable until the servers can come online again. Since our Telegram communication channel is hosted by a third-party and is also publicly available, communications during the outage should happen in Telegram. Email should not be effected by such an outage and can also be used for communication. Initially, all personnel should be notified of a communications channel outage via email and Telegram.

The Telegram channel used for this backup communication is available here: [https://t.me/Peerplays](https://t.me/Peerplays)

On-Prem servers can then be restored using their initial hardware and software specifications. When the on-prem communication channels have been restored, all personnel should be notified via email and Telegram.

## 7. Procedure for Truncating the Block Index

The blockchain database is located in the `witness_node_data_dir` folder in the following path: `./witness_node_data_dir/blockchain/database/block_num_to_block`

There should be two files here, `blocks` which contains the block data, and `index` which is the index used by the witness node program. To set the database back to a specific block number, a `truncate` operation must be done on the index file.

{% hint style="danger" %}
You cannot undo a `truncate` on a file. It's best to **always make a backup** of the file and to put the backup in a safe place **before performing a `truncate`**. Otherwise you could potentially corrupt your only copy of the database.
{% endhint %}

For each block in the database, the index file will contain the index information for the block which requires 32 bytes worth of space. First we'll find the current size of the index file and then divide that number by 32 to see how many blocks are currently stored in the database. Here are the steps (with an example, to illustrate):

1. Use the `ls -l` command in the folder with the database files to find the size (in bytes) of the `index` file.
2. Take the size of the `index` and divide by 32. The result is the number of currently indexed blocks.
3. Decide which block you should set the index back to (keeping in mind that each block represents about 3 seconds worth of time if you need to calculate the block number at a certain point in time.)
4. Take the new block number that you determined in step 3 and multiply that by 32. This will give you the new size that the `index` file must be. You're now ready to truncate.
5. Use the `truncate` command on the `index` file specifying the new file size that you calculated in step 4. The `truncate` command will return `0` if successful.

An Example:

```bash
cd ./witness_node_data_dir/blockchain/database/block_num_to_block

# Step 1
ls -l

drwxr-xr-x 2 bunker bunker       4096 Sep 13 04:01 ./
drwxr-xr-x 3 bunker bunker       4096 Sep 13 04:01 ../
-rw-r--r-- 1 bunker bunker 6982339677 Sep 13 10:54 blocks
-rw-r--r-- 1 bunker bunker 1386818464 Sep 13 10:54 index

# Step 2
# 1386818464 / 32 = 43,338,077 (last block # indexed)

# Step 3
# we should go back 10,000 blocks, so 43,338,077 - 10,000 = 43,328,077

# Step 4
# 43,328,077 * 32 = 1386498464

# Step 5
truncate --size=1386498464 ./index
```

## 8. Testing the Disaster Recovery Plan

A local testnet can be set up for testing using the standard procedure for [standing up a local testnet](https://infra.peerplays.tech/advanced-topics/private-testnets/private-testnets-manual-install). Once the testnet is running, it's possible to run various DR scenarios in a safe and controlled manner.

### 8.1 Testing Network Recovery Procedures

Testing a mainnet outage can be achieved by setting up a local testnet with two sets of witnesses and some voting accounts. To cause a chain halt:

1. The local testnet contains the entire network.
2. Vote for misconfigured witness accounts until they become active and eventually halt the chain.
3. Another testnet can be configured at this point to act as the Super-Node in this test.
4. The database can be recovered from the first testnet, processed to be suitable for a restart, and used in the second testnet.
5. The protocol in section 6.1. above can be used at this point to test the outage recovery scenario.

## 9. Example Config.ini File

Here is an example `config.ini` file which demonstrates the use of checkpoints:

```
# Endpoint for P2P node to listen on
p2p-endpoint = 0.0.0.0:9777

# P2P nodes to connect to on startup (may specify multiple times)
seed-node = 96.46.48.98:19777
seed-node = 96.46.48.98:29777
seed-node = 96.46.48.98:39777
seed-node = 96.46.48.98:49777
seed-node = 96.46.48.98:59777

# JSON array of P2P nodes to connect to on startup
seed-nodes = []

# Pairs of [BLOCK_NUM,BLOCK_ID] that should be enforced as checkpoints.
#"2021-09-09T21:59:33"
checkpoint = ["43328076","0295224c22b145b43c6ed6d4d56390b7fddb4758"]
#"2021-09-14T20:41:27"
checkpoint = ["43328077","0295224df70e863823bc29bb171e8380cd0d5f14"]
#"2021-09-14T20:41:45"
checkpoint = ["43328078","0295224e77097787cb53a4a573ac253f27912df9"]
#"2021-09-14T20:41:54"
checkpoint = ["43328079","0295224f0cbce57e982f68420a99de051f930636"]
#"2021-09-15T01:23:03"
checkpoint = ["43333079","029535d7245f2185541980db609f83afcafba04c"]
#"2021-09-15T05:55:06"
checkpoint = ["43338079","0295495fbf60a36d3070c25df0a82ed2e37155c5"]
#"2021-09-15T10:28:15"
checkpoint = ["43343079","02955ce706c015b9fcba279e0d7d45954461f586"]
#"2021-09-15T14:59:30"
checkpoint = ["43348079","0295706f11aa31f3f99579b232e184ec3a920cd3"]
#"2021-09-15T19:32:24"
checkpoint = ["43353079","029583f7af8a0784a391cdba382016b4c01f4bce"]
#"2021-09-16T00:03:42"
checkpoint = ["43358079","0295977f1764106629878a0d6e032d22f4ebaed3"]
#"2021-09-16T02:56:24"
checkpoint = ["43361260","0295a3ece12430f3f29def8c1fe16eaca07c9e9c"]

# Endpoint for websocket RPC to listen on
rpc-endpoint = 0.0.0.0:8090

# Endpoint for TLS websocket RPC to listen on
# rpc-tls-endpoint =

# The TLS certificate file for this server
# server-pem =

# Password for this certificate
# server-pem-password =

# File to read Genesis State from
# genesis-json =

# Block signing key to use for init witnesses, overrides genesis file
# dbg-init-key =

# JSON file specifying API permissions
# api-access =

# Whether to enable tracking of votes of standby witnesses and committee members. Set it to true to provide accurate data to API clients, set to false for slightly better performance.
# enable-standby-votes-tracking =

# Space-separated list of plugins to activate
plugins = witness account_history market_history accounts_list affiliate_stats bookie peerplays_sidechain


# ==============================================================================
# witness plugin options
# ==============================================================================

# Enable block production, even if the chain is stale.
enable-stale-production = false

# Percent of witnesses (0-99) that must be participating in order to produce blocks
required-participation = false

# ID of witness controlled by this node (e.g. "1.6.5", quotes are required, may specify multiple times)
witness-id = "1.6.###"

# IDs of multiple witnesses controlled by this node (e.g. ["1.6.5", "1.6.6"], quotes are required)
# witness-ids =

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
private-key = ["PPY...","5K..."]


# ==============================================================================
# peerplays_sidechain plugin options
# ==============================================================================

# ID of SON controlled by this node (e.g. "1.33.5", quotes are required)
# son-id =

# IDs of multiple SONs controlled by this node (e.g. ["1.33.5", "1.33.6"], quotes are required)
# son-ids =

# Tuple of [PublicKey, WIF private key] (may specify multiple times)
peerplays-private-key = ["PPY...","5K..."]

# IP address of Bitcoin node
bitcoin-node-ip = 127.0.0.1

# ZMQ port of Bitcoin node
bitcoin-node-zmq-port = 11111

# RPC port of Bitcoin node
bitcoin-node-rpc-port = 8332

# Bitcoin RPC user
bitcoin-node-rpc-user = 1

# Bitcoin RPC password
bitcoin-node-rpc-password = 1

# Bitcoin wallet
bitcoin-wallet = son-wallet

# Bitcoin wallet password
# bitcoin-wallet-password =

# Tuple of [Bitcoin public key, Bitcoin private key] (may specify multiple times)
bitcoin-private-key = ["02...","..."]

# Sidechain retry throttling threshold
sidechain-retry-threshold = 150
```
