# Dashboard

The dashboard is the main screen from where you can navigate around the features of the application in preparation for inputting data.

The dashboard sis opened as soon as you log in from the  [Home Page](../../functional-requirements/home-page.md).

![](../../../.gitbook/assets/image%20%2814%29.png)

The dashboard is best thought of as five feature sections:

1. Header
2. Sport Tabs
3. League \(event Group\) Tabs
4. Calendar
5. Notifications

## Header

The dashboard header is shown at the top of the screen and is non-scrollable. That is to say that if you run  the application on a small display such that you have to scroll up and down, to see all of the dashboard, the header is always 'pinned' to the top of the screen.

![](../../../.gitbook/assets/image%20%289%29.png)

The features of the header are:

* Application version number.
* Real time clock.
* Data Replay button. See Replays.
* User name.
* User icon for opening the account menu. See Account Menu.

## Sports Tabs

The sports tab runs horizontally across the dashboard and displays one tab for each sport that is enabled. The tabs are dynamic and configured through a database table that can have new sports added or deleted at any time

There is no limit on the number of sports tabs that can be created. If the tabs reach the horizontal limit of the application then they will stack in to multiple rows. 

By clicking on any tab you will:

1. Update the Leagues Tabs to show only the leagues associated with the selected sport.
2. Change the calendar display to show only events for the selected sport and league.

When you select a sports tab the league will default to the first one in the list.

## League Tabs

The leagues tab runs vertically down the left side of the dashboard and displays one tab for each league that is configured for the selected sport. The tabs are dynamic and configured through a database table so that new leagues can be added or deleted at any time.

![](../../../.gitbook/assets/image%20%281%29.png)

There is no limit on the number of sports tabs that can be created. If the tabs reach the vertical limit of the application then they will stack in to multiple columns. 

If you click on any of the tabs the calendar will show only events for the selected sport and league.

## Calendar









## Notifications

