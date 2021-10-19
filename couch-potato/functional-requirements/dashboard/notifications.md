# Notifications

The notifications panel is displayed on the right side of the dashboard and is where all notifications (reminders) will be posted for all games about to start or finish.

![](<../../../.gitbook/assets/image (5) (1).png>)

The notifications will be refreshed at a configurable millisecond interval set in the `notifications->delay` property in `config-dataproxy.json`. The default will be 3,000 (3 seconds).

Each notification will take the form of a 'note' which will have the following information:

### Colours

The colour of the notes is very important and must be visibly obvious. The colour of the note is set by the following criteria:

**Green**

* Any game that is in the range 30 - 15 minutes to it's scheduled start time.
* Any game that is in the range 30 - 15 minutes from it's predicted end time.

![](<../../../.gitbook/assets/image (23).png>)

**Amber**

* Any game that is in the range 1 - 14 minutes to it's scheduled start time.
* Any game that is in the range 1 - 14 minutes from it's predicted end time.

![](<../../../.gitbook/assets/image (11).png>)

**Red**

* Any game that should have started according to its scheduled start time.
* Any game that is in the range 30 - 15 minutes to it's predicted end time.

![](<../../../.gitbook/assets/image (13).png>)

### Start and End Times

The notifications rely on the start and end times of each game. The start time is taken as the time entered for any game when it was created, this is the only value that can be used.

The end time is more complicated because for many sports it's very hard to predict when a game ends because of time-outs, extra-time etc. For example, a game of soccer is much more predictable because the clock doesn't stop during play. So a game is likely to be two halves of 45 minutes, 15 minutes of half-time and perhaps 5 minutes of extra time, so 45+45+15+5 = 110 minutes.

However, a game of football, even thought it's four quarters of 15 minutes, has time-outs and regular clock stops, so the time the game will finish is a very broad average.

The duration of any sport is set in the `duration` column of the [`sport`](broken-reference) table. The default values are based on the accepted average durations for these sports.

{% hint style="warning" %}
**Note**: Because the end times are largely unpredictable the notification for game finishes should say "might have finished" rather than "should have finished"
{% endhint %}

### Note Attributes

| Text/Image                  | Type    | Comments                                                                                                                                                                                                                                                                               |
| --------------------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \[icon]                     | Dynamic |  The icon associated with the league for the game.                                                                                                                                                                                                                                     |
| \[start date/time]          | Dynamic | The start date and time of the game                                                                                                                                                                                                                                                    |
| \[sport] (\[league])        | Dynamic | The sport and league of the game                                                                                                                                                                                                                                                       |
| \[home team] v \[away team] | Dynamic | The home and away teams                                                                                                                                                                                                                                                                |
| \[Status]                   | Dynamic | <p>The time in minutes until start or end and the following text according to the rules above:</p><p></p><p><code>STARTS IN [x] MINUTES</code></p><p><code>SHOULD HAVE STARTED</code></p><p><code>MIGHT FINISH FINISH IN [x] MINUTES</code></p><p><code>MIGHT HAVE FINISHED</code></p> |

### Removing Notes

Each note is tied to a game and as such will be automatically removed from the notification panel as soon as the status of the game is updated. For example, if a note states that game x "SHOULD HAVE STARTED", then as soon as the game is started from the game selector, the note will be removed.

To keep things tidy notes can be set to be be automatically removed after a set interval (in hours). This means that if games haven't been started, instead of the warning note appearing even after \[x] days when there would be no point updating the status of the game, it will be removed.

The number of hours after which a notes should be removed is set in the `notifications->end` property in `config-dataproxy.json. `The default value is 240 (10 days).

### Hyperlinking

Each note is 'clickable', and when any note is clicked on it will automatically open the game selector for the selected date / league / sport combination. The game in the selector, that corresponds to the game on the selected note, will be highlighted for easy identification. For more information see:

{% content-ref url="../game-selector.md" %}
[game-selector.md](../game-selector.md)
{% endcontent-ref %}
