---
description: Introduction
---

# Python Peerplays

Its the Python library for communicating with the Peerplays blockchain.

The code is maintained at [https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays](https://gitlab.com/PBSA/PeerplaysIO/tools-libs/python-peerplays)



1.2.x : accounts

1.3.x : assets



Fund Transfer

p.transfer\("jemshid", 100, "TEST"\)



```text
def transfer(self, to, amount, asset, memo="", account=None, **kwargs):
```

p.rpc.get\_account\_balances\("jemshid", \[\]\)



get\_global\_properties







