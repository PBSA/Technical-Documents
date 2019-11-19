# Becoming a Peerplays Witness

Congratulations! You’ve taken the first step towards becoming a Peerplays Witness.

### **Can anyone be a Peerplays Witness?**

Yes, anybody can become a Peerplays Witness, but first ask yourself how you would answer the following questions:

1. Explain the role of a Peerplays Witness. 
2. Why do you want to be a Witness?
3. What qualities would you bring to the Peerplays community?
4. How much time are you willing to set aside to fulfil your obligations?
5. What do you know about the Bookie Oracle System \(BOS\)?
6. Do you have any experience as a server administrator and using a Command Line Interface \(CLI\)?
7. Do you understand the costs associated with buying and running the necessary hardware to be a witness node operator?

If you can answer these questions then you’re on your way to becoming a Peerplays Witness.

### **The duties of a Peerplays Witness**

The primary duty of Peerplays Witnesses is to bundle transactions into blocks and sign them with their signing key. Witnesses keep the blockchain alive by producing one block every three seconds. For example, if there are 20 Witnesses, each would produce one block every minute.

Other duties include:

* Operate a full node with enough bandwidth to support current network activities. 
* Operate a test version of the blockchain that functions as a public testing environment.
* Optionally operate an API seed node to support end user applications.
* Integrate, or reject, any changes to the blockchain software that are published. 
* Keep a block producing node running 24/7 every day of the year!

And one unique duty of a Peerplays Witness :

#### BookiePro & Bookie Oracle System \(BOS\)

BookiePro is the world’s first decentralized sports betting exchange application. 

For the application to function Peerplays Witnesses must also act as decentralized oracles, which means that the Witnesses are required to populate the Peerplays blockchain with real-world sporting data. For example, league and competition data, event data, betting market data for different sports, along with the final scores of each game. It requires approval from a majority of Witnesses for any of this event data to be actioned. 

This process is automated, but sometimes incident data from different sources doesn’t match and if no consensus can be made then it’s the duty of the Witnesses to manually intervene and make proposals to fix the data. Witnesses do this using the Manual Intervention Tool \(MINT\).

Smart contracts then use this data to grade and settle bets placed by users on BookiePro.

### Being a Peerplays Witness is a Paid Job

Blocks are produced every three seconds by Witnesses who take turns signing and validating the blockchain in variable rounds.

As a Peerplays Witnesses you are paid for this duty by the blockchain itself which releases new PPY tokens from the reserve pool, then issues them to the signing Witness after each block is validated. The payment for each signed block is 0.0075PPY.

With 20 blocks being signed every minute this means 28,800 blocks a day, split evenly between the number of active Witnesses. Since each block signed is equivalent to .0075 PPY there is total amount of 216 PPY a day to be split equally among every active witness; at the time of writing Peerplays had 13 active Witnesses.

Also, keep in mind that you have to either purchase, or rent, a server dedicated to Peerplays, and if you are not an active witness \(voted in\) then you'll earn nothing.

As Peerplays Witness pay is dependant on the value of the PPY it can vary a lot. Not only can being a Peerplays Witness be profitable but as the commitment and reliability of Witnesses has a direct impact on the value of the PPY, in this respect a Peerplays Witness plays a role in what their 'salary' will be.

### How to Get Votes

As only active Witnesses get paid it's very important to be voted in, and to do this you must sell yourself to the PPY token holders. Every PPY token holder can vote for a Witness, multiple times if they want to, and only as a result of the number of votes cast will a Witness be promoted to an active, block producing, Witness. The votes are weighted according to the token holdings of each PPY token holder at the time of voting.

The Peerplays core wallet makes it easy for PPY token holders to vote for Witnesses. Anyone who holds PPY can do this by adding the name of the Witness to the voting tab. The wallet then sends the information about their votes directly to the Peerplays blockchain.

To sell yourself to the Witness voting community you'll need a blog with information about how you intend to perform your duties as a Witness, and other ways in which you'll bring value to Peerplays.The best way to do this is by creating your own web page and publishing the URL in as many places as possible, and make it simple to find. Be very active on social media, especially target Peerplays and Peerplays Witness channels on Telegram and Discord. 

Qualities that voters are going to be looking for include, experience, knowledge, commitment, responsibility and community participation.

Some useful links:

Peerplays official Telegram channel  - [https://t.me/Peerplays](https://t.me/Peerplays)

Peerplays Witness Telegram channel - [https://t.me/PeerplaysWitness](https://t.me/PeerplaysWitness)

Facebook - [https://www.facebook.com/PeerPlays/](https://www.facebook.com/PeerPlays/)

### **Getting started**

We've already talked about the personal qualities a Witness needs to have, and the duties they're expected to perform, now we need to talk about nodes and hardware requirements.

#### **Nodes**

All nodes keep updating an internal database by applying the transactions as they arrive in incoming blocks. The difference between the node types lies in the amount of history they keep track of, and in the functionality they provide.

A **Witness node,** as the name implies, is a node run by a Witness. Each Witness node validates all blocks and transactions it receives. The nodes of elected Witnesses take turns in bundling new transactions into blocks and broadcasting them to the network.

**API nodes** \(nodes with an open RPC port\) provide network services to client applications. They usually have account transaction histories accessible though API calls, but can vary in the amount of available history. 

**Full nodes** are API nodes with a complete transaction history of all accounts.

**Seed nodes** are nodes that accept incoming P2P connections. They are the first nodes contacted by a freshly started node; the entry point into the network. Once a node has entered the network it will receive additional node addresses from its peers, so all nodes can connect to each other. A seed node can also be an API node. Seed nodes are not mandatory, but highly recommended.

**BOS nodes** are required to operate the Bookie Oracle System and ensure the accuracy and decentralization of the data fed into the BookiePro application. The BOS node must be run on a separate server to the Witness node.

Every Witness is required to run nodes on both Public Mainnet and Public Testnet environments.

So the minimum node requirements are a Witness node and BOS node for both Testnet and Mainnet. If you also run a Seed Node and API Node then the number of required servers could be as many as eight.

#### **System Requirements**

Unlike mining Bitcoin, or other POW based tokens, the processing power at your disposal has no influence on how many blocks you'll produce, and consequently how much you'll be paid as a Witness.

However, there are recommended requirements in terms of what hardware and software you should be running.

The following table shows what should be considered the minimum requirements for each server. 

| CPU | Memory | Storage | Bandwidth | OS |
| :--- | :--- | :--- | :--- | :--- |
| 8 Cores | 64GB | 300GB SSD | 1Gbps | Ubuntu 18.04 |

These requirements are as of the time of writing, so consider deploying a server with specs slightly higher than the ones listed above in order to "future proof" your server in case the minimum requirements grow in the future.

Once you've procured your servers then it's time to get set them up.

For instructions on setting up Witness, Seed and BOS nodes go to Joining the Peerplays Blockchain \[Hyperlink goes here\]

