# Schema

For validation of the data format presented in the sports folder, a validation is performed. 

The corresponding validation schemata are stored in the schema/ subdirectory and used internally when instantiating **bookiesports.BookieSports**.

## Schemata

### General Definitions

```yaml
definitions:
 identifier:
  type: string
  description: Identification string for the 
 id:
  description: Blockchain id of the object (e.g. 1.16.0)
  pattern: "^[0-9]*\\.[0-9]*\\.[0-9]*$"
 internationalized_name:
  type: object
  description: Internationalized name
  properties:
   en:
    type: string
    description: English name of the sport
  required:
   - en
 aliases:
  type:
   - "null"
   - array
  oneOf:
   - type: "null"
   - type: array
     description: List of known aliases
     items:
      type: string
 asset:
  type: array
  description: Asset symbol
  uniqueItems: true
  items:
   - type: string
   - enum:
      - PPY
      - BTC
      - BTCTEST
      - BTF
      - BTFUN
      - TEST
```

### Sport

```yaml
$schema: "http://json-schema.org/draft-06/schema#"
title: BookieSports::Sport
description: Format for BookieSports::Sport
type: object
properties:
 identifier:
  $ref: "#/definitions/identifier"
 name:
  $ref: "#/definitions/internationalized_name"
 aliases:
  $ref: "#/definitions/aliases"
 id:
  $ref: "#/definitions/id"
 eventgroups:
  type: array
  description: list of event groups that are in this sport
  items:
   type: string
required:
 - identifier
 - name
 - id
 - eventgroups
```

### Eventgroup

```yaml
$schema: "http://json-schema.org/draft-04/schema#"
title: BookieSports::EventGroup
description: Format for BookieSports::EventGroup
type: object
properties:
 identifier:
  $ref: "#/definitions/identifier"
 name:
  $ref: "#/definitions/internationalized_name"
 aliases:
  $ref: "#/definitions/aliases"
 id:
  $ref: "#/definitions/id"
 participants:
  description: Identifier for the teams
  type: string
 bettingmarketgroups:
  type: array
  description: list of identifiers for the betting market groups
  items:
   type: string
 eventscheme:
  type: object
  description: Internationalized name after which the events are named on creation
  properties:
   name:
    $ref: "#/definitions/internationalized_name"
 start_date:
  type: string
  format: date-time
 finish_date:
  type: string
  format: date-time
 leadtime_Max:
  type: number
required:
 - identifier
 - name
 - id
 - participants
 - bettingmarketgroups
 - eventscheme
   #- start_date
   #- finish_date
   #- leadtime_Max

```

### Participant

```yaml
$schema: "http://json-schema.org/draft-06/schema#"
title: BookieSports::MarketBettingGroup
description: Format for BookieSports::MarketBettingGroup
type: object
properties:
 participants:
  description: List of participants
  type: array
  items:
   type: object
   properties:
    identifier:
     $ref: "#/definitions/identifier"
    aliases:
     $ref: "#/definitions/aliases"
    name:
     $ref: "#/definitions/internationalized_name"
required:
 - participants
```

### Rule

```yaml
$schema: "http://json-schema.org/draft-06/schema#"
title: BookieSports::MarketBettingGroup
description: Format for BookieSports::MarketBettingGroup
type: object
properties:
 identifier:
  $ref: "#/definitions/identifier"
 name:
  $ref: "#/definitions/internationalized_name"
 description:
  $ref: "#/definitions/internationalized_name"
 id:
  $ref: "#/definitions/id"
 grading:
  type: object
  description: Grading for the rule
  properties:
   metric:
    type: string
    description: Calculate metric according to this
   resolutions:
    type: array
    descriptions: Resolve betting markets according to the rules here
    items:
     type: object
     properties:
      win:
       type: string
       description: If true this market is win
      not_win:
       type: string
       description: If true this market is not_win
      void:
       type: string
       description: If true this market is void
  required:
   - metric
   - resolutions
required:
 - identifier
 - id
 - name
 - description
 - grading
```

### BettingMarketGroup

```yaml
$schema: "http://json-schema.org/draft-06/schema#"
title: BookieSports::MarketBettingGroup
description: Format for BookieSports::MarketBettingGroup
type: object
properties:
 description:
  $ref: "#/definitions/internationalized_name"
 asset:
  $ref: "#/definitions/asset"
 dynamic:
  description: Is this a dynamic BMG (like a NFL handicap or NBA Over-under BMG)?
  anyOf:
   - type: string
     enum:
      - ou  # Over under
      - hc  # handicap
   - type: boolean
 number_betting_markets:
  type: number
  description: Number of Betting Markets in this BMG
  items:
   type: string
 is_live:
  type: boolean
  description: WIll this BMG be turned Live at Event start? This is YES for all BMGs at launch
 rules:
  type: string
  description: Human readable rules that the Grading Algorithm is a machine-readable instantiation of
 bettingmarkets:
  type: array
  description: Betting markets to open
  items:
   type: object
   properties:
    description:
     $ref: "#/definitions/internationalized_name"
required:
 - description
 - asset
 - dynamic
 - number_betting_markets
 - is_live
 - rules
 - bettingmarkets
```

