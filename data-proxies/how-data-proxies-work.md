# How Data Proxies Work

## Incidents and Triggers

There are five triggers, also called incidents, that control the flow of data to BOS from each Data Proxy.

* `create`  &lt;game&gt; created
* `in_progress`  &lt;game&gt; started
* `finish`  &lt;game&gt; finished
* `result` &lt;game&gt; score
* `canceled` &lt;game&gt; canceled / postponed / abandoned

Where &lt;game&gt; represents any game in the context of BookiePro. It's worth calling this out because right now we only think of Data Proxies in the context of BookiePro, and therefore sports data, a Data Proxy could in theory send other types of data. For example, as long as BOS receives a 'winner' and a 'loser' that data could be anything from the outcome of a coin toss to the winner of a general election!

{% hint style="warning" %}
**Note**: Sports, EventGroups, Betting Market Groups \(BMGs\) and Betting Markets are automatically created via bookie sync. Only events are affected by the Data Proxy.
{% endhint %}

### Mapping Incidents

Each Data Feed Provider will send data to a Data Proxy according to it's own rules, and not necessarily a direct mapping to the four triggers BOS expects. For example, a DFP might send a single message for both a finished game and the result. 

It would then be the job of the Data Proxy to re-format this data in to the two messages that BOS is expecting.

DFPs will also use their own status codes for the events that they send, and these status codes won't match the triggers that BOS uses. Once again it is the job of the Data Proxy to normalize this data. The following is an example of the status codes used by a DFP:

```text
|1|Not yet started|The event hash not yet started|
|2|In progress|The event is live|
|3|Finished|The event is finished|
|4|Cancelled|The event hash been cancelled|
|5|Postponed|The event hash been postponed|
|6|Interrupted|The event has been interrupted|
|7|Abandoned|The event hash been abandoned|
|8|Coverage lost|The coverage for this event has been lost|
|9|About to start|The event has not started but is about to.
```

Using the triggers supported by BOS it then requires the Data Proxy to map these codes in a similar manner to the following:

```text
|1| --> (not handled)
|2| --> in_progress
|3| --> finish
|4| --> canceled
|5| --> canceled
|6| --> canceled
|7| --> canceled
|8| --> (not handled)
|9| --> (not handled)
```

## Monitoring proper operation

The Data Proxy provides a HTTP endpoint for monitoring purposes. Assume that the Data Proxy is deployed on localhost:8010, then the URL is

localhost:8010/isalive

The response has a json body that has three main contents:

* `status`: String flag either "ok" or "nok", general state of the proxy
* `subscribers`: List of dictionaries containing information of each subscriber. Contains a status flag as well
* `providers`: List of dictionaries containing information of each provider. Contains a status flag as well

This isalive can be called from localhost and from anywhere. 

{% hint style="danger" %}
**Important**: No identifiable information on providers or subscribers is published when queried from anywhere. Details are only added when it is called from localhost.
{% endhint %}

