# Bookie API

This page documents the BookiePro data abstraction layer with the Peerplays blockchain. 

BookiePro communicates with the blockchain using web-socket API calls.

### **get\_events\_containing\_sub\_string**

Used to search for events.

```javascript
Apis.instance().bookie_api().exec( "get_events_containing_sub_string", [ sub_string, language ])
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
function getEventsContainingSubString(sub_string, language) {
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

### list\_sports

Get a list of available sports.

```javascript
Apis.instance().db_api().exec( "list_sports", [] )
```

{% tabs %}
{% tab title="Return" %}
A list of all the available sports.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getSportsList = function getSportsList() {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().db_api().exec('list_sports', []).then(function (sportsList) {
        if (sportsList) {
          resolve(sportsList);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### list\_event\_groups

Get a list of all event groups for a sport \(e.g. all soccer leagues in soccer\).

```javascript
Apis.instance().db_api().exec( "list_event_groups", [sportId] )
```

{% tabs %}
{% tab title="Parameters" %}
`sportId`: The id of the sport that the event groups are to be listed.
{% endtab %}

{% tab title="Return" %}
A list of all event groups for the selected sport.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getEventGroupsList = function getEventGroupsList(sportId) {
    var _this17 = this;

    var eventGroupsList = this.event_groups_list_by_sport_id.get(sportId);

    if (eventGroupsList === undefined) {
      this.event_groups_list_by_sport_id = this.event_groups_list_by_sport_id.set(sportId, _immutable2.default.Set());

      _ws.Apis.instance().db_api().exec('list_event_groups', [sportId]).then(function (eventGroups) {
        var set = new Set();

        for (var i = 0, len = eventGroups.length; i < len; ++i) {
          set.add(eventGroups[i]);
        }

        _this17.event_groups_list_by_sport_id = _this17.event_groups_list_by_sport_id.set(sportId, _immutable2.default.Set(set));
        _this17.notifySubscribers();
      }, function () {
        _this17.event_groups_list_by_sport_id = _this17.event_groups_list_by_sport_id.delete(sportId);
      });
    }

    return this.event_groups_list_by_sport_id.get(sportId);
  };
```
{% endtab %}
{% endtabs %}

### list\_betting\_market\_groups

Get a list of all betting market groups for an event

