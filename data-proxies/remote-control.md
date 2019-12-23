# Remote Control

## Using BOS MINT <a id="RemoteControl-UsingBOSMINT"></a>

BOS MINT has remote control functionality built-in. It is only available when advanced features are enabled and `dataproxy_link`s are defined in the configuration, and the token matches \(defined through the data proxy operator\)  
  


| `advanced_features: True` `dataproxy_link:    ping_interval_in_seconds: 300`         `proxies:        d286***************************9bb:` `# enetpulse            name: BCP            endpoint: "`[`http://95.216.13.245:8010"`](http://95.216.13.245:8010"/)            `token: pbsabookie`         `612d***************************ca2:`  `# lsports            name: Omni            endpoint: "`[`http://dp0.omnidata.tech:8010"`](http://dp0.omnidata.tech:8010"/)            `token: pbsabookie`         `790***************************80b:`  `# scorespro            name: BLCKCHND            endpoint: "`[`http://174.138.14.239:8010"`](http://174.138.14.239:8010"/)            `token: pbsabookie` |
| :--- |


### Replay of events <a id="RemoteControl-Replayofevents"></a>

The replay will be explained with an example at hand: EPL - Fulham va Brighton

1. Find the event in the incidents overview ![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-1-28_14-50-15.png?version=1&modificationDate=1548683418126&cacheVersion=1&api=v2&width=590&height=250)
2. There are currently two replay buttons for each incident, please ignore the right one. Use the one that redirects to your MINT instance under the route     &lt;mint\_endpoint&gt;`/incidents/replay/<unique_string>`
3. The Replay incidents overview is visible with the selected incidents ![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-1-28_14-54-44.png?version=1&modificationDate=1548683687071&cacheVersion=1&api=v2&width=592&height=250) 
   1. Unique String is an identifier for the incident, which is unique within one provider. The same incidents of another provider has the same string
   2. Chain is not changeable and set to the one that MINT is connecting to
   3. Block producers allows to do a targeted replay if desired, or simply all
   4. Dataproxy: Selection needs to be done from which dataproxy the replay should be triggered
   5. Under the bottoms there is already a display what incidents have been found in the local incidents store
4. Select a dataproxy and click Check. The dataproxy is being queried if it has an incident for that particular game ![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-1-28_14-58-58.png?version=1&modificationDate=1548683941217&cacheVersion=1&api=v2&width=546&height=250) 
   1. The overview shows at the bottom what it has found at the dataproxy
5. Click Replay to initiate the replay ![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-1-28_15-1-15.png?version=1&modificationDate=1548684077691&cacheVersion=1&api=v2&width=504&height=250)
6. The incident is now being sent to the selected block producer, or all

Please note that this feature is admin only and lacks a more sophisticated UX

### Cancellation of events <a id="RemoteControl-Cancellationofevents"></a>

The advanced features also includes the cancellation of events. In order to get an overview, please open "Cancel legacy events" in the MINT menu. The output is verbose and will show you all events with a start date in the past \(that are likely cancellable\):  
![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-2-15_12-39-6.png?version=1&modificationDate=1550230747945&cacheVersion=1&api=v2&width=1076&height=250)  
  
Showing duplicates or otherwise stuck events here would be possible with further refinement. To get a dry run of the cancel trigger, please follow the instructions and add `cancel/<event_id>` to the browser address. The result should look like this:  
![](https://peerplays.atlassian.net/wiki/download/thumbnails/93913113/image2019-2-15_12-41-44.png?version=1&modificationDate=1550230905931&cacheVersion=1&api=v2&width=304&height=250)

On the bottom of that JSON you will find the calls that are being made to the data proxy  
![](https://peerplays.atlassian.net/wiki/download/attachments/93913113/image2019-2-15_12-42-33.png?version=1&modificationDate=1550230954602&cacheVersion=1&api=v2)

Investigate the shown cancel incidents, and once confirmed add `/send` to the browser address to actually trigger sending those incidents.

## Using dataproxy API directly <a id="RemoteControl-UsingdataproxyAPIdirectly"></a>

The dataproxies have an API that allows these replays that can also be used directly.

When replaying you need to investigate if there are enough incidents and which provider to choose for replay. All examples here are for BLCKCHND dataproxy, switch IPs as desired. Please find a listing of the hashes in [Live Data Proxies: Information](https://peerplays.atlassian.net/wiki/spaces/PROJECTS/pages/84377829/Live+Data+Proxies%3A+Information)

All the calls that are listed here include 

   &only\_report=True 

in the call, which is a safety against wrong clicking. If you want to execute it, click on the link, check that the result contains the desired incident, then remove 

   &only\_report=True

from the call and execute again. Done.

  
Description of possible arguments is available at

   [http://174.138.14.239:8010/replay?token=pbsabookie](http://174.138.14.239:8010/replay?token=pbsabookie)

The token is necessary as it grants access. Any call without the token will give a 404.

### **World Cup Replay** <a id="RemoteControl-WorldCupReplay"></a>

#### **2018-06-14T15:00:00Z: Russia v Saudi Arabia \(1.18.85\)** <a id="RemoteControl-2018-06-14T15:00:00Z:RussiavSaudiArabia(1.18.85)"></a>

[**Details**](http://95.216.13.245:8001/event/details/1.18.85) **/** [**Overview**](http://95.216.13.245:8001/overview/event/1.18.85) **/** [**Incidents**](http://95.216.13.245:8001/incidents/2018-06-14T15:00:00Z-Soccer-World%20Cup%202018:%20Group%20Stages-Russia)

**Create**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-14,Russia,Saudi,create&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-14,Russia,Saudi,create&only_report=True)

**In\_Progress**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-14,Russia,Saudi,in\_progress&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-14,Russia,Saudi,in_progress&only_report=True)

**Finish**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-14,Russia,Saudi,finish&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-14,Russia,Saudi,finish&only_report=True)

**Result**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-14,Russia,Saudi,result&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-14,Russia,Saudi,result&only_report=True)

#### **2018-06-15T12:00:00Z: Egypt v Uruguay \(1.18.80\)** <a id="RemoteControl-2018-06-15T12:00:00Z:EgyptvUruguay(1.18.80)"></a>

[**Details**](http://95.216.13.245:8001/event/details/1.18.80) **/** [**Overview**](http://95.216.13.245:8001/overview/event/1.18.80) **/** [**Incidents**](http://95.216.13.245:8001/incidents/2018-06-15T12:00:00Z-Soccer-World%20Cup%202018:%20Group%20Stages-Egypt)

**Create**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Egypt,Uruguay,create&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Egypt,Uruguay,create&only_report=True)

**In\_Progress**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Egypt,Uruguay,in\_progress&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Egypt,Uruguay,in_progress&only_report=True)

**Finish**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Egypt,Uruguay,finish&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Egypt,Uruguay,finish&only_report=True)

