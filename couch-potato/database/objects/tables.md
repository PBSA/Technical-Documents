---
description: 'The following is a list of the tables in the Couch Potato database:'
---

# Tables

**Legend**:

**PK**- Primary Key

**NN** - Not Null

**AI** - Auto Increment

## errorlog

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| timestamp | DATETIME |  ❌  | ✅ | ❌  | CURRENT\_TIMESTAMP |
| status | VARCHAR\(4\) |  ❌  | ✅ | ❌  |   |
| subcode | VARCHAR\(4\) | ❌   | ✅ | ❌  |   |
| title | VARCHAR\(255\) |  ❌  | ✅ | ❌  |   |
| message | VARCHAR\(1000\) |  ❌  |  ❌  | ❌  |  NULL |
| url | VARCHAR\(255\) |  ❌ | ✅ |  ❌ |  |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |

#### Foreign Keys

None.

#### Script

```sql
CREATE TABLE `errorlog` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `timestamp` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `status` varchar(4) NOT NULL,
  `subcode` varchar(4) NOT NULL,
  `title` varchar(45) NOT NULL,
  `message` varchar(1000) DEFAULT NULL,
  `url` varchar(255),
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## events

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| user | INT\(11\) |  ❌  | ✅ | ❌  |   |
| league | VARCHAR\(45\) |  ❌  | ✅ | ❌  |   |
| date | DATE | ❌   | ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |
| fk\_user\_idx | INDEX | user | ASC |
| fk\_league\_idx | INDEX | league | ASC |

#### Foreign Keys

| Foreign Key | Referenced Table | Column | Referenced Column |
| :--- | :--- | :--- | :--- |
| fk\_user | 'couch\_potato'.'users' | user | id |
| fk\_leagues | 'couch\_potato'.'leagues' | league | name |

#### Script

```sql
CREATE TABLE `events` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user` int(11) NOT NULL,
  `league` varchar(45) NOT NULL,
  `date` date NOT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_league_idx` (`league`),
  KEY `fk_user_idx` (`user`),
  CONSTRAINT `fk_leagues` FOREIGN KEY (`league`) REFERENCES `leagues` (`name`),
  CONSTRAINT `fk_user` FOREIGN KEY (`user`) REFERENCES `users` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## games

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| user | INT\(11\) |  ❌  | ✅ | ❌  |   |
| event | INT\(11\) |  ❌  | ✅ | ❌  |   |
| hometeam | VARCHAR\(100\) | ❌   | ✅ | ❌  |   |
| awayteam | VARCHAR\(100\) | ❌   | ✅ | ❌   |   |
| starttime | VARCHAR\(12\) | ❌   | ✅ | ❌   |   |
| homescore | INT\(11\) | ❌   |  ❌   | ❌   |  NULL |
| awayscore | INT\(11\) | ❌   | ❌    | ❌  |  NULL |
| whistle\_start\_time | VARCHAR\(32\) | ❌   | ❌    | ❌   |  NULL |
| whistle\_end\_time | VARCHAR\(32\) | ❌   | ❌    | ❌   |  NULL |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id, user | ASC |
| user\_idx | INDEX | user | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `games` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user` int(11) NOT NULL,
  `event` int(11) NOT NULL,
  `hometeam` varchar(100) NOT NULL,
  `awayteam` varchar(100) NOT NULL,
  `starttime` varchar(12) NOT NULL,
  `homescore` int(11) DEFAULT NULL,
  `awayscore` int(11) DEFAULT NULL,
  `whistle_start_time` varchar(32) DEFAULT NULL,
  `whistle_end_time` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`,`user`),
  KEY `user_idx` (`user`),
  KEY `event_idx` (`event`),
  CONSTRAINT `fk_event` FOREIGN KEY (`event`) REFERENCES `events` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## httplog

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| timestamp | DATETIME |  ❌  | ✅ | ❌  | CURRENT\_TIMESTAMP |
| type | VARCHAR\(12\) |  ❌  | ✅ | ❌  |   |
| url | VARCHAR\(255\) | ❌   | ✅ | ❌  |   |
| uniqueid | VARCHAR\(255\) | ❌   | ✅ | ❌   |   |
| approveid | VARCHAR\(255\) | ❌   | ✅ | ❌   |   |
| message | VARCHAR\(1000\) | ❌   | ✅ | ❌   |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `httplog` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `timestamp` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `type` varchar(12) NOT NULL,
  `url` varchar(255) NOT NULL,
  `uniqueid` varchar(255) NOT NULL,
  `approveid` varchar(255) NOT NULL,
  `message` varchar(1000) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=35 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## incidents

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| timestamp | VARCHAR\(60\) |  ❌  | ✅ | ❌  |   |
| uniquename | VARCHAR\(255\) |  ❌  | ✅ | ❌  |   |
| call | VARCHAR\(20\) | ❌   | ✅ | ❌  |   |
| message | JSON | ❌   | ✅ | ❌   |   |
| url | VARCHAR\(255\) | ❌ | ✅ | ❌ |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `incidents` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `timestamp` varchar(60) NOT NULL,
  `uniquename` varchar(255) NOT NULL,
  `call` varchar(20) NOT NULL,
  `message` json NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=35 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## leagues

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| name | VARCHAR\(45\) |  ❌  | ✅ | ❌  |   |
| sport | INT\(11\) |  ❌  | ✅ | ❌  |   |
| icon | VARCHAR\(64\) | ❌   | ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id, name | ASC |
| idx\_name | INDEX | name | ASC |
| fk\_sport\_idx | INDEX | sport | ASC |

