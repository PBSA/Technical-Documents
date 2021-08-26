---
title: Consensus Mechanisms Compared
description: 'The differences among POW, POS, & POP (and their variations)'
---

# Consensus Mechanisms Compared

## 1. Consensus

In the realm of crypto-economies, consensus is the way decisions are made that impact a whole network despite having no central decision-maker. Systems must be in place to allow everyone to agree on transactions happening in real-time across the globe. This is why the developers of blockchains have carefully crafted mechanisms of consensus to try to preserve decentralization while also maintaining mutual agreement of all members of the network.

Here we will explain the most widely used and well known consensus mechanisms, Proof of Work and Proof of Stake. We will also introduce a new consensus mechanism which serves to be the latest evolution and a paradigm shift to the approach of consensus, Proof of Pulse.

## 2. Proof of Work

### 2.1. The Big Idea

> Competitive: The one with the most computing power wins.

Proof of Work \(**POW**\) is essentially a puzzle-solving race to determine block creation. Block producers, known in this case as "miners", use ever-increasing levels of computing power to solve more and more difficult puzzles. The first miner to solve the puzzle produces a block on the network, receives a reward, and the process starts again with a new puzzle. Its main goal is to incentivize people to use their computing power to ensure the network continues to run, and the blocks are validated.

### 2.2. Incentivization

Miners are in a competitive race to produce blocks on the network. For every block produced, a reward is given to the producer. The difficulty of the puzzles increases over time to keep the block production at roughly 10 minutes per block \(on the Bitcoin chain\). The difficultly must increase due to more and more computing power coming online to earn rewards. This has the unfortunate side effect of being horribly inefficient. More and more electricity is being used to fuel the computing power, etc.

### 2.3. Network Security

In POW, the distribution of computing power matters most. If an individual or organization could gain 51% of the network computing resources, they could simply validate any block they wished. In essence, it would be a complete chain take-over. The power of cryptocurrencies comes from the decentralization of its ledger of transactions. If anyone could own 51% of the network resources, they would own the ledger and the network would no longer be decentralized.

## 3. Proof of Stake

### 3.1. The Big Idea

> Competitive: The one with the most tokens wins.

Proof of Stake \(**POS**\) is another competitive consensus algorithm. But instead of a computing power race, the level of staked tokens is used as the determining factor of who can produce and validate blocks. This method is much more efficient than POW. The more staked tokens an account has, the more priority it is given to produce and validate blocks. Rewards at then given to the producers upon block production.

### 3.2. Incentivization

Block producers, sometimes called "Master Nodes" in POS, store vast amounts of the network's tokens in stakes. Depending on the chain in question, sometimes a certain \(usually very high\) level of stake is required to be a block producer. Higher stakes means more rights to produce blocks which in turn means more rewards. This translates to a classic "the rich get richer, the poor get poorer" scenario.

### 3.3. Network Security

In POS, the distribution of staked tokens matters most. Much like in POW, If an individual or organization could gain 51% of the staked tokens, they could simply validate any block they wished. Again, it would be a complete chain take-over. The power of cryptocurrencies comes from the decentralization of its ledger of transactions. If anyone could own 51% of the staked tokens, they would own the ledger and the network would no longer be decentralized.

### 3.4. Variations

#### Delegated Proof of Stake

Delegated Proof of Stake \(**DPOS**\) is a variation of POS in that instead of only the highest levels of stake getting a say, everybody with any amount of stake can participate. Although the same "Master Node" concept applies, anyone can apply their stake to back a Master Node. When the Master Nodes are rewarded, the rewards are shared proportionally with those who backed them up with their stake. It helps to spread the rewards a little, but it's like the blockchain equivalent of "trickle down" economics. It still requires huge stakes to have meaningful rewards.

In some chains, like Graphene based chains, DPOS is implemented more like a vote. Master Nodes \("Witnesses" in Graphene\) are voted on by applying your stake as a vote for someone else's node. The winners of the vote are then the active block producers.

