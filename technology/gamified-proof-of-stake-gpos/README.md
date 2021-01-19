# Gamified Proof of Stake \(GPOS\)

## Introduction

Building on the success of the DPOS consensus mechanism, Peerplays introduced a unique enhancement called Gamified Proof of Stake \(GPOS\). This idea was first proposed as [Peerplays Improvement Proposal \(PIP\) \#2](https://github.com/peerplays-network/pips/blob/master/pip-0002.md) in January 2019.

The original intent of Peerplays was to operate as a Decentralized Autonomous Cooperative \(DAC\) where DPOS enables the voting collective of core token holders to determine who would act as Advisors, Witness, and Proposals within Peerplays. However, like other DPOS based blockchains, the challenges of voter turnout continue to plague Peerplays. 

## What Makes GPOS Different?

GPOS made a protocol change such that PPY token holders now receive a _participation reward_  based on their voting performance and how many PPY they have vested or _staked_.

This is a significant change from the original protocol where token holders were rewarded with their share of a rake, taken from a percentage of the blockchain fees, and then distributed to token holders relative to their token holdings, regardless of any voting participation.

Put simply, this means that each PPY token holder needs to vest some, or close to all, of their PPY balance towards GPOS and then once a month vote for either Witnesses, Advisors or Proxies.

## Why is GPOS Important?

Being a DPOS consensus blockchain, the importance of voter participation from the PPY token holders is paramount to the security of the blockchain. The introduction of GPOS ensures that token holders will take an active interest in the operation and governance of Peerplays.

Each vote cast impacts the people behind the successful operation of Peerplays, in particular the  Witnesses. A Witness receiving high votes is much more likely to be elected as a block producing \(active\) Witness.

Under DPOS the Witness position is more secure because a lot less token holders are voting and subsequently it might just take one or two major PPY token holders to influence the election of a Witness. Under GPOS all token holders should participate making it a much more democratic process, and also receive rewards for voting.

## Participation Rewards

Participation rewards combined with voting performance are what makes the Peerplays consensus mechanism _gamified._

The rewards are calculated as follows:

### **Qualified Reward %**

This is the percentage of a user's maximum possible reward received based on their voting performance. This reward _decays_ at a rate of 16.67% per month. For example, if a token holder doesn't vote for a month the qualified reward percentage drops to 83.33% and if the token holder doesn't vote for six consecutive months the reward will be 0%.

{% hint style="warning" %}
**Note**: Complete decay was set at six consecutive months to coincide with the dividend distribution periods.

Below are the some of the key GPOS parameters that are currently set on mainnet.

GPOS period = 6 months\(180 days\)

GPOS sub-period = 30 days

GPOS lock-in period = 30 days

Vesting fee = 1PPY

Withdrawal fee = 0.01PPY
{% endhint %}

### **Estimated Rake Reward %**

This is the potential percentage reward a user could receive based on the [qualified reward percentage](./#qualified-reward), the amount of PPY they have vested and their share of the total GPOS balance. 

So the estimated rake reward is calculated as:

GPOS balance = b  
Total GPOS balance on blockchain = TB  
Qualified reward % = q

Estimated Rake Reward% = \(b / TB\) \* q

**For example:**

A user has a GPOS balance of 1,000PPY and is entitled to 100% of their reward based on voting performance. The total GPOS balance on the blockchain is 4,000,000PPY.

The user would receive the following percentage of the rake:

\(1,000 / 4,000,000\) \* 100% = 0.025%

So if the total \(month\) rake was 100,000PPY then the user would receive 25PPY.

## GPOS related cli_wallet commands

**create_vesting_balance account amount asset_symbol vesting_type broadcast**

Creates vesting balance of GPOS type, enables user to participate in GPOS

Example:
```
create_vesting_balance account_name 50 PPY gpos true
```

**vote_for_witness account witness approve broadcast**

Cast or withdraw vote for a witness by given account. To keep user vesting performance, it is best for user to vote in each GPOS subperiod

Example:
```
# Cast a vote for a witness
vote_for_witness account_name witness_name true true

# Withdraw a vote for a witness
vote_for_witness account_name witness_name false true
```

**withdraw_GPOS_vesting_balance account amount asset_symbol broadcast**

Once the vesting period expires, user is able to collect his reward with withdraw_GPOS_vesting_balance command. User is not allowed to withdraw his balance before vesting period expires. Default GPOS vesting period is 30 days.

Example:
```
withdraw_GPOS_vesting_balance account_name 10 PPY true
```

**How to retrieve GPOS info**

Some GPOS info is stored in global properties object. Retrieve GPO and inspect the following values - gpos_period, gpos_subperiod, gpos_period_start and gpos_vesting_lockin_period.

Example:
```
# Output is shortened, check out "extensions" section
get_global_properties 
{
  "id": "2.0.0",
  "parameters": {
...
    "maximum_tournament_start_delay": 604800,
    "maximum_tournament_number_of_wins": 100,
    "extensions": {
      "sweeps_distribution_percentage": 200,
      "sweeps_distribution_asset": "1.3.0",
      "sweeps_vesting_accumulator_account": "1.2.0",
      "gpos_period": 15552000,                   <----- Length of GPOS period (180 days)
      "gpos_subperiod": 2592000,                 <----- Length of GPOS subperiod (30 days)
      "gpos_period_start": 1609376400,           <----- Epoch time of last GPOS subperiod start (2020-12-31 1:00:00)
      "gpos_vesting_lockin_period": 2592000,     <----- GPOS Vesting period (30 days)
      "rbac_max_permissions_per_account": 5,
      "rbac_max_account_authority_lifetime": 15552000,
      "rbac_max_authorities_per_permission": 15,
      "account_roles_max_per_account": 20,
      "account_roles_max_lifetime": 31536000,
...
    }
  },
...
}
```

**How to check account's last voting time**

Account's last voting time is stored in account's statistic object. To retrieve the account statistic object, user needs account statistic object id, which is stored in account object.

Example:
```
# Get the account object first, and retrieve statistic object id
get_account account_name
{
  "id": "1.2.20",
  "membership_expiration_date": "1970-01-01T00:00:00",
  "registrar": "1.2.18",
  "referrer": "1.2.18",
  "lifetime_referrer": "1.2.18",
  "network_fee_percentage": 2000,
  "lifetime_referrer_fee_percentage": 3000,
  "referrer_rewards_percentage": 0,
  "name": "account_name",
  ...
  "statistics": "2.6.20",          <--- HERE IT IS!!!
  "whitelisting_accounts": [],
  "blacklisting_accounts": [],
  "whitelisted_accounts": [],
  "blacklisted_accounts": [],
  "owner_special_authority": [
    0,{}
  ],
  "active_special_authority": [
    0,{}
  ],
  "top_n_control_flags": 0
}

# Retrieve the account's statistic object
get_object 2.6.20
[{
    "id": "2.6.20",
    "owner": "1.2.20",
    "name": "account_name",
    "most_recent_op": "2.9.0",
    "total_ops": 0,
    "removed_ops": 0,
    "total_core_in_orders": 0,
    "core_in_balance": "5000000000000",
    "has_cashback_vb": false,
    "is_voting": false,
    "lifetime_fees_paid": 0,
    "pending_fees": 0,
    "pending_vested_fees": 0,
    "last_vote_time": "1970-01-01T00:00:00"     <--- HERE IT IS
  }
]

```
