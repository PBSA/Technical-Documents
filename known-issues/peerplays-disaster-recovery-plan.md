---
title: Peerplays Chain Disaster Recovery Plan
---

# Peerplays Disaster Recovery Plan

The objective of this disaster recovery \(DR\) plan is to ensure that anyone can respond to a disaster or other emergency that affects the Peerplays Blockchain and minimize the effect on the operation of the blockchain. This document should be stored in a safe, accessible location off site.

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

The witness\_node program is distributed across many private \(node operator\) servers. Each node operator is responsible for maintaining their own copy of the Peerplays source code and builds. This is most often done on GitHub though individuals may store local copies and follow their own procedures.

The Peerplays database is synchronized among all running \(active and inactive\) witness nodes. This decentralized and distributed approach makes the database integrity and security extremely robust. Even if the mainnet has an outage, the database remains intact and duplicated across potentially dozens of individual servers globally.

### 3.2. Documentation

Documentation is stored on GitHub and GitBook. This includes all public documentation for community docs, developer docs, and infrastructure docs. Both GitHub and GitBook are third-party SaaS solutions. Other private or internal documentation for PBSA is stored in Confluence workspaces and Google Drive. These are also third-party SaaS solutions. For the purposes of this DR plan, it is assumed that these providers have sufficient recovery capabilities on their own and that recovery of our documentation is covered under their DR plans and protocols.

Documentation that is currently being worked on is sometimes stored on personal computers or workstations. In these cases, it is recommended to store or back-up such documents on a cloud solution such as Google Drive, GitBook, GitHub, etc.

#### GitBook.com Resources

* [https://docs.gitbook.com/resources/faq\#where-and-how-is-my-data-stored](https://docs.gitbook.com/resources/faq#where-and-how-is-my-data-stored)
* Gitbook Support is available in the gitbook app. Once logged in, go to Spaces &gt; "?" menu \(lower left\) &gt; Contact Support

### 3.3. Source Code

Peerplays source code is stored and hosted on GitLab.com \(a SaaS solution\). The source code is then mirrored to GitHub.com \(another SaaS solution\). Since both GitLab and GitHub are third-party providers, for the purposes of this DR plan, it is assumed that these providers have sufficient recovery capabilities on their own and that recovery of our source code is covered under their DR plans and protocols.

#### GitLab.com Resources

* [https://about.gitlab.com/handbook/engineering/infrastructure/production/architecture/](https://about.gitlab.com/handbook/engineering/infrastructure/production/architecture/)
* [https://about.gitlab.com/handbook/engineering/infrastructure/faq/](https://about.gitlab.com/handbook/engineering/infrastructure/faq/)
* [https://status.gitlab.com/](https://status.gitlab.com/)
* [https://about.gitlab.com/support/\#contact-support](https://about.gitlab.com/support/#contact-support)

#### GitHub.com Resources

* [https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository](https://docs.github.com/en/repositories/archiving-a-github-repository/backing-up-a-repository)
* [https://www.githubstatus.com/](https://www.githubstatus.com/)
* [https://support.github.com/request](https://support.github.com/request)

### 3.4. Peerplays.com

#### Source Code

The website Peerplays.com has its source code stored in GitHub. The website is static and requires no database or dynamic data. No data backup is required.

#### Hosting

Peerplays.com is hosted on a PBSA VM.

### 3.5. Peerplays.tech

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

| Communication Channel | Hosting | Uses |
| :--- | :---: | :--- |
| Email | Third-Party | Daily internal communications |
| Telegram | Third-Party | Community discussions, Group discussions \(witnesses\) |
| Rocket Chat | On-Prem | Internal and Community discussions, Group discussions, Meetings |
| Jitsi | On-Prem | Meetings |
| Twitter | Third-Party | Social Media |
| Facebook | Third-Party | Social Media |
| Instagram | Third-Party | Social Media |

#### Telegram Resources

* [https://telegram.org/faq](https://telegram.org/faq)
* [https://telegram.org/faq\#telegram-support](https://telegram.org/faq#telegram-support)

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

1. A suitable \(not corrupted or unintentionally forked\) copy of the Peerplays database should be collected.
2. Provision a Super-Node \(SN\) server using a cloud provider VM.
   1. The SN should be firewalled such that no incoming nodes can sync with the SN.
   2. The database copy should be loaded to the SN.
   3. The latest Peerplays chain code should be installed on the SN.
   4. The SN should be configured with as many witness nodes as required to recover the network. \(Init accounts and/or collaborating witness accounts\)
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

## 7. Testing the Disaster Recovery Plan

A local testnet can be set up for testing using the standard procedure for [standing up a local testnet](https://infra.peerplays.tech/advanced-topics/private-testnets/private-testnets-manual-install). Once the testnet is running, it's possible to run various DR scenarios in a safe and controlled manner.

### 7.1 Testing Network Recovery Procedures

Testing a mainnet outage can be achieved by setting up a local testnet with two sets of witnesses and some voting accounts. To cause a chain halt:

1. The local testnet contains the entire network.
2. Vote for mis-configured witness accounts until they become active and eventually halt the chain.
3. Another testnet can be configured at this point to act as the Super-Node in this test.
4. The database can be recovered from the first testnet, processed to be suitable for a restart, and used in the second testnet.
5. The protocol in section 6.1. above can be used at this point to test the outage recovery scenario.

