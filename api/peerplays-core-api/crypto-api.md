# Crypto API

The crypto API is available from the full node via websockets.

## Blinding and Un-Blinding

### blind

Get signed blocks.

Generates a Pedersen Commitment: \*commit = blind \* G + value \* G2. The commitment is 33 bytes, the blinding factor is 32 bytes. 

{% hint style="info" %}
**Tip**: For more information about Pedersen Commitment see: [Commitment Scheme](https://en.wikipedia.org/wiki/Commitment_scheme)
{% endhint %}

```cpp
commitment_type 
graphene::app::crypto_api::blind(
    const fc::ecc::blind_factor_type &blind, 
    uint64_t value)
```

{% tabs %}
{% tab title="Parameters" %}
* **`blind`**: Sha-256 blind factor type
* **`value`**: Positive 64-bit integer value
{% endtab %}

{% tab title="Return" %}
A 33-byte Pedersen Commitment: _commit = blind_  G + value \* G2
{% endtab %}
{% endtabs %}

### blind\_sum

Get SHA-256 blind factor type.

```cpp
blind_factor_type 
graphene::app::crypto_api::blind_sum(
    const std::vector<blind_factor_type> &blinds_in, 
    uint32_t non_neg)
```

{% tabs %}
{% tab title="Parameters" %}
* **`blinds_in`**: List of SHA-256 blind factor types
* **`non_neg`**: 32-bit integer value
{% endtab %}

{% tab title="Return" %}
A blind factor type.
{% endtab %}
{% endtabs %}

## Rage Proofs



