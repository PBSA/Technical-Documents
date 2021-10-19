# API Reference

## add\_game

Adds a new game and sends a create incident message to BOS.

```http
POST /add_game/:create_message
```

{% tabs %}
{% tab title="Parameters" %}
* **`create_message`**: Object of type [create](broken-reference)
{% endtab %}

{% tab title="Response" %}
* Success&#x20;
  * ``[Add Game Success Response](broken-reference)
* Failure
  * `status`: 400: Bad Request
  * `subcode`:  One of **`Error Objects`**
  * `title`: One of` `**`Error Objects`**
  * `message`: One of **`Error Objects`**

**`Error Objects`**

* [BOS Errors](broken-reference)
* [Incident Errors](broken-reference)
* [Add Game Errors](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/json'});
var postData:any = {};
postData.sport = "Soccer";
postData.league = "EPL";
postData.user = 1;
postData.home = "Chelsea";
postData.away = "Manchester United";
postData.start_time = "2020-02-04T18:33:00Z";
http.post(url + "add_game.php?" , postData, {headers}).map();
```
{% endtab %}
{% endtabs %}

## start\_game

Starts an existing game and sends an in\_progress incident message to BOS.

```http
POST /start_game/:in_progress_message
```

{% tabs %}
{% tab title="Parameters" %}
* **`in_progress_message`**: Object of type [in\_progress](broken-reference)
{% endtab %}

{% tab title="Response" %}
* Success&#x20;
  * &#x20;[Start Game Success Response](broken-reference)
* Failure
  * `status`: 400: Bad Request
  * `subcode`:  One of **`Error Objects`**
  * `title`: One of` `**`Error Objects`**
  * `message`: One of **`Error Objects`**

**`Error Objects`**

* [BOS Errors](broken-reference)
* [General Errors](broken-reference)
* [Start Game Errors](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/json'});
var postData:any = {};
postData.sport = "Soccer";
postData.league = "EPL";
postData.user = 1;
postData.home = "Chelsea";
postData.away = "Manchester United";
postData.start_time = "2020-02-04T18:33:00.000Z";
postData.whistle_start_time = "2020-02-04T18:45:00.000Z";
postData.match_id: 24;
http.post(url + "start_game.php?" , postData, {headers}).map();
```
{% endtab %}
{% endtabs %}

## add\_score

Add scores to a game.

```http
POST /add_score/:result_message
```

{% tabs %}
{% tab title="Parameters" %}
* **`result_message`**: Object of type [result](broken-reference)
{% endtab %}

{% tab title="Response" %}
* Success&#x20;
  * &#x20;[Add Score Success Response](broken-reference)
* Failure
  * `status`: 400: Bad Request
  * `subcode`:  One of **`Error Objects`**
  * `title`: One of` `**`Error Objects`**
  * `message`: One of **`Error Objects`**

**`Error Objects`**

* [BOS Errors](broken-reference)
* [General Errors](broken-reference)
* [Add Score Errors](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/json'});
var postData:any = {};
postData.sport = "Soccer";
postData.league = "EPL";
postData.user = 1;
postData.home = "Chelsea";
postData.away = "Manchester United";
postData.start_time = "2020-02-04T18:33:00.000Z";
postData.home_score = 4;
postData.away_score = 2;
postData.match_id: 24;
http.post(url + "add_score.php?" , postData, {headers}).map();
```
{% endtab %}
{% endtabs %}

## finish\_game

Finish a game

```http
POST /finish_game/:finish_game_message
```

{% tabs %}
{% tab title="Parameters" %}
* **`finish_game_message`**: Object of type [finish](broken-reference)
{% endtab %}

{% tab title="Response" %}
* Success&#x20;
  * &#x20;[Finish game success response](broken-reference)
* Failure
  * `status`: 400: Bad Request
  * `subcode`:  One of **`Error Objects`**
  * `title`: One of` `**`Error Objects`**
  * `message`: One of **`Error Objects`**

**`Error Objects`**

* [BOS Errors](broken-reference)
* [General Errors](broken-reference)
* [Finish Game Errors](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/json'});
var postData:any = {};
postData.sport = "Soccer";
postData.league = "EPL";
postData.user = 1;
postData.home = "Chelsea";
postData.away = "Manchester United";
postData.start_time = "2020-02-04T18:33:00.000Z";
postData.whistle_end_time = "2020-02-04T20:33:00.000Z";
postData.match_id: 24;
http.post(url + "finish_game.php?" , postData, {headers}).map();
```
{% endtab %}
{% endtabs %}

## cancel\_game

Cancel a game

```http
POST /add_score/:cancel_game_message
```

{% tabs %}
{% tab title="Parameters" %}
* **`cancel_game_message`**: Object of type [canceled](broken-reference)
{% endtab %}

{% tab title="Response" %}
* Success&#x20;
  * [Cancel game success response](broken-reference)
* Failure
  * `status`: 400: Bad Request
  * `subcode`:  One of **`Error Objects`**
  * `title`: One of` `**`Error Objects`**
  * `message`: One of **`Error Objects`**

**`Error Objects`**

* [BOS Errors](broken-reference)
* [General Errors](broken-reference)
* [Cancel Game Errors](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/json'});
var postData:any = {};
postData.sport = "Soccer";
postData.league = "EPL";
postData.user = 1;
postData.home = "Chelsea";
postData.away = "Manchester United";
postData.start_time = "2020-02-04T18:33:00.000Z";
postData.match_id: 24;
http.post(url + "cancel_game.php" , postData, {headers}).map();
```
{% endtab %}
{% endtabs %}

## delete\_event

Delete an event according to the league and date.

```http
DELETE /delete_event/:date/:league
```

{% tabs %}
{% tab title="Parameters" %}
* **`date`**: The date of the event. Format is YYYY-MM-DD (UTC)
* **`league`**: The name of the league
{% endtab %}

{% tab title="Response" %}
* Success  - 200
  * `title: `League Deleted
  * `message:`\[league]
* Failure - 400
  * [Error 430](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/x-www-form-urlencoded'});
let httpParams = new HttpParams().set('date', '2020-02-29', 'league', 'EPL');
return http.delete(url + "delete_event.php", { 
        params: httpParams, headers: headers});
```
{% endtab %}
{% endtabs %}

## delete\_game

Delete an event according to the league and date.

```http
DELETE /delete_game/:game_id
```

{% tabs %}
{% tab title="Parameters" %}
* **`game_id`**: The id of the game to be deleted.
{% endtab %}

{% tab title="Response" %}
* Success  - 200
  * `title: `League Deleted
  * `message:`\[league]
* Failure - 400
  * [Error 431](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
var http: HttpClient;
var headers = new HttpHeaders({'Content-Type' : 'application/x-www-form-urlencoded'});
let httpParams = new HttpParams().set('game_id', 24);
return this.http.delete(this.url + "delete_game.php", { 
        params: httpParams, headers: headers});
```
{% endtab %}
{% endtabs %}

## get\_all\_data\_by\_date\_range

Get all games data between a date range.

```http
GET /get_all_data_by_date_range/:start_date/:end_date
```

{% tabs %}
{% tab title="Parameters" %}
* **`start_date`**: The start of the date range. Format is YYYY-MM-DDTHH:MM:SS.000Z
* **`end_date`**: The end of the date range. Format is YYYY-MM-DDTHH:MM:SS.000Z
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * List of all games between `start_date` and `end_date`
* Failure - 400
  * ``[Error 432](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_all_data_by_date_range.php", {
        params:{startdate: "2020-02-19T12:00:00.000Z", 
                enddate: "2020-02-29T12:00:00.000Z"}}).map();
```
{% endtab %}
{% endtabs %}

## get\_all\_games

Get all games.

```http
GET /get_all_games/
```

{% tabs %}
{% tab title="Response" %}
* Success - 200
  * List of all games
* Failure - 400
  * [Error 433](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_all_games.php").map()
```
{% endtab %}
{% endtabs %}

## get\_all\_sports

Get all sports.

```http
GET /get_all_sports/
```

{% tabs %}
{% tab title="Response" %}
* Success - 200&#x20;
  * List of all sports
* Failure - 400
  * [Error 434](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_all_sports.php").map()
```
{% endtab %}
{% endtabs %}

## get\_games\_by\_league\_and\_date

Get all games data between a date range and for a league.

```http
GET /get_games_by_league_and_date/:league/:start_date/:end_date
```

{% tabs %}
{% tab title="Parameters" %}
* **`league: `**`The sport league (event group).`
* **`start`**: The start of the date range. Format is YYYY-MM-DDTHH:MM:SS.000Z.
* **`end`**: The end of the date range. Format is YYYY-MM-DDTHH:MM:SS.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * List of all games for `league` between `start` and `end`
* Failure - 400
  * [Error 435](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_games_by_league_and_date.php", {
        params:{league: "NFL",
                startdate: "2020-02-19T12:00:00.000Z", 
                enddate: "2020-02-29T12:00:00.000Z"}}).map();
```
{% endtab %}
{% endtabs %}

## get\_games\_by\_league

Get all games for a league.

```http
GET /get_games_by_league/:league
```

{% tabs %}
{% tab title="Parameters" %}
* **`league: `**`The league (event group).`
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * List of all games for `league`
* Failure - 400
  * [Error 436](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_games_by_league.php", {
        params:{league: "NFL" }}).map();
```
{% endtab %}
{% endtabs %}

## get\_leagues\_by\_sport

Get all leagues from a league.

```http
GET /get_leagues_by_sport/:sport
```

{% tabs %}
{% tab title="Parameters" %}
* **`sport: `**The id of the sport.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * List of all leagues for `sport`
* Failure - 400
  * [Error 437](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_leagues_by_sport.php", {
        params:{sport: 0}}).map();
```
{% endtab %}
{% endtabs %}

## get\_sports\_and\_leagues

Get all sports and leagues

```http
GET /get_sports_and_leagues/
```

{% tabs %}
{% tab title="Response" %}
* Success - 200
  * List of all sports
* Failure
  * [Error 438](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_sports_and_leagues.php").map()
```
{% endtab %}
{% endtabs %}

## get\_teams\_by\_league

Get all teams from a league.

```http
GET /get_teams_by_league/:league
```

{% tabs %}
{% tab title="Parameters" %}
* **`league: `**The id of the league.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * List of all teams for the `league`
* Failure - 400
  * [Error 439](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "get_teams_by_league.php", {
        params:{league: 1}}).map();
```
{% endtab %}
{% endtabs %}

## get\_league\_data\_by\_name

Get all league information from its name.

```http
GET /get_league_data_by_name/:leaguename
```

{% tabs %}
{% tab title="Parameters" %}
* **`leaguename: `**The name of the league.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * All fields for the selected league
* Failure - 400
  * [Error 446](broken-reference)
{% endtab %}
{% endtabs %}

## last\_event\_id\_by\_date\_and\_league

Get the event id of the last event on a date and for the league.

```http
GET /last_event_id_by_date_and_league/:date/:league
```

{% tabs %}
{% tab title="Parameters" %}
* **`date: `**Event date in the format YYYY-MM\_DD
* **`league: `**The name of the league.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * The last event id for `league` on `date`
* Failure - 400
  * [Error 440](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "last_event_id_by_date_and_league.php", {
        params:{
                date: "2020-02-29",
                league: 1}}).map();
```
{% endtab %}
{% endtabs %}

## last\_event\_id

Get the id of the last event.

```http
GET /last_event_id/
```

{% tabs %}
{% tab title="Parameters" %}
* Success - 200
  * The last `event_id` for all leagues
* Failure - 400
  * [Error 441](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "last_event_id.php").map()
```
{% endtab %}
{% endtabs %}

## last\_game\_id\_by\_date\_and\_league

Get the game id of the last game on a date and for the league.

```http
GET /last_game_id_by_date_and_league/:date/:league
```

{% tabs %}
{% tab title="Parameters" %}
* **`date: `**Game date in the format YYYY-MM\_DD
* **`league: `**The name of the league.
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * The last game id for `league` on `date`
* Failure - 400
  * [Error 442](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "last_game_id_by_date_and_league.php", {
        params:{
                date: "2020-02-29",
                league: 1}}).map();
```
{% endtab %}
{% endtabs %}

## last\_game\_id

Get the game id of the last game for all sports.

```http
GET /last_game_id
```

{% tabs %}
{% tab title="Response" %}
* Success - 200
  * The last game id for all sports.
* Failure - 400
  * [Error 443](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "last_game_id.php").map();
```
{% endtab %}
{% endtabs %}

## last\_game

Get the game details of all games sorted descending so the most recent (last) game is the first record

```http
GET /last_game
```

{% tabs %}
{% tab title="Response" %}
* Success - 200
  * All game records sorted descending.
* Failure - 400
  * [Error 444](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "last_game").map();
```
{% endtab %}
{% endtabs %}

## run\_replay

Run a data replay for the selected sport and league(s)

```http
GET /run_replay/:sport/:leagues/:start/:end
```

{% tabs %}
{% tab title="Parameters" %}
* **`sport: `**The name of the sport
* **`leagues: `**The name of the leagues. Pipe separated list, e.g "EPL|La Liga|Serie A
* **`start`**: Start date in the format YYYY-MM\_DD
* **`end`**: End date in the format YYYY-MM\_DD
{% endtab %}

{% tab title="Response" %}
* Success - 200
  * `title`: Replay completed
  * `message`: \[sport]: { \[league]: total, \[league]: total, ... }
* Failure - 400
  * [Error 445](broken-reference)
{% endtab %}

{% tab title="Example" %}
```typescript
http.get(url + "run_replay.php", {params:{
                sport: "Soccer", 
                leagues: "EPL | La Liga", 
                start: "2020-02-01", 
                end: "2020-01-08"}}).map();
```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
**Note**: Replays can only be run for one sport at a time.
{% endhint %}