#### Foreign Keys

| Foreign Key | Referenced Table | Column | Referenced Column |
| :--- | :--- | :--- | :--- |
| fk\_sport | 'couch\_potato'.'sports' | sport | id |

#### Script

```sql
CREATE TABLE `leagues` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `sport` int(11) NOT NULL,
  `icon` varchar(64) NOT NULL,
  PRIMARY KEY (`id`,`name`),
  KEY `idx_name` (`name`),
  KEY `fk_sport_idx` (`sport`),
  CONSTRAINT `fk_sport` FOREIGN KEY (`sport`) REFERENCES `sports` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=21 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## progress

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| game | INT\(11\) |  ❌  | ✅ | ❌  |   |
| status | INT\(11\) |  ❌  | ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |
| fk\_status\_idx | INDEX | status | ASC |

#### Foreign Keys

| Foreign Key | Referenced Table | Column | Referenced Column |
| :--- | :--- | :--- | :--- |
| fk\_status | 'couch\_potato'.'status' | status | id |

#### Script

```sql
CREATE TABLE `progress` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `game` int(11) NOT NULL,
  `status` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `idx_progress_game` (`game`),
  KEY `fk_status_idx` (`status`),
  CONSTRAINT `fk_game` FOREIGN KEY (`game`) REFERENCES `games` (`id`),
  CONSTRAINT `fk_status` FOREIGN KEY (`status`) REFERENCES `status` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## sports

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| name | VARCHAR\(45\) | ✅ | ✅ | ❌  |   |
| icon | VARCHAR\(45\) |  ❌  | ✅ | ❌  |   |
| duration | INT\(11\) | ❌  | ❌  | ❌  | NULL |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |
| fk\_leagues\_idx | INDEX | name | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `sports` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `icon` varchar(45) NOT NULL,
  `duration` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`,`name`),
  KEY `fk_leagues_idx` (`name`)
) ENGINE=InnoDB AUTO_INCREMENT=20 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## status

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| name | VARCHAR\(20\) | ❌ | ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `status` (
  `id` int(11) NOT NULL,
  `name` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## teams

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| name | VARCHAR\(100\) | ✅ | ✅ | ❌  |   |
| icon | VARCHAR\(45\) |  ❌  | ✅ | ❌  |   |
| league | INT\(11\) | ❌  |  ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |
| fk\_teams\_leagues\_idx | INDEX | league | ASC |

#### Foreign Keys

| Foreign Key | Referenced Table | Column | Referenced Column |
| :--- | :--- | :--- | :--- |
| fk\_teams\_leagues | 'couch\_potato'.'leagues' | league | id |

#### Script

```sql
CREATE TABLE `teams` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(100) NOT NULL,
  `icon` varchar(45) NOT NULL,
  `league` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_teams_leagues_idx` (`league`),
  CONSTRAINT `fk_teams_leagues` FOREIGN KEY (`league`) REFERENCES `leagues` (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=721 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

## users

#### Columns

| Column | Datatype | PK | NN | AI | Default |
| :--- | :--- | :--- | :--- | :--- | :--- |
| id | INT\(11\) | ✅ | ✅ | ✅ |   |
| timestamp | DATETIME | ❌  | ✅ | ❌  | CURRENT\_TIMESTAMP  |
| username | VARCHAR\(45\) |  ❌  | ✅ | ❌  |   |
| salt | VARCHAR\(128\) | ❌  |  ✅ | ❌  |   |
| password | CHAR\(255\) | ❌  | ✅ | ❌  |   |
| email | VARCHAR\(60\) | ❌  | ✅ | ❌  |   |

#### Indexes

| Index | Type | Columns | Order |
| :--- | :--- | :--- | :--- |
| PRIMARY | PRIMARY | id | ASC |

#### Foreign Keys

None

#### Script

```sql
CREATE TABLE `users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `timestamp` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `username` varchar(45) NOT NULL,
  `salt` varchar(128) NOT NULL,
  `password` char(255) NOT NULL,
  `email` varchar(60) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=38 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci
```

