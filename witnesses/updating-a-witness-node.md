# Updating a Witness Node

There will be many occasions when a node has to be updated with new or modified features. These software updates can be categorized as "soft forks" or "hard forks". 

#### Soft Fork

A soft fork is a software update that is compatible with earlier versions, in other words it’s backward compatible. A soft fork contains no new operation or updates to existing operations on the blockchain.

Generally a soft fork provides an update to an existing feature that isn't relevant to core blockchain operations.

#### Hard Fork

A “hard fork” is a software update that isn’t backwards compatible, so any blocks coming after the activation of the software update will have to follow the new rules in order to be considered valid.

A hard fork is required whenever there is need for introducing new operation\(s\) or updating existing operation\(s\) on the blockchain. Each hard fork is time bound to update nodes and all Witnesses will be expected to finish the update before this date/time.

For example, if an update was released that required a hard fork and the hard fork date/time was set to be Jan 01, 2020 12:00EST, then every witness node must be updated before that date/time.

## Updating The Node

To update a Witness node use the following steps:

### STEP 1 - Make a Backup

The current blockchain `witness_node` binary \(.exe\) should be backed up in such a way that if there is a serious problem with the update then the node can be rolled-back easily.

### STEP 2 - Get the Release

Each new release will be published, and tagged, in the Peerplays public repository ready for download.

For example, the following is the code for test release 1.4.4:

{% embed url="https://github.com/peerplays-network/peerplays/releases/tag/test-1.4.4" %}

{% hint style="warning" %}
**Note**: The above release is just an example, each release will have it's own tag / link.
{% endhint %}

Download the code from the provided tag / link.

### STEP 3 - Building the Witness Node

Follow this guide to build the release from [Step 2](updating-a-witness-node.md#step-2-get-the-release).

{% page-ref page="setting-up-a-witness-node/" %}

### STEP 4 - Start the Witness Node

After the Witness node has been built / compiled it needs to be started and the data replayed.

Run the following command:

```text
./programs/witness_node/witness_node --replay-blockchain
```

If there are any issues during this step then a data resync should be run instead to download blocks from the seed nodes.

```text
./programs/witness_node/witness_node --resync-blockchain
```

### STEP 5 - Swap Nodes

Finally the active node needs to be swapped for the newly built node.

