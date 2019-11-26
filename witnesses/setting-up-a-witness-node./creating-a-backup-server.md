# Creating a Backup Server

**Simple guide for witnesses creating a backup server and switching without missing blocks:  
The same process can be applied to a steem witness node  
Replace joseph-witness to your witness name.  
Replace public/private keys with yours.**  


First: you created your witness node and now running it according to these directions:

{% page-ref page="./" %}



If you followed the instructions you will end up with something like this in config.txt

```text
witness-id = "1.6.41"
private-key = ["PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR","private-key"]
```

and you will be signing blocks with that key.

Second: you will compile another node on server2 following the same instructions like before, only this time, you generate a new witness key using the command  
" suggest\_brain\_key "  
lets say the new public key was  
" PPY5tPcG5AX6YFYsBR739JNxFzmDWUgrw6BZFBxfKRsTKAx4QiA76 "

your config.txt on server2 should have this:

```text
witness-id = "1.6.41"
private-key = ["PPY5tPcG5AX6YFYsBR739JNxFzmDWUgrw6BZFBxfKRsTKAx4QiA76","private-key"]
```

When you run the node on server2 you will see this message:  
  


```text
[block_production_loo ] Not producing block because I don't have the private key for 
PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR
```

  
  
This is normal because we did not run the update\_witness command yet.

Now let's say you need to update server1 when the code on github is updated

First you run the update\_witness command in your CLI like this:

```text
update_witness joseph-witness "https://steemit.com/peerplays-witness/@joseph/peerplays-blockchain-witness-proposal" "PPY5tPcG5AX6YFYsBR739JNxFzmDWUgrw6BZFBxfKRsTKAx4QiA76" true
```

soon after, your server1 will show this message:

```text
[block_production_loo ] Not producing block because I don't have the private key for 
PPY5tPcG5AX6YFYsBR739JNxFzmDWUgrw6BZFBxfKRsTKAx4QiA76
```

  
  
and info on your account for signing key will look like this  
  


```text
    "signing_key": "PPY5tPcG5AX6YFYsBR739JNxFzmDWUgrw6BZFBxfKRsTKAx4QiA76",
    "name": "joseph-witness",
```

  
  
You just pulled a quick switch from server1 to server2 without missing any blocks. Now you can safely shut down server1 and update the code. once the code is updated and the node is restarted, you can switch back signing blocks to server1 using the update\_witness command again with public-key for server1.  
  


```text
update_witness joseph-witness "https://steemit.com/peerplays-witness/@joseph/peerplays-blockchain-witness-proposal" "PPY7ZBgUduxjAsk5zXLeZX8skTHAjqQAyu6NnoSDKHs2Utm5XsDLR" true
```

#### Important:

As of now the TX fee for the update\_witness command is 0.5 PPY that's a high cost and should be much lower.  
I spoke with Jonathan today and he said the fee for update\_witness should be much lower. Until that is fixed, make sure you have at least 1 PPY in your account to execute two update\_witness commands.

It's always a good idea to maintain a backup witness server for block production, in case of downtime on one server or the need to update code without shutting down your production node. A back up server will also be handy in case of an attack on the network.

