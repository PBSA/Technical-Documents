# RNG Technical Summary

## Introduction <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

The Peerplays blockchain RNG was initially added to the blockchain to provide draw features for the Easy5050 decentralized application \(DApp\). 

The first release of the RNG was localized to the Easy5050 DApp and wasn't exposing the number generated via an API, or a similar mechanism, such that other DApps or users could consume them. 

For the second release of the RNG the functionality was extended to become a general RNG, with a public API, for supporting games such as Slots, Poker, Roulette, Raffle, Bingo, Keno, etc.

## How the Random Numbers are Generated <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

Random numbers are generated from secret hashes taken from the previous and current block, which are then combined into a single data stream and encoded using the  `ripemd160` algorithm, and finally fed into a random number generator as a seed.

{% hint style="info" %}
For more information on the `ripemd160` algorithm see:

[https://en.wikipedia.org/wiki/RIPEMD](https://en.wikipedia.org/wiki/RIPEMD)
{% endhint %}

### **Block-Hash Randomness**

In this approach, the hash of blocks or transactions is used as the source of randomness. As the hash is deterministic, everyone will get the same result. A block, once added to the blockchain, is likely to stay there forever, therefore, everyone can verify the correctness of the generated numbers.

Consider an example of a lottery service that adopts this method. The players first buy a ticket by placing their number before a specific time, say 7PM everyday. After 8PM, the buying ticket phase is closed, the protocol proceeds to the next phase which is to determine the winning numbers for a ticket. This ticket is calculated based on the hash of the first block accessible for everyone on the blockchain after 8PM. 

As we can see, at 7PM, no one can predict the hash of block at 8PM which makes the service seemingly a sound one. However, this hash is subject to manipulation by the block-signers of the blockchain. When the reward of the lottery is small, the block-signers have little motivation to tamper with the block, but as soon as this amount is larger than the block reward plus the transaction fees, there is a chance that witnesses will start influencing the block-hash to generate their desired numbers. 

So on it's own a block-hash level of randomness is not enough. This is why the Peerplays RNG extends the block-hash mechanism by using the hash as a seed for randomization along with the `repemd160` algorithm and Secure Hash Algorithm \(SHA\).

### Distributed Ledger Technology \(DLT\)

Peerplays, and other blockchains, are one type of a distributed ledger. Distributed ledgers use independent computers \(referred to as nodes\) to record, share and synchronize transactions in their respective electronic ledgers \(instead of keeping data centralized as in a traditional ledger\). 

The immutability of DLT is critical for the RNG because it ensures that once a random number is generated it is authentic and can't be changed.

### Witness Randomness

Peerplays is based on the Delegated Proof of Stake \(DPOS\) consensus mechanism, which means that the block signers \(Witnesses\) are all elected by the token holders. This is important from an RNG perspective because it requires loyalty, commitment and honesty to get voted in as a Witness. Since a component of the randomness is based on the block hash, knowing that the block signers are a trusted, elected, group greatly mitigates the risk of block tampering.

Peerplays further extends the block-signing robustness and randomness by:

1. Not all Witnesses are block-signing \(active\) Witnesses. There is a second level of consensus that has to happen before a Witness is promoted to an active Witness.
2. Not all active Witnesses are signing blocks at any given time. The blockchain randomly selects which Witnesses are signing at ten minute intervals.

Because of the role of the Witnesses, Peerplays has two levels of randomness. First the Witnesses themselves are randomly selected, and secondly the randomness of the number generation itself. 

## Testing

For testing the RNG, we used the “Dieharder” random number generator testing suite.

Dieharder is intended to test generators, not files of possibly random numbers as the latter is based on the mistaken view of what it means to be random. Perfect random number generators produce "unlikely" sequences of random numbers -- at exactly the right average rate. Testing an RNG is therefore quite subtle.

Dieharder is a tool designed to push a weak generator to unambiguous failure.

{% hint style="info" %}
For more information on Dieharder see:

[https://webhome.phy.duke.edu/~rgb/General/dieharder.php](https://webhome.phy.duke.edu/~rgb/General/dieharder.php)
{% endhint %}

### Install prerequisites for running RNG tests

```text
sudo apt install dieharder
```

### To run RNG test suite, use the following command: <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-TorunRNGtestsuite,usethefollowingcommand:"></a>

```text
$ ./tests/random_test 
```

## Gaming Laboratories International \(GLI\)

GLI are an internationally recognized institute offering the the most experienced and robust RNG testing methodologies in the world. This includes software-based \(pseudo-algorithmic\) RNG’s, hardware RNG’s, and hybrid combinations of both.

The Peerplays RNG has been submitted to GLI for approval.

GLI generally performs the testing of applications and games, such as Keno, as opposed to an API. The game testing will automatically include the backend API as well. On successful completion of all tests each application will be certified by GLI.

For each new game, different testing will be required. An example would be the Easy5050 DApp that can be tested and with all subsequent releases, tested again.

However, as the core Peerplays RNG is a blockchain \(back-end\) implementation and not an application it can only be approved by GLI rather than certified.

## API

The RNG has a very simple API for generating random numbers,  requiring just a single API call to `get_random_bits(bound)` ; supplying an upper bound.

```cpp
get_random_bits(uint64_t bound) 
```

For more information on the API see:

{% page-ref page="api.md" %}

