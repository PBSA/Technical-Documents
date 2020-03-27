# Proxy Payment Considerations

The Couch Potato concept relies on a number users operating as data feed providers for their own data proxies \(Couch Potato instances\).

In return for the Couch Potato user's participation they would rewarded according to a payment structure.

This document proposes how this payment structure could work and the challenges of a 'definition of success'.

At it simplest the payment structure could be something like, for every successful incident a user submits they'll be paid $1. On paper that's simple, but what do we mean by a 'successful' incident.

BOS operates by receiving a series of incidents from all of the Data Proxies. These incidents are sometimes called triggers or messages, but basically they're just JSON sent to BOS though a RESTful web service. What's important to us are the number of incidents and what they represent, as follows:



