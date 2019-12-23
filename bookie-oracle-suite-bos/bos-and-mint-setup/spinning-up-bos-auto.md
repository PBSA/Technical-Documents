# Spinning Up bos-auto

Now that bos-auto has been configured we want to make sure it works correctly. To do this, we need to start two processes:

1. An endpoint that takes incident reports from the data proxy and stores them in MongoDB as well as issues work for the worker via Redis.
2. The worker then takes those incidents and processes them.

{% hint style="warning" %}
**Note**: It is recommended to run both via system services.
{% endhint %}

The commands shown are for production installation, for debug installation replace `“bos-auto”` with `“python3 cli.py”`.

{% hint style="warning" %}
**Note**: Former installations also required to run the scheduler as a separate process. This is no longer necessary, it is spawned as a subprocess.
{% endhint %}

### Start the Endpoint

This is a basic setup and uses the flask built-in development server, see Production Deployment below.

{% hint style="danger" %}
**Important**: Before executing the next command make sure that your node is set to the correct environment. For example, if the installation is for Testnet \(Beatrice\) run:

`peerplays set node <Beatrice Node>` where &lt;Beatrice node&gt; is any Beatrice API node.
{% endhint %}

```text
cd bos-auto
bos-auto api --host 0.0.0.0 --port 8010       [--help for more information]
```

After this, if it's set up correctly you'll see the following messages:

