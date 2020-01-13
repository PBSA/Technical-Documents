# Governance

## Governance

### create\_committee\_member

Creates a committee\_member object owned by the given account.

An account can have at most one committee\_member object.

```cpp
signed_transaction graphene::wallet::wallet_api::create_committee_member(
    string owner_account, 
    string url, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`owner_account`**: the name or id of the account which is creating the committee\_member
* **`url`**: a URL to include in the committee\_member record in the blockchain. Clients may display this when showing a list of committee\_members. May be blank.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction registering a committee\_member
{% endtab %}
{% endtabs %}

### get\_witness

Returns information about the given witness.

```cpp
witness_object graphene::wallet::wallet_api::get_witness(
    string owner_account)
```

{% tabs %}
{% tab title="Parameters" %}
* **`owner_account`**: the name or id of the witness account owner, or the id of the witness
{% endtab %}

{% tab title="Return" %}
The information about the witness stored in the block chain.
{% endtab %}
{% endtabs %}

### **get\_committee\_member**

Returns information about the given committee\_member.

```cpp
committee_member_object graphene::wallet::wallet_api::get_committee_member(
    string owner_account)
```

{% tabs %}
{% tab title="Parameters" %}
* **`owner_account`**: the name or id of the committee\_member account owner, or the id of the committee\_member.
{% endtab %}

{% tab title="Return" %}
**T**he information about the committee\_member stored in the block chain
{% endtab %}
{% endtabs %}

### list\_witnesses

Lists all Witnesses registered in the blockchain. This returns a list of all account names that own Witnesses, and the associated witness id, sorted by name. This lists Witnesses whether they are currently voted in or not.

Use the `lowerbound` and limit parameters to page through the list. To retrieve all Witness's, start by setting `lowerbound` to the empty string `""`, and then each iteration, pass the last witness name returned as the `lowerbound` for the next `list_witnesss()` call.

```cpp
map<string, witness_id_type> graphene::wallet::wallet_api::list_witnesses(
    const string &lowerbound, 
    uint32_t limit)
```

{% tabs %}
{% tab title="Parameters" %}
* `lowerbound`: the name of the first Witness to return. If the named Witness does not exist, the list will start at the witness that comes after `lowerbound`
* `limit`: the maximum number of Witness's to return \(max: 1000\)
{% endtab %}

{% tab title="Return" %}
A list of Witness's mapping witness names to witness ids
{% endtab %}
{% endtabs %}

### list\_committee\_members

Lists all committee\_members registered in the blockchain. This returns a list of all account names that own committee\_members, and the associated committee\_member id, sorted by name. This lists committee\_members whether they are currently voted in or not.

Use the `lowerbound` and limit parameters to page through the list. To retrieve all committee\_members, start by setting `lowerbound` to the empty string `""`, and then each iteration, pass the last committee\_member name returned as the `lowerbound` for the next `list_committee_members()` call.

```cpp
map<string, committee_member_id_type> graphene::wallet::wallet_api::list_committee_members(
    const string &lowerbound, 
    uint32_t limit)
