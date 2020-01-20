# Tournaments

### get\_tournaments\_in\_state

Get a list of tournament ids for upcoming tournaments

```javascript
Apis.instance().db_api().exec('get_tournaments_in_state', [stateString, accountId])
```

{% tabs %}
{% tab title="Parameters" %}
* `stateString`: The tournament state
* `accountId`:Account Id
{% endtab %}

{% tab title="Return" %}
All tournaments for the selected state.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getTournamentIdsInState = function getTournamentIdsInState(accountId, stateString) {
    var _this7 = this;

    var tournamentIdsForThisAccountAndState = void 0;
    var tournamentIdsForThisAccount = this.tournament_ids_by_state.get(accountId);

    if (tournamentIdsForThisAccount === undefined) {
      tournamentIdsForThisAccountAndState = new _immutable2.default.Set();
      tournamentIdsForThisAccount = new _immutable2.default.Map().set(stateString, tournamentIdsForThisAccountAndState);
      this.tournament_ids_by_state = this.tournament_ids_by_state.set(accountId, tournamentIdsForThisAccount);
    } else {
      tournamentIdsForThisAccountAndState = tournamentIdsForThisAccount.get(stateString);

      if (tournamentIdsForThisAccountAndState !== undefined) {
        return tournamentIdsForThisAccountAndState;
      }

      tournamentIdsForThisAccountAndState = new _immutable2.default.Set();
      tournamentIdsForThisAccount = tournamentIdsForThisAccount.set(stateString, tournamentIdsForThisAccountAndState);
      this.tournament_ids_by_state = this.tournament_ids_by_state.set(accountId, tournamentIdsForThisAccount);
    }

    _ws.Apis.instance().db_api().exec('get_tournaments_in_state', [stateString, 100]).then(function (tournaments) {
      var originalTournamentIdsInState = _this7.tournament_ids_by_state.getIn([accountId, stateString]);
      // call updateObject on each tournament, which will classify it
      tournaments.forEach(function (tournament) {
        /**
         * Fix bug: we cant update tournament_ids_by_state if objects_by_id has a tournament
         */
        if (!originalTournamentIdsInState.get(tournament.id)) {
          _this7.clearObjectCache(tournament.id);
        }

        _this7._updateObject(tournament);
      });

      var tournament_id = _this7.tournament_ids_by_state.getIn([accountId, stateString]);

      if (tournament_id !== originalTournamentIdsInState) {
        _this7.notifySubscribers();
      }
    });
    return tournamentIdsForThisAccountAndState;
  };
```
{% endtab %}
{% endtabs %}

### get\_registered\_tournaments

Get a list of registered tournaments by account id.

```javascript
Apis.instance().db_api().exec('get_registered_tournaments', [accountId, 100])
```

{% tabs %}
{% tab title="Parameters" %}
* `accountId`: Account Id.
{% endtab %}

{% tab title="Return" %}
All registered tournaments for an account.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getRegisteredTournamentIds = function getRegisteredTournamentIds(accountId) {
    var _this8 = this;

    var tournamentIds = this.registered_tournament_ids_by_player.get(accountId);

    if (tournamentIds !== undefined) {
      return tournamentIds;
    }

    tournamentIds = new _immutable2.default.Set();
    this.registered_tournament_ids_by_player = this.registered_tournament_ids_by_player.set(accountId, tournamentIds);

    _ws.Apis.instance().db_api().exec('get_registered_tournaments', [accountId, 100]).then(function (registered_tournaments) {
      var originalTournamentIds = _this8.registered_tournament_ids_by_player.get(accountId);
      var newTournamentIds = new _immutable2.default.Set(registered_tournaments);

      if (!originalTournamentIds.equals(newTournamentIds)) {
        _this8.registered_tournament_ids_by_player = _this8.registered_tournament_ids_by_player.set(accountId, newTournamentIds);
        _this8.notifySubscribers();
      }
    });

    return tournamentIds;
  };

```
{% endtab %}
{% endtabs %}

### get\_tournaments

Get all tournaments between `last_tournament_id` and `start_tournament_id`.

```javascript
 _ws.Apis.instance().db_api().exec('get_tournaments', [last_tournament_id, limit, start_tournament_id])
```

{% tabs %}
{% tab title="Parameters" %}
* `last_tournament_id`: The last tournament id
* `limit`: The limit of tournaments to return.
* `start_tournament_id`: The starting tournament id.
{% endtab %}

{% tab title="Return" %}
A list of all tournaments between `last_tournament_id` and `start_tournament_id`.
{% endtab %}

{% tab title="Code" %}
```javascript
ChainStore.prototype.getTournaments = function getTournaments(last_tournament_id) {
    var _this20 = this;

    var limit = arguments.length > 1 && arguments[1] !== undefined ? arguments[1] : 5;
    var start_tournament_id = arguments[2];

    return _ws.Apis.instance().db_api().exec('get_tournaments', [last_tournament_id, limit, start_tournament_id]).then(function (tournaments) {
      var list = _immutable2.default.List();

      _this20.setLastTournamentId(null);

      if (tournaments && tournaments.length) {
        list = list.withMutations(function (l) {
          tournaments.forEach(function (tournament) {
            if (!_this20.objects_by_id.has(tournament.id)) {
              _this20._updateObject(tournament);
            }

            l.unshift(_this20.objects_by_id.get(tournament.id));
          });
        });
      }

      return list;
    });
  };
```
{% endtab %}
{% endtabs %}



