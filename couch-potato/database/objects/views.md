# Views

## vwgameevents

#### Columns

| Column | Alias |
| :--- | :--- |
| events.league | league |
| events.id | eventid |
| events.date | date |
| games.hometeam | hometeam |
| games.awayteam | awayteam |
| games.starttime | starttime |
| games.homescore | homescore |
| games.awayscore | awayscore |
| games.id | gameid |
| leagues.sport | sportid |
| leagues.id | leagueid |
| leagues.icon | leagueicon |
| sports.name | sportname |
| sports.icon | icon |
| sports.duration | duration |
| progress.status | status |
| status.name | name |

#### Script

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `vwgameevents` AS
    SELECT 
        `x`.`league` AS `league`,
        `x`.`id` AS `eventid`,
        `x`.`date` AS `date`,
        CONCAT(`x`.`date`, ' ', `y`.`starttime`) AS `datetime`,
        `y`.`hometeam` AS `hometeam`,
        `y`.`awayteam` AS `awayteam`,
        `y`.`starttime` AS `starttime`,
        `y`.`homescore` AS `homescore`,
        `y`.`awayscore` AS `awayscore`,
        `y`.`id` AS `gameid`,
        `z`.`sport` AS `sportid`,
        `z`.`id` AS `leagueid`,
        `z`.`icon` AS `leagueicon`,
        `a`.`name` AS `sportname`,
        `a`.`icon` AS `icon`,
        `a`.`duration` AS `duration`,
        `p`.`status` AS `status`,
        `s`.`name` AS `name`
    FROM
        (((((`events` `x`
        JOIN `games` `y` ON ((`y`.`event` = `x`.`id`)))
        JOIN `leagues` `z` ON ((`x`.`league` = `z`.`name`)))
        JOIN `sports` `a` ON ((`z`.`sport` = `a`.`id`)))
        JOIN `progress` `p` ON ((`y`.`id` = `p`.`game`)))
        JOIN `status` `s` ON ((`p`.`status` = `s`.`id`)))
    ORDER BY `x`.`date` , `y`.`starttime`
```

## vwsports

#### Columns

| Column | Alias |
| :--- | :--- |
| sports.id | id |
| sports.name | sportsname |
| sports.icon | sportsicon |
| leagues.name | leaguename |
| leagues.icon | leagueicon |

#### Script

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `vwsports` AS
    SELECT 
        `x`.`id` AS `id`,
        `x`.`name` AS `sportsname`,
        `x`.`icon` AS `sportsicon`,
        `y`.`name` AS `leaguename`,
        `y`.`icon` AS `leagueicon`
    FROM
        (`sports` `x`
        JOIN `leagues` `y` ON ((`x`.`id` = `y`.`sport`)))
```

## vwteams

#### Columns

| Column | Alias |
| :--- | :--- |
| teams.name | teamname |
| leagues.name | leaguename |

#### Script

```sql
CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`localhost` 
    SQL SECURITY DEFINER
VIEW `vwteams` AS
    SELECT 
        `x`.`name` AS `teamname`, 
        `y`.`name` AS `leaguename`
    FROM
        (`teams` `x`
        JOIN `leagues` `y` ON ((`x`.`league` = `y`.`id`)))
```

