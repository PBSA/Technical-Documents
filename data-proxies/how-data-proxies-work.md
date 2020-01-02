# How Data Proxies Work

## Incidents and Triggers

There are four triggers, also called incidents, that control the flow of data to BOS from each Data Proxy.

* `create`  &lt;game&gt; created
* `in_progress`  &lt;game&gt; started
* `finish`  &lt;game&gt; finished
* `result` &lt;game&gt; score

 Where &lt;game&gt; represents any game \(market\) in the context of BookiePro. It is worth calling this out because while we only think of Data Proxies right now in the context of BookiePro, and therefore sports data, a Data Proxy could in theory send other types of data. For example, as long as BOS receives a 'winner' and a 'loser' that data could be anything from the outcome of a coin toss to the winner of a general election!

All data incoming from data providers and sent out incidents are stored within the data provider in the "dump" sub folder.

{% hint style="warning" %}
Note: Sports, EventGroups, Betting Market Groups \(BMGs\) and Betting Markets are automatically created via bookie sync. Only events are affected by the Data Proxy.
{% endhint %}

## 

## Monitoring proper operation

The Data Proxy provides a HTTP endpoint for monitoring purposes. Assume that the Data Proxy is deployed on localhost:8010, then the URL is

localhost:8010/isalive

The response has a json body that has three main contents:

* status: String flag either "ok" or "nok", general state of the proxy
* subscribers: List of dictionaries containing information of each subscriber. Contains a status flag as well
* providers: List of dictionaries containing information of each provider. Contains a status flag as well

This isalive can be called from localhost and from anywhere. No identifiable information on providers or subscribers is published when queried from anywhere. Details are added when it is called from localhost.

