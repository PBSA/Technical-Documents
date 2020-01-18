# Bookie API

This page documents the BookiePro data abstraction layer with the Peerplays blockchain. 

BookiePro communicates with the blockchain using web-socket API calls.

### **get\_events\_containing\_sub\_string**

Used to search for events.

```javascript
Apis.instance().bookie_api().exec( 
"get_events_containing_sub_string", [ sub_string, language ])
```

{% tabs %}
{% tab title="Parameters" %}
`sub_string:` The \(sub\) string of text to search for

`language`: Language id.
{% endtab %}

{% tab title="Return" %}
List of events that contain the `sub-string`
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getEventsContainingSubString = function getEventsContainingSubString(sub_string, language) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().bookie_api().exec('get_events_containing_sub_string', [sub_string, language]).then(
        function (events_containing_sub_string) {
        if (events_containing_sub_string) {
          resolve(events_containing_sub_string);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}









| Purpose | Code File | Example |  |
| :--- | :--- | :--- | :--- |
| \*\*\*\* | Used to search for events. | ChainStore.js | Apis.instance\(\).bookie\_api\(\).exec\( "get\_events\_containing\_sub\_string", \[ sub\_string, language \] \) |
| **get\_binned\_order\_book** |  | ChainStore.js | Apis.instance\(\).bookie\_api\(\).exec\( "get\_binned\_order\_book", \[ betting\_market\_id, precision \] \) |
| **get\_global\_betting\_statistics** |  | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "get\_global\_betting\_statistics", \[\] \) |
| **get\_total\_matched\_bet\_amount\_for\_betting\_market\_group** | Get the total matched bets for the BMG. | ChainStore.js | Apis.instance\(\).bookie\_api\(\).exec\( "get\_total\_matched\_bet\_amount\_for\_betting\_market\_group", \[ group\_id \] \) |
| **get\_matched\_bets\_for\_bettor** | Get the matched bets for a bettor. | ChainStore.js | Apis.instance\(\).bookie\_api\(\).exec\( "get\_matched\_bets\_for\_bettor", \[ bettor\_id \] \) |
| **get\_all\_matched\_bets\_for\_bettor** |  | ChainStore.js | Apis.instance\(\).bookie\_api\(\).exec\( "get\_all\_matched\_bets\_for\_bettor", \[ bettor\_id, start, limit \] \) |
| **get\_unmatched\_bets\_for\_bettor** | Get unmatched bets for a bettor. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "get\_unmatched\_bets\_for\_bettor", \[ betting\_market\_id\_type, account\_id\_type \] \) |
| **get\_all\_unmatched\_bets\_for\_bettor** |  | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "get\_all\_unmatched\_bets\_for\_bettor", \[ account\_id\_type \] \) |
| **get\_fill\_order\_history** |  | test/Api.js | Apis.instance\(\).history\_api\(\).exec\( "get\_fill\_order\_history", \["1.3.121", "1.3.0", 10\]\) |
|  |  |  |  |
| **list\_sports** | Get a list of available sports. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "list\_sports", \[\] \) |
| **list\_event\_groups** | Get a list of event groups. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "list\_event\_groups", \[sportId\] \) |
| **list\_events\_in\_group** | Get a list of events. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "list\_events\_in\_group", \[ event\_group\_id \] \) |
| **list\_betting\_market\_groups** | Get a list of betting market groups. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "list\_betting\_market\_groups", \[eventId\] \) |
| **list\_betting\_markets** | Get a list of betting markets. | ChainStore.js | Apis.instance\(\).db\_api\(\).exec\( "list\_betting\_markets", \[bettingMarketGroupId\] \) |
|  |  |  |  |
| **subscribe\_to\_market** | Subscribe a listener to a betting market. | sub\_unsub.js | Apis.instance\(\).db\_api\(\).exec\( "subscribe\_to\_market", \[updateListener, "1.3.0", "1.3.19"\]\) |
| **unsubscribe\_from\_market** | Unsubscribe a listener from a betting market. | sub\_unsub.js | Apis.instance\(\).db\_api\(\).exec\( "unsubscribe\_from\_market", \[updateListener, "1.3.0", "1.3.19"\]\) |

