# API

Get a random number

```cpp
Function uint64_t database::get_random_bits(uint64_t bound) 
```

{% tabs %}
{% tab title="Parameters" %}
**`bound`**`:` The upper limit for the random number.
{% endtab %}

{% tab title="Returns" %}
A random number within the `bound` range.
{% endtab %}
{% endtabs %}

### TypeDefs

```cpp
peerplays/libraries/chain/include/graphene/chain/protocol/types.hpp
typedef fc::ripemd160 secret_hash_type;


peerplays/libraries/fc/include/fc/crypto/hash_ctr_rng.hpp
template<class HashClass, int SeedLength>

class hash_ctr_rng
{...}
```

#### Declaration

```cpp
peerplays/libraries/chain/include/graphene/chain/database.hpp 

fc::hash_ctr_rng<secret_hash_type, 20> _random_number_generator;
```

```cpp


Typedefs






Declaration






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

#### Examples



