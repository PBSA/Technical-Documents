# General

This page documents the BookiePro data abstraction layer with the Peerplays blockchain.&#x20;

BookiePro communicates with the blockchain using web-socket API calls.

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

Get a list of all event groups for a sport, for example, all soccer leagues in soccer.

```javascript
Apis.instance().db_api().exec( "list_event_groups", [sportId] )
```

{% tabs %}
{% tab title="Parameters" %}
`sportId`: The id of the sport that the event groups are to be listed for.
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

Get a list of all betting market groups for an event, for example, Moneyline and OVER/UNDER for soccer)

```javascript
Apis.instance().db_api().exec( "list_betting_market_groups", [eventId] )
```

{% tabs %}
{% tab title="Parameters" %}
`eventId`: The id of the event that the betting market groups are to be listed for.
{% endtab %}

{% tab title="Return" %}
A list of all the betting market groups for the event.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getBettingMarketGroupsList = function getBettingMarketGroupsList(eventId) {
    var _this18 = this;

    var bettingMarketGroupsList = this.betting_market_groups_list_by_sport_id.get(eventId);

    if (bettingMarketGroupsList === undefined) {
      this.betting_market_groups_list_by_sport_id = this.betting_market_groups_list_by_sport_id.set(eventId, _immutable2.default.Set());

      _ws.Apis.instance().db_api().exec('list_betting_market_groups', [eventId]).then(function (bettingMarketGroups) {
        var set = new Set();

        for (var i = 0, len = bettingMarketGroups.length; i < len; ++i) {
          set.add(bettingMarketGroups[i]);
        }

        _this18.betting_market_groups_list_by_sport_id = _this18.betting_market_groups_list_by_sport_id.set( // eslint-disable-line
        eventId, _immutable2.default.Set(set));
        _this18.notifySubscribers();
      }, function () {
        _this18.betting_market_groups_list_by_sport_id = _this18.betting_market_groups_list_by_sport_id.delete( // eslint-disable-line
        eventId);
      });
    }

    return this.betting_market_groups_list_by_sport_id.get(eventId);
  };
```
{% endtab %}
{% endtabs %}

### list\_betting\_markets

Get a list of all betting markets for a betting market group (BMG).

```javascript
Apis.instance().db_api().exec( "list_betting_markets", [bettingMarketGroupId] )
```

{% tabs %}
{% tab title="Parameters" %}
`bettingMarketGroupId: `The id of the betting market group that the betting markets are to be listed for.
{% endtab %}

{% tab title="Return" %}
A list of all the betting markets for the betting market group.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getBettingMarketsList = function getBettingMarketsList(bettingMarketGroupId) {
    var _this19 = this;

    var bettingMarketsList = this.betting_markets_list_by_sport_id.get(bettingMarketGroupId);

    if (bettingMarketsList === undefined) {
      this.betting_markets_list_by_sport_id = this.betting_markets_list_by_sport_id.set(bettingMarketGroupId, _immutable2.default.Set());

      _ws.Apis.instance().db_api().exec('list_betting_markets', [bettingMarketGroupId]).then(function (bettingMarkets) {
        var set = new Set();

        for (var i = 0, len = bettingMarkets.length; i < len; ++i) {
          set.add(bettingMarkets[i]);
        }

        _this19.betting_markets_list_by_sport_id = _this19.betting_markets_list_by_sport_id.set(bettingMarketGroupId, _immutable2.default.Set(set));
        _this19.notifySubscribers();
      }, function () {
        _this19.betting_markets_list_by_sport_id = _this19.betting_markets_list_by_sport_id.delete(bettingMarketGroupId);
      });
    }

    return this.betting_markets_list_by_sport_id.get(bettingMarketGroupId);
  };
```
{% endtab %}
{% endtabs %}

### get\_global\_betting\_statistics

Get global betting statistics

```javascript
Apis.instance().db_api().exec( "get_global_betting_statistics", [] )
```

