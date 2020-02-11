# RNG Technical Summary

\[TODO\] Start with [https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain)

## Introducing the Peerplays Random Number Generator <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>



## How are Random Numbers Generated <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

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

### Witness Randomness

Peerplays is based on the Delegated Proof of Stake \(DPOS\) consensus mechanism, which means that that the block signers \(Witnesses\) are all elected by the token holders. This is important from an RNG perspective because it requires loyalty, commitment and honesty to get voted in as a Witness. Since a component of the randomness is based on the block hash, knowing that the block signers are a closed group greatly mitigates the risk of block tampering.

Peerplays then further extends the block-signing robustness and randomness by:

1. Not all Witnesses are block-signing \(active\) Witnesses. There is a second level of consensus that has to happen before a Witness is promoted to an active Witness.
2. Not all active Witnesses are signing blocks at any given time. The blockchain randomly selects which Witnesses are signing at ten minute intervals.

Because of the importance of the Witness role we see that the Peerplays has two levels of randomness. First the block producers themselves are randomly selected, and then secondly the randomness of the number generation itself. 

### API

The RNG has a very simple API for generating random numbers,  requiring just a single API call to `get_random_bits(bound)` ; supplying an upper bound.

For more information on the API see:

{% page-ref page="api.md" %}



```cpp
get_random_bits(uint64_t bound) 
```

