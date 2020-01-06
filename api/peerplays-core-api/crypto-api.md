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

## Range Proofs

### range\_get\_info

Gets “range proof” information. 

The cli\_wallet includes functionality for sending blind transfers in which the values of the input and output amounts are “blinded.” 

{% hint style="warning" %}
**Note**: In the case where a transaction produces two or more outputs, \(e.g. an amount to the intended recipient plus “charge” back to the sender\), a “range proof” must be supplied to prove that none of the outputs commit to a negative value.
{% endhint %}

```cpp
range_proof_info 
graphene::app::crypto_api::range_get_info(
    const std::vector<char> &proof)
```

{% tabs %}
{% tab title="Parameters" %}
**`proof`**: List of proof’s characters
{% endtab %}

{% tab title="Return" %}
A range proof info structure with exponent, mantissa, min and max values.
{% endtab %}
{% endtabs %}

### range\_proof\_sign

Proves with respect to min\_value the range for Pedersen Commitment which has the provided blinding factor and value.

```cpp
std::vector<char> 
graphene::app::crypto_api::range_proof_sign(
    uint64_t min_value, 
    const commitment_type &commit, 
    const blind_factor_type &commit_blind, 
    const blind_factor_type &nonce, 
    int8_t base10_exp, 
    uint8_t min_bits, 
    uint64_t actual_value
```

{% tabs %}
{% tab title="Parameters" %}
* **`min_value`**: Positive 64-bit integer value
* **`commit`**: 33-byte pedersen commitment
* **`commit_blind`**: Sha-256 blind factor type for the correct digits
* **`nonce`**: Sha-256 blind factor type for our non-forged signatures
* **`base10_exp`**: Exponents base 10 in range \[-1 ; 18\] inclusively
* **`min_bits`**: 8-bit positive integer, must be in range \[0 ; 64\] inclusively
* **`actual_value`**: 64-bit positive integer, must be greater or equal min\_value
{% endtab %}

{% tab title="Return" %}
A list of characters as proof in proof.
{% endtab %}
{% endtabs %}

## Verification

### verify\_sum

Verifies that `commits` + `neg_commits` + `excess` == 0.

```cpp
bool 
graphene::app::crypto_api::verify_sum(
    const std::vector<commitment_type> &commits_in, 
    const std::vector<commitment_type> &neg_commits_in, 
    int64_t excess)
```

{% tabs %}
{% tab title="Parameters" %}
* **`commits_in`**: List of 33-byte Pedersen Commitments
* **`neg_commits_in`**: List of 33-byte Pedersen Commitments
* **`excess`**: Sum of two list of 33-byte Pedersen Commitments where sums the first set and subtracts the second
{% endtab %}

{% tab title="Return" %}
\(Boolean\) True in event of `commits` + `neg_commits` + `excess` == 0, otherwise false
{% endtab %}
{% endtabs %}

### verify\_range

Verifies range proof for 33-byte Pedersen Commitment.

```cpp
verify_range_result 
graphene::app::crypto_api::verify_range(
    const fc::ecc::commitment_type &commit, 
    const std::vector<char> &proof)
```

{% tabs %}
{% tab title="Parameters" %}
* **`commit`**: 33-byte pedersen commitment
* **`proof`**: List of characters
{% endtab %}

{% tab title="Return" %}
A structure with success, min and max values
{% endtab %}
{% endtabs %}

### verify\_range\_proof\_rewind

Verifies range proof rewind for 33-byte Pedersen Commitment.

```cpp
verify_range_proof_rewind_result 
graphene::app::crypto_api::verify_range_proof_rewind(
    const blind_factor_type &nonce, 
    const fc::ecc::commitment_type &commit, 
    const std::vector<char> &proof)
```

{% tabs %}
{% tab title="Parameters" %}
* **`nonce`**: Sha-256 blind refactor type
* **`commit`**: 33-byte pedersen commitment
* **`proof`**: List of characters
{% endtab %}

{% tab title="Return" %}
A structure with success, min, max, value\_out, blind\_out and message\_out values.
{% endtab %}
{% endtabs %}