```

{% tabs %}
{% tab title="Parameters" %}
* **`lowerbound`**: the name of the first committee\_member to return. If the named committee\_member does not exist, the list will start at the committee\_member that comes after `lowerbound`
* **`limit`**: the maximum number of committee\_members to return \(max: 1000\)
{% endtab %}

{% tab title="Return" %}
A list of committee\_members mapping committee\_member names to committee\_member ids
{% endtab %}
{% endtabs %}

### create\_witness

Creates a witness object owned by the given account.

An account can have at most one witness object.

```cpp
signed_transaction graphene::wallet::wallet_api::create_witness(
    string owner_account, 
    string url, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`owner_account`**: the name or id of the account which is creating the witness
* **`url`**: a URL to include in the witness record in the blockchain. Clients may display this when showing a list of witnesses. May be blank.
* **`broadcast`**: true to broadcast the transaction on the network
{% endtab %}

{% tab title="Return" %}
The signed transaction registering a witness
{% endtab %}
{% endtabs %}

### update\_witness

Update a witness object owned by the given account.

```cpp
signed_transaction graphene::wallet::wallet_api::update_witness(
    string witness_name, 
    string url, 
    string block_signing_key, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`witness_name`**: The name of the witness’s owner account. Also accepts the ID of the owner account or the ID of the witness.
* **`url`**: Same as for create\_witness. The empty string makes it remain the same.
* **`block_signing_key`**: The new block signing public key. The empty string makes it remain the same.
* **`broadcast`**: true if you wish to broadcast the transaction.
{% endtab %}

{% tab title="Return" %}
The signed transaction
{% endtab %}
{% endtabs %}

### create\_worker

Create a worker object.

```cpp
signed_transaction graphene::wallet::wallet_api::create_worker(
    string owner_account, 
    time_point_sec work_begin_date, 
    time_point_sec work_end_date, 
    share_type daily_pay, 
    string name, string url, 
    variant worker_settings, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`owner_account`**: The account which owns the worker and will be paid
* **`work_begin_date`**: When the work begins
* **`work_end_date`**: When the work ends
* **`daily_pay`**: Amount of pay per day \(NOT per maint interval\)
* **`name`**: Any text
* **`url`**: Any text
* **`worker_settings`**: {“type” : “burn”\|”refund”\|”vesting”, “pay\_vesting\_period\_days” : x}
* **`broadcast`**: true if you wish to broadcast the transaction.
{% endtab %}

{% tab title="Return" %}
The signed transaction
{% endtab %}
{% endtabs %}

### update\_worker\_votes

Update your votes for workers.

```cpp
signed_transaction graphene::wallet::wallet_api::update_worker_votes(
    string account, 
    worker_vote_delta delta,
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account`**: The account which will pay the fee and update votes.
* **`delta`**: {“vote\_for” : \[…\], “vote\_against” : \[…\], “vote\_abstain” : \[…\]}
* **`broadcast`**: true if you wish to broadcast the transaction.
{% endtab %}

{% tab title="Return" %}
The signed transaction
{% endtab %}
{% endtabs %}

### vote\_for\_committee\_member

Vote for a given committee\_member.

An account can publish a list of all committee\_members they approve of. This command allows you to add or remove committee\_members from this list. Each account’s vote is weighted according to the number of shares of the core asset owned by that account at the time the votes are tallied.

{% hint style="warning" %}
**Note:** You can't vote against a committee\_member, you can only vote for the committee\_member or not vote for the committee\_member.
{% endhint %}

```cpp
signed_transaction graphene::wallet::wallet_api::vote_for_committee_member(
    string voting_account, 
    string committee_member, 
    bool approve, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`voting_account`**: the name or id of the account who is voting with their shares
* **`committee_member`**: the name or id of the committee\_member’ owner account
* **`approve`**: true if you wish to vote in favour of that committee\_member, false to remove your vote in favour of that committee\_member
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed transaction changing your vote for the given committee\_member.
{% endtab %}
{% endtabs %}

### vote\_for\_witness

Vote for a given witness.

An account can publish a list of all witnesses they approve of. This command allows you to add or remove witnesses from this list. Each account’s vote is weighted according to the number of shares of the core asset owned by that account at the time the votes are tallied.

{% hint style="warning" %}
Note: You can't vote against a witness, you can only vote for the witness or not vote for the witness.
{% endhint %}

```cpp
signed_transaction graphene::wallet::wallet_api::vote_for_witness(
    string voting_account, 
    string witness, 
    bool approve, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`voting_account`**: the name or id of the account who is voting with their shares
* **`witness`**: the name or id of the witness’ owner account
* **`approve`**: true if you wish to vote in favour of that witness, false to remove your vote in favour of that witness
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed transaction changing your vote for the given witness
{% endtab %}
{% endtabs %}

### set\_voting\_proxy

Set the voting proxy for an account.

If a user does not wish to take an active part in voting, they can choose to allow another account to vote their stake.

Setting a vote proxy does not remove your previous votes from the blockchain, they remain there but are ignored. If you later null out your vote proxy, your previous votes will take effect again.

This setting can be changed at any time.

```cpp
signed_transaction graphene::wallet::wallet_api::set_voting_proxy(
    string account_to_modify, 
    optional<string> voting_account, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_to_modify`**: the name or id of the account to update
* **`voting_account`**: the name or id of an account authorized to vote account\_to\_modify’s shares, or null to vote your own shares
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed transaction changing your vote proxy settings
{% endtab %}
{% endtabs %}

### set\_desired\_witness\_and\_committee\_member\_count

Set your vote for the number of witnesses and committee\_members in the system.

Each account can voice their opinion on how many committee\_members and how many witnesses there should be in the active committee\_member/active witness list. These are independent of each other. You must vote your approval of at least as many committee\_members or witnesses as you claim there should be \(you can’t say that there should be 20 committee\_members but only vote for 10\).

There are maximum values for each set in the blockchain parameters \(currently defaulting to 1001\).

This setting can be changed at any time. If your account has a voting proxy set, your preferences will be ignored.

```cpp
signed_transaction graphene::wallet::wallet_api::set_desired_witness_and_committee_member_count(
    string account_to_modify, 
    uint16_t desired_number_of_witnesses, 
    uint16_t desired_number_of_committee_members, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`account_to_modify`**: the name or id of the account to update
* **`desired_number_of_witnesses`**: desired number of active witnesses
* **`desired_number_of_committee_members`**: desired number of active committee members
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Result" %}
The signed transaction changing your vote proxy settings
{% endtab %}
{% endtabs %}

### propose\_parameter\_change

Creates a transaction to propose a parameter change.

Multiple parameters can be specified if an atomic change is desired.

```cpp
signed_transaction graphene::wallet::wallet_api::propose_parameter_change(
    const string &proposing_account, 
    fc::time_point_sec expiration_time, 
    const variant_object &changed_values, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`proposing_account`**: The account paying the fee to propose the tx
* **`expiration_time`**: Timestamp specifying when the proposal will either take effect or expire.
* **`changed_values`**: The values to change; all other chain parameters are filled in with default values
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed version of the transaction
{% endtab %}
{% endtabs %}

### propose\_fee\_change

Propose a fee change.

```cpp
signed_transaction graphene::wallet::wallet_api::propose_fee_change(
    const string &proposing_account, 
    fc::time_point_sec expiration_time, 
    const variant_object &changed_values, 
    bool broadcast = false)
```

{% tabs %}
{% tab title="Parameters" %}
* **`proposing_account`**: The account paying the fee to propose the tx
* **`expiration_time`**: Timestamp specifying when the proposal will either take effect or expire.
* **`changed_values`**: Map of operation type to new fee. Operations may be specified by name or ID. The “scale” key changes the scale. All other operations will maintain current values.
* **`broadcast`**: true if you wish to broadcast the transaction
{% endtab %}

{% tab title="Return" %}
The signed version of the transaction
{% endtab %}
{% endtabs %}

