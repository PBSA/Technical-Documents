---
description: The page to explain the operation fee calculation
---

# Cost Estimation

The cost for each operation can be calculated using a function called _**`get_global_properties`**_. Each operation cost varies and change over time due to blockchain governance voting. The final value will be calculated based on the type of operation and the total number of runs that operation has to be executed. The fees value are in terms of PPYs.

The following list of points has to be taken into consideration while calculating the operation cost,&#x20;

1. Execute the _**`get_global_properties`**_ in the cli wallet and copy the list of operations ID's/Fees from the return value.
2. Download the latest copy of Operation ID mapping. The below link directs to the latest list of operations,\
   [https://devs.peerplays.com/supporting-and-reference-docs/operation-ids-list](https://devs.peerplays.com/supporting-and-reference-docs/operation-ids-list)
3. Combine the two lists to derive the fee for each operation.

Click the below link to learn about the operation cost calculation in detail,

{% embed url="https://devs.peerplays.com/development-guides/calculating-costs" %}

This page explains about the fees estimation, function execution, and a detailed example to understand the fees calculation.
