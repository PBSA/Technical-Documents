# Configuration of bos-auto

### Configuration of bos-auto

{% hint style="danger" %}
Warning: At this point it's crucial to set the default witness node to your own server \(ideally running in`localhost`, see below config.yaml\) using `peerplays set node ws://ip:port`. If this step is skipped the setup will not work, or at best will work with very high latency.
{% endhint %}

#### Setup Your python-peerplays Wallet

```text
# you will be asked to provide a new wallet passphrase. Later in the
# tutorial you will be asked to store that password in a file
# (config.yaml)
peerplays createwallet

# to add the key we need to make the node known (preferably on localhost)
peerplays set node ws://localhost:8090

peerplays addkey
# You will be prompted to enter your active private key for the witness
```

#### Funding the Account

Since your Witness account is going to create and approve proposals automatically,  you need to ensure that the Witness account is funded with PPY.

#### Modify Configuration

We now need to configure bos-auto:

```text
wget https://raw.githubusercontent.com/peerplays-network/bos-auto/master/config-example.yaml
mv config-example.yaml config.yaml
# modify config.yaml
```

The variables are described below:

The following options need to be set:

`node: ws://localhost:8090`. If not running a local installation then change this to any Testnet \(Beatrice\) API node.

`network: beatrice.` Only change if you're not using this Testnet.

{% hint style="danger" %}
**Important**: Make sure you set a Redis password during the Redis installation.
{% endhint %}

```text
# Please see bos_auto/config-defaults.yaml for description

node: ws://localhost:8090

network: beatrice

redis_password: <your redis password>

passphrase: <your python peerplays wallet password>

BOOKIE_PROPOSER: <your witness account name>

BOOKIE_APPROVER: <your witness account name>
```

