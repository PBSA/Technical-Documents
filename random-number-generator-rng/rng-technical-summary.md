# RNG Technical Summary

\[TODO\] Start with [https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/330957016/RNG-on-Chain)

## Introducing the Peerplays Random Number Generator <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

## How are Random Numbers Generated <a id="RandomNumberGenerationonPeerplays-howit&apos;sdone?-HowisRNGgenerated"></a>

Random numbers are generated from secret hashes taken from the previous and current block, which are then combined into a single data stream and encoded using the  `ripemd160` algorithm, and finally fed into a random number generator as a seed.

{% hint style="info" %}
For more information on the `ripemd160` algorithm see:

[https://en.wikipedia.org/wiki/RIPEMD](https://en.wikipedia.org/wiki/RIPEMD)
{% endhint %}

```cpp


Function uint64_t database::get_random_bits(uint64_t bound) returns random number, where parameter bound represents upper limit.



Typedefs


peerplays/libraries/chain/include/graphene/chain/protocol/types.hpp

typedef fc::ripemd160 secret_hash_type;


peerplays/libraries/fc/include/fc/crypto/hash_ctr_rng.hpp

template<class HashClass, int SeedLength>
class hash_ctr_rng
{...}



Declaration


peerplays/libraries/chain/include/graphene/chain/database.hpp 

fc::hash_ctr_rng<secret_hash_type, 20> _random_number_generator;



Default initialization in constructor


peerplays/libraries/chain/db_management.cpp

_random_number_generator(fc::ripemd160().data())



Updating seed on new block


peerplays/libraries/chain/db_update.cpp

modify( _dgp, [&]( dynamic_global_property_object& dgp ){
      secret_hash_type::encoder enc;       
      fc::raw::pack( enc, dgp.random );       
      fc::raw::pack( enc, b.previous_secret );        
      dgp.random = enc.result();
      _random_number_generator = fc::hash_ctr_rng<secret_hash_type, 20>(dgp.random.data());


Getting new random number

peerplays/libraries/chain/db_update.cpp

uint64_t database::get_random_bits( uint64_t bound )
{
   return _random_number_generator(bound);
}

peerplays/libraries/fc/include/fc/crypto/hash_ctr_rng.hpp

      uint64_t operator()( uint64_t bound )
      {
         if( bound <= 1 )
            return 0;
         uint8_t bitcount = boost::multiprecision::detail::find_msb( bound ) + 1;
         // probability of loop exiting is >= 1/2, so probability of
         // running N times is bounded above by (1/2)^N
         while( true )
         {
            uint64_t result = get_bits( bitcount );
            if( result < bound )
               return result;
         }
      }
```

  
For more understanding why RNG generation is done this way, read the following article  
[https://medium.com/ginar-io/a-review-of-random-number-generator-rng-on-blockchain-fe342d76261b](https://medium.com/ginar-io/a-review-of-random-number-generator-rng-on-blockchain-fe342d76261b)

**Important part:**

**Block-hash**

In this approach, the hash of blocks or transactions is used as the source of randomness. As the hash is deterministic, everyone will get the same result. A block, once added to the blockchain, is likely to stay there forever, therefore, everyone can verify the correctness of the generated numbers.

Consider an example of a lottery service that adopts this method. The players first buy a ticket by placing their number before a specific time, say 7PM everyday. After 7PM, the buying ticket phase is closed, the protocol proceeds to the next phase which is to determine the winning numbers for a ticket. This ticket is calculated based on the hash of the first block accessible for everyone on the Bitcoin blockchain after 8PM. As we can see, at 7PM, no one can predict the hash of block at 8 PM which makes the service seemingly a sound one. However, this hash is subject to manipulation by the miners guarding the blockchain. When the reward of the lottery is small, the miners have little motivation to tamper with the block but as soon as this amount is larger than the block reward plus the transaction fees, there is a chance that miners will start influencing the block-hash to generate their desired numbers. Thus, for high-stake randomness-based applications, this method is not a secure one.