{% tabs %}
{% tab title="Return" %}
A list of all the global betting statistics.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getGlobalBettingStatistics = function getGlobalBettingStatistics() {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().db_api().exec('get_global_betting_statistics', []).then(function (getGlobalBettingStatistics) {
        if (getGlobalBettingStatistics) {
          resolve(getGlobalBettingStatistics);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### get\_binned\_order\_book

Get the binned order book for a betting market.

```javascript
Apis.instance().bookie_api().exec( "get_binned_order_book", [ betting_market_id, precision ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `betting_market_id: `The id of the betting market for the order book.
* `precision: `Precision
{% endtab %}

{% tab title="Return" %}
A list of binned orders for the betting market.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getBinnedOrderBook = function getBinnedOrderBook(betting_market_id, precision) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().bookie_api().exec('get_binned_order_book', [betting_market_id, precision]).then(function (order_book_object) {
        if (order_book_object) {
          resolve(order_book_object);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### get\_total\_matched\_bet\_amount\_for\_betting\_market\_group

Get the total matched bets for a betting market group (BMG).

```javascript
Apis.instance().bookie_api().exec( "get_total_matched_bet_amount_for_betting_market_group", [ group_id ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `group_id`: The betting market group id.
{% endtab %}

{% tab title="Return" %}
Total of all the matched bet amounts for the selected betting market group.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getTotalMatchedBetAmountForBettingMarketGroup = function getTotalMatchedBetAmountForBettingMarketGroup(group_id) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().bookie_api().exec('get_total_matched_bet_amount_for_betting_market_group', [group_id]).then(function (total_matched_bet_amount) {
        if (total_matched_bet_amount) {
          resolve(total_matched_bet_amount);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### **get\_events\_containing\_sub\_string**

Used to search for events.

```javascript
Apis.instance().bookie_api().exec( "get_events_containing_sub_string", [ sub_string, language ])
```

{% tabs %}
{% tab title="Parameters" %}
* `sub_string:` The (sub) string of text to search for
* `language`: Language id.
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

### get\_unmatched\_bets\_for\_bettor

Get unmatched bets for a bettor.

```javascript
Apis.instance().bookie_api().exec( "get_matched_bets_for_bettor", [ bettor_id ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `bettor_id`: The id of the bettor.
{% endtab %}

{% tab title="Return" %}
List of all matched bets for a bettor.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getUnmatchedBetsForBettor = function getUnmatchedBetsForBettor(betting_market_id_type, account_id_type) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().db_api().exec('get_unmatched_bets_for_bettor', [betting_market_id_type, account_id_type]).then(function (unmatched_bets_for_bettor) {
        if (unmatched_bets_for_bettor) {
          resolve(unmatched_bets_for_bettor);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### list\_events\_in\_group

Get a list of events in any event group.

```javascript
Apis.instance().db_api().exec( "list_events_in_group", [ event_group_id ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `event_group_id`: The id of the event group.
{% endtab %}

{% tab title="Return" %}
A list of all the events in the event group.
{% endtab %}

{% tab title="Code" %}
```javascript
 ChainStore.listEventsInGroup = function listEventsInGroup(event_group_id) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().db_api().exec('list_events_in_group', [event_group_id]).then(function (events_in_group) {
        if (events_in_group) {
          resolve(events_in_group);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### get\_all\_unmatched\_bets\_for\_bettor

Get all unmatched bets of a bettor according to account type.

```javascript
Apis.instance().db_api().exec( "get_all_unmatched_bets_for_bettor", [ account_id_type ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `account_type_id`: The id of the bettor account type/
{% endtab %}

{% tab title="Return" %}
All unmatched bets by bettor account type.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getAllUnmatchedBetsForBettor = function getAllUnmatchedBetsForBettor(account_id_type) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().db_api().exec('get_all_unmatched_bets_for_bettor', [account_id_type]).then(function (all_unmatched_bets_for_bettor) {
        if (all_unmatched_bets_for_bettor) {
          resolve(all_unmatched_bets_for_bettor);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### get\_matched\_bets\_for\_bettor

Get the matched bets for a bettor.

```javascript
Apis.instance().bookie_api().exec( "get_matched_bets_for_bettor", [ bettor_id ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `bettor_id`: The id of the bettor.
{% endtab %}

{% tab title="Return" %}
All matched bets for the bettor.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getMatchedBetsForBettor = function getMatchedBetsForBettor(bettor_id) {
    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().bookie_api().exec('get_matched_bets_for_bettor', [bettor_id]).then(function (matched_bets_for_bettor) {
        if (matched_bets_for_bettor) {
          resolve(matched_bets_for_bettor);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}

### get\_all\_matched\_bets\_for\_bettor

Get all matched bets for a bettor within a range.

```javascript
Apis.instance().bookie_api().exec( "get_all_matched_bets_for_bettor", [ bettor_id, start, limit ] )
```

{% tabs %}
{% tab title="Parameters" %}
* `bettor_id`: The id of the bettor
* `start`: The start date
* `limit`: Number of bets to be returned
{% endtab %}

{% tab title="Return" %}
All matched bets for the better within the range `start` to `limit.`
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.getAllMatchedBetsForBettor = function getAllMatchedBetsForBettor(bettor_id, start) {
    var limit = arguments.length > 2 && arguments[2] !== undefined ? arguments[2] : 1000;

    return new Promise(function (resolve, reject) {
      _ws.Apis.instance().bookie_api().exec('get_all_matched_bets_for_bettor', [bettor_id, start, limit]).then(function (all_matched_bets_for_bettor) {
        if (all_matched_bets_for_bettor) {
          resolve(all_matched_bets_for_bettor);
        } else {
          resolve(null);
        }
      }, reject);
    });
  };
```
{% endtab %}
{% endtabs %}
