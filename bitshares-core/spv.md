# SPV

Simple Payment Verification \(SPV\) basically provides a way for a client to validate a transaction was included within a specific block, provided knowledge of only a subset of blocks, not requiring the entire blockchain dataset.

### Proof of work

Provided that Proof of work does not come with finality \(but rather probabilistic finality - e.g. 99.99% after 6 blocks\), SPV on POW also is probabilistic. This makes it particular easy because all we need to do is validate that a particular transaction is part of a block \(through merkle root proofs\) and collect 6 subsequent blocks. From those, the required _work_ can be identified from the _difficulty_ miners had to resolve to find those blocks. Hence, we basically end up with an equation that connections electrical expense with probability of a transaction being part of a block. Parameters allow to fine-tune for required security.

### Delegated Proof-of-Stake \(DPoS\)

DPoS presents some difficulty in the fact that no connection between electrical expense and validity of a block can be made. Instead, only the authoritative block producer may sign a given block with their corresponding _signing key_ \(a secret private key\) within a specific time slot. Also, the block producer may change their authoritative signing key using a validated blockchain operation. Further, the set of valid block producers will change over time, as reflected by the votes collected, during each maintenance interval \(see below\). This brings us to some interesting questions:

* How does a client determine the set of valid block producers, over time?
* How does a client determine each block producers authoritative _signing key_, over time?
* How does a client confirm a given blocks was signed by authoritative signing key for that block production time slot?

An advantage of DPoS is the **finality** that comes as early as 2/3 of active block producers individually approving a given block \(e.g. having signed a block on top of the chain containing that block\).

#### Rounds and Maintenance Intervals

It is important to note that block producers are a fixed set of entities that have been granted permission to produce blocks in an a-priori deterministic order. In each **round** a block producer may only produce at most one block. Hence, if our blockchain defines its block production interval as 3 seconds, and a round has 20 block producers, the round ends after exactly 60 seconds. Afterwards, a new round is shuffled from this set of block producers.

Every maintenance interval, the votes for block producers are tallies which might cause the set of valid block producers to change, as well as the total number within the set, for the upcoming round. Therefore, additional producers may become active, active ones may become inactive, or active ones may be replaced. There are currently no bounds on neither the turnover within the block producer set nor the total number of block producers between maintenance intervals \(blockchain parameters do define minimum and maximum producer set size\).

### Problem Statement

All this is dealt with by DPoS internally, as part of the consensus protocol. If we were to verify the validity of a particular block, the only way to do so would be to go through every single block since genesis, as we cannot otherwise ensure that the block producers have actually been voted the way it is observed currently.

At the current state, a single block simply does not contain sufficient _information_ to ensure provability of that block being part of the actual \(BitShares\) blockchain. The only solution to that, obviously, requires to provide **additional** information together with the block/transaction in question. Obviously, we are looking for a way that requires less than the entire blockchain to be provided to prove a single transaction/block.

### Proposal

#### Step 1 - Announcing block producer

Provided that we need to be able to identify the block producers and their signing keys, we propose to:

* announce changes of block producers and signing keys in each first-block-in-round after the _\*maintenance interval_

This allows for keeping track of changes to block producers by providing only one block of information every maintenance interval \(currently 4hrs\). It thus becomes trivial to keep track of block producers since genesis \(as of writing, only `1095` have happened so far\).

#### Step 2 - BFT commitments of block producers per round

Provided that the order of block producers changes every round, we cannot verify that a block producer may have _gone rouge_ and provided a proof of a spoof block. For this reason, we introduce:

* a BFT-style operation that has to be broadcast by the block producer and can only be included in the **first-block-in-round** as a commitment of the producer to the first-block-in-round in the **previous round**.

This way, we can _count_ how many block producers agree on the outcome of the **last** round. If this number is higher than 2/3 of the available block producers \(which is a number we know because of Step 1\), we can assume that the **previous round** has also reached irreversibility.

#### Required data for a proof

Hence, for a proof that a transaction `tx` is part of a block `B(tx)`, we need the following information:

* the block header of `B(tx)` that contains the merkle root for proofing `tx in B(tx)`
* the previous `x` block headers up to the _\*first-block-in-round_ of the round containing `B(tx)`
* the previous `y` _\*first-block-in-round_ headers up to genesis

## Open Questions

* How to allow a block producer to change signing key?
* Can we make irreversible explicit if the block producer commit to the first-block-in-round of the **previous round**?

## Literature

1. [kyber.network](https://blog.kyber.network/waterloo-a-decentralized-practical-bridge-between-eos-and-ethereum-1c230ac65524)

