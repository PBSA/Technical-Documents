# Creating the wallet

All these experiments and examples are expected to be tried out in ipython command shell. Start the command shell with `ipython`

Node is the chain with which you wish to interact.

`node = "wss://irona.peerplays.download/api"`

`password = "password"`

`from peerplays import PeerPlays`

```
p = PeerPlays(node, 
        # proposer="init0",
        # proposal_expiration=300,
        # nobroadcast=False,
        # bundle=False
    )
```

The commented out lines are optional.

To create a new wallet

`p.newWallet(password)`

Unlock the wallet

`p.unlock(password)`

\`\`

