# Introduction to BOS

The Bookie Oracle System, or BOS, is a unique decentralized sports feed oracle system originally designed for the BookiePro dApp.

Unlike traditional centralized sports betting applications that rely on perhaps one or two data feeds, Bookie, through BOS, has the potential for almost unlimited data feeds through the use of data proxies.

At its simplest, a data proxy is just middleware that consumes it's own data feed \(usually from a commercial supplier\) then parses and normalizes that data before sending it to BOS.

Since BOS supports multiple data proxies, it therefore supports multiple data feed providers.

This is where BOS becomes truly decentralized. There is no single source of truth as far as the sports data is concerned. BOS, either automatically or through manual intervention by Witnesses requires a consensus of at least 2 approvals in most cases, before processing incident/event data.

For example:

A soccer game takes place between two teams and there are three data proxies interfaced to BOS.

Each data proxy sends the result of the game to BOS, two send the result as 2 - 1, but one sends the result as 2 - 2. In this instance BOS will process the result as 2-1 because **at least two** data proxies agree that this is the correct score.

However, if only two data proxies were reporting to BOS, and they gave different scores, that game would not get automatic approval and instead would require a manual proposal by the Witnesses using the manual intervention tool \(MINT\). 

## High Level Structure

![](../.gitbook/assets/bos-flow.jpg)

An overview of some of the elements in this diagram:

_**bos-auto**_  
This service provides the endpoints that receive incidents from Data Proxies, triggers that distinguish incidents according to their information, as well as a worker that processes the triggers and incidents and synchronizes them on the Peerplays blockchain by means of bos-sync and BookieSports.

_**bos-mint**_  
The Manual Intervention Module \(MINT\) provides a web interface for Witnesses to manually intervene in the otherwise fully-automated process of bringing Bookie Events, BMGs, and Betting Markets to the Peerplays blockchain \(through bos-auto\). This allows Witnesses to handle any edge cases that may arise and cannot be dealt with by bos-auto.

_**python-peerplays**_  
This is a communications library which allows interface with the Peerplays blockchain directly and without the need for a cli\_wallet. It provides a wallet interface and can construct any kind of transactions and properly sign them for broadcast.

_**bookiesports**_  
bookiesports is essentially a set of rules and recommendations about the sports, leagues, competitions, and betting markets that should be offered on Bookie. bookiesports also provides configuration information regarding betting market formats, along with rules and grading algorithms used to settle markets on Bookie. Use of bookiesports allows Bookie to provide a coherent product offering that meets the expectations of the sports betting consumer. bookiesports is also used by Data Proxies for standardization of Sport, Event Group, and team/competitor names.

_**bos-sync**_  
The bos-sync module is involved to the blockchain either through creating a proposal or by approving an existing proposal. This module is the heart of BOS and provides a library with an easy-to-use programming interface that hooks straight into bookiesports.

_**bos-incidents**_  
This module stores incoming incidents from Data Proxies and is also integrated with bos-mint \(MINT\). This module integrates a database that persistently stores incidents and allows the tracking of status changes. 

For more information on MINT see:

{% page-ref page="introduction-to-mint/" %}