> INFO \| Opening Redis connection \(redis://localhost/6379\) \* Running on http://0.0.0.0:8010/ \(Press CTRL+C to quit\)

This means that you can now send incidents to http://0.0.0.0:8010/.

### **Testing**

You can test that the endpoint is properly running with the following command:

```text
curl http://localhost:8010
```

If the endpoint is running, the API daemon will print the following line:

```text
127.0.0.1 - - [26/Apr/2018 14:19:45] "GET / HTTP/1.1" 404 -
```

At this point, we are done with setting up the endpoint and can go on to setting up the actual worker.

### **Delivery to Data Proxies**

Data proxies are interested in this particular endpoint as they will push incidents to it. This means that you need to provide them with your IP address as well as the port that you opened above.

For more information on Data Proxies see:

{% page-ref page="../../data-proxies/data-proxies-page-1.md" %}

### **Monitoring**

The endpoint has an `isalive` call that should be used for monitoring:

```text
curl http://localhost:8010/isalive
```

which produces an output like:

```text
{
   "background": {
      "scheduler": True
   },
   "queue": {
      "status": {
         "default": {
            "count": 1
         },
         "failed": {
            "count": 0
         }
      }
   },
   "versions": {
      "bookiesports": "0.2.6",
      "bos-auto": "0.1.10",
      "bos-incidents": "0.1.5",
      "bos-sync": "0.1.8",
      "peerplays": "0.1.32"
   }
}
```

Of interest here are the  listed versions and `queue.status.default.count`. 

The count should be zero most of the time, it reflects how many unhandled incidents are currently in the cache.

### **Production deployment**

Going into production mode, a Witness may want to deploy the endpoint via UWSGI, create a local socket and hide it behind an SSL supported nginx that deals with a simple domain instead of `ip:port` pair, like `https://dataproxy.mywitness.com/trigger`.

### Start worker

{% hint style="danger" %}
**Important**: At this point it's crucial to set the default Witness node to your own server \(ideally running in `localhost`\) using `peerplays set node ws://ip:port`. If this step is missed, the setup will not work or, at best, will work with very high latency.
{% endhint %}

Start the worker with the following commands:

```text
cd bos-auto
bos-auto worker      [--help for more information]
```

It will already try to use the provided password to unlock the wallet and, if successful, return the following test:

```text
INFO     | Opening Redis connection (redis://localhost/6379)
unlocking wallet ...
14:21:53 RQ worker 'rq:worker:YOURHOSTNAME.554' started, version 0.9.2
14:21:53 Cleaning registries for queue: default
14:21:53
14:21:53 *** Listening on default...
```

Nothing else needs to be done at this point.

### **Testing**

{% hint style="danger" %}
**Important**: For testing, we highly recommend that you set the `nobroadcast` flag in `config.yaml` to `True`
{% endhint %}

For testing, we need to throw a properly formatted incident at the endpoint. The following is an example of the file format,

{% hint style="warning" %}
**Note**: Because the incident data changes all the time and is quickly out of date, the actual contents of this file are unlikely to work. At the time of testing reach out to PBSA for some up to date incident data.
{% endhint %}

```text
{'provider_info': {'pushed': '2018-03-10T00:06:23Z', 'name': '5e2cdc120c9404f2609936aa3a8d49e4'}, 'call': 'create', 'timestamp': '2018-04-25T10:54:10.495868Z', 'arguments': {'unsure': True, 'season': '2018'}, 'unique_string': '2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true', 'id': {'away': 'New York Islanders', 'event_group_name': 'NHL Regular Season', 'start_time': '2018-03-16T23:00:00Z', 'home': 'Washington Capitals', 'sport': 'Ice Hockey'}}
{'provider_info': {'pushed': '2018-03-10T00:06:23Z', 'name': '5e2cdc1safasf4f2609936aa3a8d49e4'}, 'call': 'create', 'timestamp': '2018-04-25T10:54:10.495868Z', 'arguments': {'unsure': True, 'season': '2018'}, 'unique_string': '2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true', 'id': {'away': 'New York Islanders', 'event_group_name': 'NHL Regular Season', 'start_time': '2018-03-16T23:00:00Z', 'home': 'Washington Capitals', 'sport': 'Ice Hockey'}}
```

Store them in a file called `replay.txt` and run the following call:

```text
bos-auto replay --url http://localhost:8010/trigger replay.txt
```

{% hint style="warning" %}
Note the `trigger` at the end of the endpoint URL.
{% endhint %}

This will show you the incident and a load indicator at 100% once the incident has been successfully sent to the endpoint.

Your endpoint should return the following:

```text
INFO     | Forwarded incident create to worker via redis
127.0.0.1 - - [26/Apr/2018 14:25:43] "POST /trigger HTTP/1.1" 200 -
```

And your worker to return something along the lines of \(once for each incident above\):

```text
14:23:38 default: bookied.work.process({'provider_info': {'pushed': '2018-03-10T00:06:23Z', 'name': '5e2cdc120c9404f2609936aa3a8d49e4'}, 'call': 'create', 'timestamp': '2018-04-25T10:54:10.495868Z', 'arguments': {'unsure': True, 'season': '2018'}, 'unique_string': '2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true', 'id': {'away': 'New York Islanders', 'event_group_name': 'NHL Regular Season', 'start_time': '2018-03-16T23:00:00Z', 'home': 'Washington Capitals', 'sport': 'Ice Hockey'}, 'approver': 'init0', 'proposer': 'init0'}, approver=None, proposer=None) (a2f4eaaf-e750-4934-8c73-5481fe32df94)
  INFO     | processing create call with args {'unsure': True, 'season': '2018'}
  INFO     | Creating a new event ...
  INFO     | Creating event with teams ['Washington Capitals', 'New York Islanders'] in group NHL Regular Season.
  INFO     | Object "NHL Regular Season/Washington Capitals/New York Islanders" has pending update proposal. Approving {'pid': '1.10.413', 'oid': 0, 'proposal': <Proposal 1.10.413>}
  INFO     | Approval Map: {'1.10.224': '0.0', '1.10.413': '25.0', '1.10.414': '0.0', '1.10.416': '0.0'}
  INFO     | Object "New York Islanders @ Washington Capitals/Moneyline" has pending update proposal. Approving {'pid': '1.10.413', 'oid': 1, 'proposal': <Proposal 1.10.413>}
  INFO     | Approval Map: {'1.10.224': '0.0', '1.10.413': '50.0', '1.10.414': '0.0', '1.10.416': '0.0'}
  INFO     | Updating Betting Markets ...
  INFO     | Updating Betting Market New York Islanders ...
  INFO     | Object "Moneyline/New York Islanders" has pending update proposal. Approving {'pid': '1.10.413', 'oid': 2, 'proposal': <Proposal 1.10.413>}
  INFO     | Approval Map: {'1.10.224': '0.0', '1.10.413': '75.0', '1.10.414': '0.0', '1.10.416': '0.0'}
  INFO     | Updating Betting Market Washington Capitals ...
  INFO     | Object "Moneyline/Washington Capitals" has pending update proposal. Approving {'pid': '1.10.413', 'oid': 3, 'proposal': <Proposal 1.10.413>}
  INFO     | Approval Map: {'1.10.224': '0.0', '1.10.413': '100.0', '1.10.414': '0.0', '1.10.416': '0.0'}
  INFO     | Proposal 1.10.413 has already been approved by init0
  INFO     | Skipping dynamic BMG: New York Islanders @ Washington Capitals/Handicap
  INFO     | Skipping dynamic BMG: New York Islanders @ Washington Capitals/Over/Under {OU} pts
14:23:45 default: Job OK (a2f4eaaf-e750-4934-8c73-5481fe32df94)
14:23:45 Result is kept for 500 seconds
14:23:45
14:23:45 *** Listening on default...
14:23:45 default: bookied.work.approve(approver=None, proposer=None) (cb914014-3bc1-4db7-b684-723826ce3c09)
  INFO     | Testing for pending proposals created by init0 that we could approve by init0
14:23:45 default: Job OK (cb914014-3bc1-4db7-b684-723826ce3c09)
14:23:45 Result is kept for 500 seconds
14:23:45
```

{% hint style="info" %}
**Tip**: Each incident results in **two** work items, namely a `bookied.work.process()` as well as a `bookied.work.approve()` call. 

The former does the heavy lifting and may produce a proposal, while the latter approves proposals that we have created on our own.
{% endhint %}

### **Command Line Intervention**

With the command line tool, we can connect to the MongoDB and inspect the incidents that we inserted above:

```text
bos-auto incidents list [Begin Date] [End Date]
```

Where \[Begin Date\] and \[End Date\] specify the date range to pull incident data from.

The output should look like:

```text
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| identifier                   | Incidents                                                                                                                                                             |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Ice Hockey                   | +--------+------------+----------------------------------------------------------------------------------------------------------+----------------------------------+ |
| NHL Regular Season           | | call   | status     | incident uid                                                                                             | incident provider                | |
| 2018-03-16T23:00:00Z         | +--------+------------+----------------------------------------------------------------------------------------------------------+----------------------------------+ |
| home: Washington Capitals    | | create | name: done | 2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true | 5e2cdc117c9404f2609936aa3a8d49e4 | |
| away: New York Islanders     | |        |            | 2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true | 5e2cdc120c9404f2609936aa3a8d49e4 | |
|                              | +--------+------------+----------------------------------------------------------------------------------------------------------+----------------------------------+ |
+------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

It tells you that **two** incidents for that particular match came in that both proposed to **create** the incident. The status tells us that the incidents have been processed.

We can now read the actual incidents with:

```text
bos-auto incidents show 2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true 5e2cdc117c9404f2609936aa3a8d49e4
```

And replay any of the two incidents by using:

```text
bos-auto incidents resend 2018-03-16t230000z-ice-hockey-nhl-regular-season-washington-capitals-new-york-islanders-create-2018-true 5e2cdc117c9404f2609936aa3a8d49e4
```

{% hint style="info" %}
Tip: For more information on BOS supported commands run:

`bos-auto --help` or `bos-incidents --help`
{% endhint %}

Your worker should now be started.

