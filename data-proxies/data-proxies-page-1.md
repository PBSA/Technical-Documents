# Introduction to Data Proxies

## Overview

The Data Proxy serves as a middle man between the Data Feed Providers \(DFPs\) and the Bookie Oracle System \(BOS\) operated by the Witnesses. 

The simplest way to understand this relationship is knowing that as BOS requires all data it receives to be parsed/ normalized to the exactly the same format, a process needs to exist to make this happen. This 'process' is the data proxy.

Each DFP provides data on sports events in some format, but no two DFPs might use the same format, or necessarily support the same sports and events. Both Data Proxies and BOS use the [Bookiesports](../bookie-oracle-suite-bos/bookiesports/) module to manage this common format and ensure the consistency of data, regardless of how many Data Proxies are operating.



and analyzed for the appearance of four triggers, also called incidents within the data proxy: Creation, Start, End and Result verification of an event.

Those incidents are normalized into a common format via [bookiesports](https://pypi.python.org/pypi/bookiesports) and then sent to the subscribed witnesses of the data proxy. All data incoming from data providers and sent out incidents are stored within the data provider in the "dump" sub folder.

Note: Sports, EventGroups, Betting Market Groups \(BMGs\) and Betting Markets are automatically created via bookie sync. Only events are affected by the Data Proxy.

## Monitoring proper operation

The Data Proxy provides a HTTP endpoint for monitoring purposes. Assume that the Data Proxy is deployed on localhost:8010, then the URL is

localhost:8010/isalive

The response has a json body that has three main contents:

* status: String flag either "ok" or "nok", general state of the proxy
* subscribers: List of dictionaries containing information of each subscriber. Contains a status flag as well
* providers: List of dictionaries containing information of each provider. Contains a status flag as well

This isalive can be called from localhost and from anywhere. No identifiable information on providers or subscribers is published when queried from anywhere. Details are added when it is called from localhost.