#### Gamified Proof of Stake

Gamified Proof of Stake \(**GPOS**\) is a further improvement on DPOS. Voting for Witnesses with staked tokens occurs like with other Graphene chains, but in this case the rewards given to the voters diminishes over time. Voters must periodically vote again to maintain their rewards income. This requires the voters to be more actively engaged in the network governance to reap the full benefits of their stake.

GPOS is where we start to see a major paradigm shift:

* We're moving from **competitive** consensus to **cooperative** consensus.
* The rewards shift from paying those that **own capital** \(expensive computers, vast sums of tokens\) to paying those that **do work** \(node operators providing services\).
* People go from being **passive users** of a network to **engaged participants** in network governance.
* The incentives shift from the **extrinsic**, "What can the network do for me?" to the **intrinsic**, "How can we improve the network for everyone?".

## 4. Proof of Pulse

### 4.1. The Big Idea

> Cooperative: Everyone contributes to incremental network improvement.

Proof of Pulse \(**POP**\) generates network consensus spontaneously from the outcomes of continuous, randomized, and incremental voting. Every hour a round of voting, a **"pulse"**, begins:

1. First, a random subset of accounts are selected across the network.
2. These accounts are each given one random item to vote on. \(Fees, node operators, etc.\)
3. Each chosen account is notified, and there is a limited time to make their vote.
   1. Votes can only be: Up \(in favor\), Down \(against\), or Stay \(no change\).
   2. Votes are weighted by voting power of the account.
4. The votes are tallied per item and the winning vote is how that particular item will change.
5. The impact of the pulse will expire after 60 days.

This method ensures that, while everyone has a voice, it's the will of the collective whole that moves the network along.

### 4.2. Incentivization

The POP consensus mechanism relies on the intrinsic motivation of individuals who genuinely wish to improve the state of the network. There are no monetary rewards for casting votes or participating in blockchain governance. Node operators are paid for the work they perform for the network. One of the decisions of consensus is how much a node operator will be paid for their services.

Another form of income is to stake tokens to supply liquidity pools to enable instant token swaps. In this case, staked tokens will earn rewards from the transaction fees of the token swaps they are backing. Once again, the transaction fees are changed through decisions of consensus.

Note that in POP, rewards are only paid when some form of work is done. If you are a node operator, you are running software to provide a service. If you stake to a liquidity pool, people are using your tokens to make swaps in the exchange.

### 4.3. Network Security

#### The Attacker's Dilemma

An attacker's goal is to gain as much control over the network parameters as possible. This is usually done by using overwhelming resources to gain the majority of voting power or ability to edit the chain itself. In a POP system, an attacker has to make a decision:

* Concentrate all of their voting power on one account...
  * but severely limit their chances of being included in the vote they wish to control!
* Open hundreds of accounts to give themselves a much greater chance of being randomly assigned the votes they wish to control...
  * but spread their voting power too thin to actually control the vote!

As you can see, it's a bad situation for an attacker. They cannot simultaneously have "whale" levels of voting power and also guarantee they'll be assigned the votes they want to control. But it gets even worse for an attacker. Even if they hold massive voting power in their account, and get lucky enough to be randomly assigned the vote they want, they can't **set** chain parameters. Votes are only for incremental changes from the existing parameter levels. And on top of that, the effects of votes drop off over time. That is to say, a vote outcome today will not have an effect on the chain parameters after 60 days. So they would have to have absurd luck, again and again, to dominate the chain.

Proof of Pulse was designed specifically for consensus of the whole network.

## 5. Future of Consensus

Consensus is about agreement. That's why it's important to rethink the mechanisms we create to form consensus in this new decentralized world. Competitive structures don't seek agreement. Instead we can use cooperation to build a system that benefits everyone. We can use incentives to get work done rather than to pay those who are already at the top. We can build more human centered networks. This is what the Proof of Pulse consensus mechanism is all about.

