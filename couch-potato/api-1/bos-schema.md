# BOS Schema



These objects are used for the five BOS incident messages. The objects are created and populated internally by the Couch Potato API.

## create

{% tabs %}
{% tab title="Schema" %}
```javascript
{
    "call":,             // Always 'create'
    "unique_string":,    // unique identifier of type unique_string
    "timestamp":,        // The date/time the game was created (UTC)
    "arguments":         
    {
        "season":        // The season for the sports
    },
    "id":
    {
        "home":,            // The home team
        "away":,            // The away team
        "sport":,           // The sport
        "start_time":,      // The scheduled date/time for the game to start (UTC)
        "event_group_name": // The event group (league)
    },
    "provider_info":
    {
        "match_id":,        // Unique identifier for the match (game)
        "name":,            // Name of the provider
        "source":           // Always 'direct string input'
        "source_file":,     // Not used
        "pushed":           // The date/time the incident was sent (UTC)
    }
}
```
{% endtab %}

{% tab title="Example" %}
```java
{
    "call":"create",
    "unique_string":"2020-05-01t01:12:00.000z__soccer__epl__leicester-city__aston-villa__create__20192020",
    "timestamp":"2020-01-28T14:41:13.705Z",
    "arguments":
    {
        "season":"2019/2020"
    },
    "id":
    {
        "home":"Leicester City",
        "away":"Aston Villa",
        "sport":"Soccer",
        "start_time":"2020-05-01T01:12:00Z",
        "event_group_name":"EPL"
    },
    "provider_info":
    {
        "match_id":"25",
        "name":"couch-potato",
        "source":
        "direct string input",
        "source_file":"",
        "pushed":"2020-01-28T14:41:13.705Z"
    }
}
```
{% endtab %}
{% endtabs %}

## in\_progress

{% tabs %}
{% tab title="Schema" %}
```javascript
{
    "call":,             // Always 'in_progress'
    "unique_string":,    // unique identifier of type unique_string
    "timestamp":,        // The date/time the game was created (UTC)
    "arguments":         
    {
         "whistle_start_time": // The time the game actually started
    },
    "id":
    {
        "home":,            // The home team
        "away":,            // The away team
        "sport":,           // The sport
        "start_time":,      // The scheduled date/time for the game to start (UTC)
        "event_group_name": // The event group (league) 
    },
    "provider_info":
    {
        "match_id":,        // Unique identifier for the match (game)
        "name":,            // Name of the provider
        "source":           // Always 'direct string input'
        "source_file":,     // Not used
        "pushed":           // The date/time the incident was sent (UTC)
    }
}
```
{% endtab %}

{% tab title="Example" %}
```java
{
    "call":"in_progress",
    "unique_string":"2020-05-01t01:12:00.000z__soccer__epl__leicester-city__aston-villa__create__20192020",
    "timestamp":"2020-01-28T14:41:13.705Z",
    "arguments":
    {
        "whistle_start_time":"2020-05-01T01:15:00Z"
    },
    "id":
    {
        "home":"Leicester City",
        "away":"Aston Villa",
        "sport":"Soccer",
        "start_time":"2020-05-01T01:12:00Z",
        "event_group_name":"EPL"
    },
    "provider_info":
    {
        "match_id":"25",
        "name":"couch-potato",
        "source":
        "direct string input",
        "source_file":"",
        "pushed":"2020-01-28T14:41:13.705Z"
    }
}
```
{% endtab %}
{% endtabs %}

## result

{% tabs %}
{% tab title="Schema" %}
```javascript
{
    "call":,             // Always 'result'
    "unique_string":,    // unique identifier of type unique_string
    "timestamp":,        // The date/time the game was created (UTC)
    "arguments":         
    {
         "home_score":,     // The home team score/result
         "away_score":,     // The away team score/result
    },
    "id":
    {
        "home":,            // The home team
        "away":,            // The away team
        "sport":,           // The sport
        "start_time":,      // The scheduled date/time for the game to start (UTC)
        "event_group_name": // The event group (league) 
    },
    "provider_info":
    {
        "match_id":,        // Unique identifier for the match (game)
        "name":,            // Name of the provider
        "source":           // Always 'direct string input'
        "source_file":,     // Not used
        "pushed":           // The date/time the incident was sent (UTC)
    }
}
```
{% endtab %}

