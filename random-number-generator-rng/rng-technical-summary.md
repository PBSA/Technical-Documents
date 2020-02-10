# RNG Technical Summary

\[TODO\] Start with [https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain)

## Introducing the Peerplays Random Number Generator <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>



## How are Random Numbers Generated <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

Random numbers are generated from secret hashes taken from the previous and current block, which are then combined into a single data stream and encoded using the  `ripemd160` algorithm, and finally fed into a random number generator as a seed.

{% hint style="info" %}
For more information on the `ripemd160` algorithm see:

[https://en.wikipedia.org/wiki/RIPEMD](https://en.wikipedia.org/wiki/RIPEMD)
{% endhint %}



### 

  
For more understanding why RNG generation is done this way, read the following article  
[https://medium.com/ginar-io/a-review-of-random-number-generator-rng-on-blockchain-fe342d76261b](https://medium.com/ginar-io/a-review-of-random-number-generator-rng-on-blockchain-fe342d76261b)

**Important part:**

**Block-hash**

In this approach, the hash of blocks or transactions is used as the source of randomness. As the hash is deterministic, everyone will get the same result. A block, once added to the blockchain, is likely to stay there forever, therefore, everyone can verify the correctness of the generated numbers.

Consider an example of a lottery service that adopts this method. The players first buy a ticket by placing their number before a specific time, say 7PM everyday. After 7PM, the buying ticket phase is closed, the protocol proceeds to the next phase which is to determine the winning numbers for a ticket. This ticket is calculated based on the hash of the first block accessible for everyone on the Bitcoin blockchain after 8PM. As we can see, at 7PM, no one can predict the hash of block at 8 PM which makes the service seemingly a sound one. However, this hash is subject to manipulation by the miners guarding the blockchain. When the reward of the lottery is small, the miners have little motivation to tamper with the block but as soon as this amount is larger than the block reward plus the transaction fees, there is a chance that miners will start influencing the block-hash to generate their desired numbers. Thus, for high-stake randomness-based applications, this method is not a secure one.

### API

The RNG has a very simple API for generating random numbers,  requiring just a single API call to `get_random_bits(bound)` ; supplying an upper bound.

For more information on the API see:

{% page-ref page="api.md" %}



```cpp
get_random_bits(uint64_t bound) 
```

