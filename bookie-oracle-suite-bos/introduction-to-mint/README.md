# Manual Intervention Tool \(MINT\)

The Manual Intervention Module \(MINT\) provides a web interface for Witnesses to manually intervene in the otherwise fully-automated process of bringing Bookie Events, BMGs, and Betting Markets to the Peerplays blockchain \(through bos-auto\). 

Occasionally, there may be a need for manual intervention in some element of BOS’s operation. One example would be when multiple Data Proxies are not able to provide the final result of a game \(due to sustained connectivity issues or, further back down the chain, an issue with the reporting of the match to the third party data feeds\). In this situation, a Peerplays Witness can use the Manual Intervention Module \(MINT\) which is part of the BOS suite.

MINT provides a web interface for Witnesses to manually intervene in the otherwise fully-automated process of bringing Bookie events and betting markets to Bookie. Some of the functions that MINT can be used to perform:

* turn an event ‘in-progress’ \(match has started\) or a betting market ‘in-play’
* freeze an event or betting market
* cancel an event or betting market
* settle a betting market \(i.e. decide winners and losers\)

It is important to remember that changes made using MINT do not automatically become ‘fact’ on Bookie. As with BOS’s automated operation, data sent by a particular Witness using MINT is representative of the ‘opinion’ of only that Witness. It requires a consensus amongst a simple majority \(50%+1\) of **all** Witnesses for that ‘opinion’ to be accepted as ‘fact’ on Bookie.

MINT also allows for the creation of new events and betting markets. This allows for Bookie to offer betting on longer-term markets like “Who will win the Superbowl?” at the start of a new NFL season \(called ‘Futures’ in North America, or ‘Ante-Post’ betting in the UK\). These markets do not lend themselves to fully automated management by BOS but are an important part of any sports betting offering.

The ability to create new games or events using MINT also opens up the possibility of Bookie offering ‘novelty’ betting markets where it is not feasible to implement fully-automated data management. 

