---
description: Running a backup node on another server to prevent downtime
---

# Backup Servers

When running a node, it's important to minimize downtime. This can be difficult when you have to stop the node to perform upgrades to the software or to troubleshoot an issue. This is where having a backup server comes in handy.

This document will explain how to set up a backup server that runs in parallel to your main server. When necessary, you can quickly switch to your backup node so users will not experience any downtime. Then you can perform your maintenance and switch back once complete.

## 1. Creating a Backup Server

In this tutorial, it's assumed that you have a Witness node up and running according to the Witness node documentation. As such, you'll have your witness-id and public-private key-pair containing the signing key within the `config.ini` file. As an example, you might have the following in the config:

```text
witness-id = "1.6.41"
private-key = ["PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR","some-private-key"]
```

### 1.1. Step 1: Install a Witness Node on a Second Server

Just like the first time you installed a witness node, you'll need to do it again on a separate server. You can follow the same instructions provided in the witness node documentation.

The important part of the second install is to generate another signing key, different from the first. To do this, you'll use the `suggest_brain_key` command:

```text
suggest_brain_key
```

Which will return something like this:

```text
suggest_brain_key
{
  "brain_priv_key": "SECOHM OBESELY REVERER YARD CAZA TYROSYL HATTING MYOTOMY FARDO WANGHEE ACOLYTE TOPKNOT DETENT THICK BOODIE TEMPRE",
  "wif_priv_key": "5JCMoqthZ95S6NJ16fqMs18qZrkegWTW9xgAxBKG3LeseorGv9e",
  "pub_key": "PPY8gjHZE13qLmSUmvMGR8mZWcD1ejLUkPhg4j9bNG6bRzWjZSUBM"
}
```

And then you'll use the `wif_priv_key` in the output above in the `config.ini` file on the second server, for example:

```text
witness-id = "1.6.41"
private-key = ["PPY8gjHZE13qLmSUmvMGR8mZWcD1ejLUkPhg4j9bNG6bRzWjZSUBM","5JCMoqthZ95S6NJ16fqMs18qZrkegWTW9xgAxBKG3LeseorGv9e-key"]
```

At this point you'll see the following message on the second server if you were to run that node:

```text
[block_production_loo ] Not producing block because I don't have the private key for 
PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR
```

Since you haven't switched the nodes yet, that's what we expect.

### 1.2. Step 2: Making the Switch

To flip the node production, you'll run the `update_witness` command in the cli\_wallet:

```text
update_witness example-witness "https://example.com/peerplays-blockchain-witness-proposal" "PPY8gjHZE13qLmSUmvMGR8mZWcD1ejLUkPhg4j9bNG6bRzWjZSUBM" true
```

{% hint style="success" %}
Congrats! You updated your witness to use the new signing key! 🎉 
{% endhint %}

Soon after you'll see this message on your first server:

```text
[block_production_loo ] Not producing block because I don't have the private key for 
PPY8gjHZE13qLmSUmvMGR8mZWcD1ejLUkPhg4j9bNG6bRzWjZSUBM
```

Your second server will now be producing blocks. You just pulled a quick switch from server 1 to server 2 without missing any blocks. Now you can safely shut down server 1 and update the code. once the code is updated and the node is restarted, you can switch back signing blocks to server 1 using the `update_witness` command again with the public-key for server 1.

```text
update_witness example-witness "https://example.com/peerplays-blockchain-witness-proposal" "PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR" true
```

It's always a good idea to maintain a backup witness server for block production, in case of downtime on one server or the need to update code without shutting down your production node. A back up server will also be useful in case of an attack on the network.

