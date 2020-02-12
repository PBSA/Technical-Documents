# Gamified Proof of Stake \(GPOS\)

## Introduction

Building on the success of the DPOS consensus mechanism, Peerplays introduced a unique enhancement called Gamified Proof of Stake \(GPOS\). This idea was first proposed as [Peerplays Improvement Proposal \(PIP\) \#2](https://github.com/peerplays-network/pips/blob/master/pip-0002.md) in January 2019.

The original intent of Peerplays was to operate as a Decentralized Autonomous Cooperative \(DAC\) where DPOS enables the voting collective of core token holders to determine who would act as Advisors, Witness, and Proposals within Peerplays. However, like other DPOS based blockchains, the challenges of voter turnout continue to plague Peerplays. 

## What Makes GPOS Different?

GPOS made a protocol change such that PPY token holders now receive a _participation reward_ based on their voting performance and how many PPY they have vested or _staked_.

This is a significant change from the original protocol where token holders were rewarded with their share of a rake, taken from a percentage of the blockchain fees, and then distributed to token holders relative to their token holdings, regardless of any voting participation.

Put simply, this means that each PPY token holder needs to vest some, or potentially all, of their PPY balance towards GPOS and then once a month vote for either Witnesses, Advisors or Proxies.

## Why is GPOS Important?

Being a DPOS consensus blockchain, the importance of voter participation of the PPY token holders is paramount to the security of the blockchain. The introduction of GPOS ensures that token holders will take an active interest in the operation and governance of Peerplays.

Each vote cast impacts the people behind the successful operation of Peerplays, in particular the  Witnesses. A Witness receiving high votes is much more likely to be elected as a block producing \(active\) Witness.

Under DPOS the Witness position is more secure because a lot less token holders are voting and subsequently it might just take one or two major PPY token holders to influence the election of a Witness. Under GPOS all token holders should participate making it a much more democratic process, and also receive rewards for voting.

## Participation Rewards

Participation rewards combined with voting performance are what makes the Peerplays consensus mechanism _gamified._

The rewards are calculated as follows:

### **Qualified Reward %**

This is the percentage of a user's maximum possible reward received based on their voting performance. This reward _decays_ at a rate of 16.67% per month. For example, if a token holder doesn't vote for a month the qualified reward percentage drops to 83.33% and if the token holder doesn't vote for six consecutive months the reward will be 0%.

{% hint style="warning" %}
**Note**: Complete decay was set at six consecutive months to coincide with the dividend distribution periods.
{% endhint %}

### **Estimated Rake Reward %**

This is the potential percentage reward a user could receive based on qualified reward percentage, the amount of PPY they have vested and their share of the total GPOS balance, as:

Estimated rake reward %= r  
GPOS balance = b  
Total GPOS balance on blockchain = TB  
Reward % = p

r = \(b / TB\) \* p

For example:

A user has a GPOS balance of 1,000PPY and is entitled to 100% of his reward based on voting performance. The total GPOS balance on the blockchain is 4,000,000PPY.

The user would receive the following percentage of the rake:

\(1,000 / 4,000,000\) \* 100% = 0.025%

So if the total \(month\) rake was 100,000PPY then the user would receive 25PPY



