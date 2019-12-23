# Global Replays

Global replays are triggered by data proxy operators and as such only available to them. How they are done depends on the setup of the corresponding data provider.

### Enetpulse <a id="Globalreplays-Enetpulse"></a>

Enetpulse connects to the data proxy via a local PHP client that receives pushes in delta format, which are translated into SQL queries and syncs a local database. That database contains a series of additional tables, views and triggers designed for the interaction with the data proxy. Every time a incident is recognized by the triggers, a UDF \(user-defined function\) is called that makes HTTP push on localhost to the data proxy, this from the data proxy perspective it receives properly formatted incidents from the Enetpulse provider \(it is agnostic to its source\).

A global replay is triggered by manipulating the timestamp of the event table, for example for the three main Soccer leagues

| `UPDATE    eventSET    ut = '2019-02-14 17:00:00'` `# current timeWHERE    id in` `(        select            event_id        from            boss_event_details_view        where            sport = "Soccer"            and status = "notstarted"            and league in` `("Serie A", "Primera Division", "Premier League")            and game_scheduled between '2019-02-14'` `and '2019-04-01'    );` |
| :--- |


Event league names can be queried with the view `boss_event_details_view`. The downside of this query is that you need to enter the time of events to send out \(one could adjust this query to fetch the league start and end time\), and it does only replay the state the event is currently in. For the usage of global replays this is acceptable, since it should replay create incidents.

### Scorespro <a id="Globalreplays-Scorespro"></a>

Data from the Scorepro provider is polled every 5 seconds from their web API and processed by the data proxy. They also provide league summary APIs, which are used to do global replays. Can only be called from localhost.

Fetch all incidents from one league until a specified time

| `python3 cli.py pull-create scorespro --eventgroup <event_group_id> --until <YYYYMMDD>` |
| :--- |


To get the event group id use

| `python3 cli.py find scorespro` |
| :--- |


which prints all ids of configured leagues. The configuration contains a section `providers.scorespro.recognize` that holds the following keys

* `seasons`  List of years to be recognized, e.g. "2019"
* `countries List of countries to recognize, e.g. "USA"`
* `leagues Tree of league short codes (as defined by Scorepro) to recognize`

The defaults for those values are found here: [https://bitbucket.org/peerplaysblockchain/bos-dataproxy/src/c6583619de233e9e7b9ecc45a31db811f2ba5dba/dataproxy/provider/scorespro/initialize.py\#lines-39](https://bitbucket.org/peerplaysblockchain/bos-dataproxy/src/c6583619de233e9e7b9ecc45a31db811f2ba5dba/dataproxy/provider/scorespro/initialize.py#lines-39)

### LSports / StatScore <a id="Globalreplays-LSports/StatScore"></a>

Both providers enable us to connect to a RMQ feed which pushes data. Both providers have a web management console that needs to be used to subscribe to leagues \(billable events\), which are then being sent out. Both have additional HTTP calls available that allow the fetching of events. Can only be called from localhost.

Fetch all incidents from one league until a specified time

| `python3 cli.py pull-create lsports --eventgroup <event_group_id> --until <YYYYMMDD>python3 cli.py pull-create statscore --eventgroup <event_group_id> --until <YYYYMMDD>` |
| :--- |


To find event group ids you can use 

| `python3 cli.py find lsports --sport "American Football"` `--country "United States"python3 cli.py find statscore --sport "Am. football"` `--country "USA"` |
| :--- |


The find call for both providers is hierarchical. If you start with no argument, it will list you available sports. If you only have the sport argument, it will list you available countries. If you have sport and country, it will list available leagues.

