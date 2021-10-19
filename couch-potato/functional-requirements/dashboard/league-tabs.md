# League Tabs

The leagues tab runs vertically down the left side of the dashboard and displays one tab for each league that is configured for the selected sport. The tabs are dynamic and configured through the MySql database [`Leagues`](broken-reference) table.&#x20;

![](<../../../.gitbook/assets/image (1).png>)

The order the leagues tabs are displayed in is defined by their `id` value in the [`Leagues`](broken-reference) table.

There is no limit on the number of sports tabs that can be created. If the tabs reach the vertical limit of the application then they will stack in to multiple columns. Realistically there should never be so many leagues enabled at any one time to cause the tabs to be stacked.

{% hint style="danger" %}
**Important**: The leagues tabs must be 100% configurable through the database only. Leagues must be added or removed without any code changes.
{% endhint %}

Clicking on any unselected tab will change the calendar display to show only events for the selected sport and league.

### **Captions**

| Text           | Type    | Comments                                                                                                                                                                                                                                         |
| -------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| \[league name] | Dynamic | Value set in [`Leagues`](broken-reference) table.                                                                                                                                                                                                |
| \[icon]        | Dynamic | <p> Path and name defined in the <code>icon</code> column of the <a href="broken-reference"><code>Leagues</code></a> table.</p><p>The icon itself must exist in the corresponding <code>asset/imgs/leagues</code> folder in the application.</p> |

### **Actions**

| Caption   | Type | Action                                                        |
| --------- | ---- | ------------------------------------------------------------- |
| \[League] | Text | Change calendar to the selected league of the selected sport. |
