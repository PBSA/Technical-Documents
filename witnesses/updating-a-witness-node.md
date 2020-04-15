# Updating a Witness Node

There will be many occasions when a node has to be updated with new code. These software updates can be broken down as "soft forks' or "hard forks". 

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

STEP 2 - Get the Release

Each new release will be published, and tagged, in the Peerplays public repository ready for download.

For example, the following is the code for a test release:

{% embed url="https://github.com/peerplays-network/peerplays/releases/tag/test-1.4.4" %}

{% hint style="warning" %}
**Note**: The above release is just an example, each release will have a different tag that will be provided.
{% endhint %}

Download the code from the provided tag / link.



STEP 2 - Setting Up a Node

Follow this guide to