{% tab title="Example" %}
```java
{
    "call":"result",
    "unique_string":"2020-05-01t01:12:00.000z__soccer__epl__leicester-city__aston-villa__create__20192020",
    "timestamp":"2020-01-28T14:41:13.705Z",
    "arguments":
    {
         "home_score":4,    
         "away_score":2,  
    },
    "id":
    {
        "home":"Leicester City",
        "away":"Aston Villa",
        "sport":"Soccer",
        "start_time":"2020-05-01T01:12:00Z",
        "event_group_name":"EPL"
    },
    "provider_info":
    {
        "match_id":"25",
        "name":"couch-potato",
        "source":
        "direct string input",
        "source_file":"",
        "pushed":"2020-01-28T14:41:13.705Z"
    }
}
```
{% endtab %}
{% endtabs %}

## finish

{% tabs %}
{% tab title="Schema" %}
```javascript
{
    "call":,             // Always 'finish'
    "unique_string":,    // unique identifier of type unique_string
    "timestamp":,        // The date/time the game was created (UTC)
    "arguments":         
    {
         "whistle_end_time": // The time the game ended
    },
    "id":
    {
        "home":,            // The home team
        "away":,            // The away team
        "sport":,           // The sport
        "start_time":,      // The scheduled date/time for the game to start (UTC)
        "event_group_name": // The event group (league) 
    },
    "provider_info":
    {
        "match_id":,        // Unique identifier for the match (game)
        "name":,            // Name of the provider
        "source":           // Always 'direct string input'
        "source_file":,     // Not used
        "pushed":           // The date/time the incident was sent (UTC)
    }
}
```
{% endtab %}

{% tab title="Example" %}
```csharp
{
    "call":"finish",
    "unique_string":"2020-05-01t01:12:00.000z__soccer__epl__leicester-city__aston-villa__create__20192020",
    "timestamp":"2020-01-28T14:41:13.705Z",
    "arguments":
    {
         "whistle_start_time":"2020-05-01T01:16:00Z"
    },
    "id":
    {
        "home":"Leicester City",
        "away":"Aston Villa",
        "sport":"Soccer",
        "start_time":"2020-05-01T01:12:00Z",
        "event_group_name":"EPL"
    },
    "provider_info":
    {
        "match_id":"25",
        "name":"couch-potato",
        "source":
        "direct string input",
        "source_file":"",
        "pushed":"2020-01-28T14:41:13.705Z"
    }
}
```
{% endtab %}
{% endtabs %}

## canceled

{% tabs %}
{% tab title="Schema" %}
```javascript
 {
    "call":,             // Always 'canceled'
     "unique_string":,    // unique identifier of type unique_string
    "timestamp":,        // The date/time the game was created (UTC)
    "arguments":         
    {
    },
    "id":
    {
        "home":,            // The home team
        "away":,            // The away team
        "sport":,           // The sport
        "start_time":,      // The scheduled date/time for the game to start (UTC)
        "event_group_name": // The event group (league) 
    },
    "provider_info":
    {
        "match_id":,        // Unique identifier for the match (game)
        "name":,            // Name of the provider
        "source":           // Always 'direct string input'
        "source_file":,     // Not used
        "pushed":           // The date/time the incident was sent (UTC)
    }
}
```
{% endtab %}

{% tab title="Example" %}
```csharp
{
    "call":"canceled",
    "unique_string":"2020-05-01t01:12:00.000z__soccer__epl__leicester-city__aston-villa__create__20192020",
    "timestamp":"2020-01-28T14:41:13.705Z",
    "arguments":
    {
    },
    "id":
    {
        "home":"Leicester City",
        "away":"Aston Villa",
        "sport":"Soccer",
        "start_time":"2020-05-01T01:12:00Z",
        "event_group_name":"EPL"
    },
    "provider_info":
    {
        "match_id":"25",
        "name":"couch-potato",
        "source":
        "direct string input",
        "source_file":"",
        "pushed":"2020-01-28T14:41:13.705Z"
    }
}
```
{% endtab %}
{% endtabs %}

## unique\_string

This special attribute uniquely identifies each game to BOS. The format is as follows:

"`start_time`" + "\_\_" + "`sport`" + "\_\_" + "`event_group_name`" + "`home_team`" + "\_\_" + "`away_team`" + "create" + "\_\_" + "`season`"

**Example**

2020-05-01t01:12:00.000z\_\_soccer\_\_epl\_\_leicester-city\_\_aston-villa\_\_create\_\_20192020

{% hint style="danger" %}
**Important**: The string must be lowercase and any spaces in fields replaced with dashes \(-\).
{% endhint %}