**Result**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Egypt,Uruguay,result&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Egypt,Uruguay,result&only_report=True)

#### **2018-06-15T15:00:00Z: Morocco v Iran \(1.18.97\)** <a id="RemoteControl-2018-06-15T15:00:00Z:MoroccovIran(1.18.97)"></a>

[**Details**](http://95.216.13.245:8001/event/details/1.18.97) **/** [**Overview**](http://95.216.13.245:8001/overview/event/1.18.97) **/** [**Incidents**](http://95.216.13.245:8001/incidents/2018-06-15T15:00:00Z-Soccer-World%20Cup%202018:%20Group%20Stages-Morocco)

**Create**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Morocco,Iran,create&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Morocco,Iran,create&only_report=True)

**In\_Progress**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Morocco,Iran,in\_progress&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Morocco,Iran,in_progress&only_report=True)

**Finish**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Morocco,Iran,finish&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Morocco,Iran,finish&only_report=True)

**Result**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Morocco,Iran,result&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Morocco,Iran,result&only_report=True)

#### **2018-06-15T18:00:00Z: Portugal v Spain \(1.18.100\)** <a id="RemoteControl-2018-06-15T18:00:00Z:PortugalvSpain(1.18.100)"></a>

[**Details**](http://95.216.13.245:8001/event/details/1.18.100) **/** [**Overview**](http://95.216.13.245:8001/overview/event/1.18.100) **/** [**Incidents**](http://95.216.13.245:8001/incidents/2018-06-15T18:00:00Z-Soccer-World%20Cup%202018:%20Group%20Stages-Portugal)

**Create**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Portugal,Spain,create&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Portugal,Spain,create&only_report=True)

**In\_Progress**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Portugal,Spain,in\_progress&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Portugal,Spain,in_progress&only_report=True)

**Finish**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Portugal,Spain,finish&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Portugal,Spain,finish&only_report=True)

**Result**

[http://174.138.14.239:8010/replay?token=pbsabookie&restrict\_witness\_group=baxter&name\_filter=2018-06-15,Portugal,Spain,result&only\_report=True](http://174.138.14.239:8010/replay?token=pbsabookie&restrict_witness_group=baxter&name_filter=2018-06-15,Portugal,Spain,result&only_report=True)

