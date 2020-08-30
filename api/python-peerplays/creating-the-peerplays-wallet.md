# Creating the wallet

All these experiments and examples are expected to be tried out in ipython command shell. Start the command shell with `ipython`

Node is the chain with which you wish to interact.

```text
node = "wss://elizabeth.peerplays.download/api"
```

`password = "password"`

`from peerplays import PeerPlays`

```
p = PeerPlays(node)
```

To create a new wallet

`p.newWallet(password)`

Unlock the wallet

`p.unlock(password)`

Add private key / keys

`p.wallet.addPrivateKey("5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3")`

\`\`

## Additional Methods

`p.wallet.unlocked() #returns True if wallet is unlocked. Wallet needs to be unlocked to perform operations on the blockchain.`

`p.wallet.getAccounts() #Lists all the accounts associated with the private key.`

