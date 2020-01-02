# Introduction to Data Proxies

## Overview

The Data Proxy serves as a middle man between the Data Feed Providers \(DFPs\) and the Bookie Oracle System \(BOS\) operated by the Witnesses. 

The simplest way to understand this relationship is knowing that as BOS requires all data it receives to be parsed/ normalized to the exactly the same format, a process needs to exist to make this happen. This 'process' is the data proxy.

Each DFP provides data on sports events in some format, but no two DFPs might use the same format, or necessarily support the same sports and events. Both Data Proxies and BOS use the [Bookiesports](../bookie-oracle-suite-bos/bookiesports/) module to manage this common format and ensure the consistency of data, regardless of how many Data Proxies are operating.

The normalized data is then sent to the subscribed Witnesses.

![](../.gitbook/assets/data-proxies.png)

## Decentralization of Data

and analyzed for the appearance of 

## 

