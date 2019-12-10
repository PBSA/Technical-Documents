# Naming Scheme

Some BookieSports files \(in particular name and description fields\) allow the use of variables. Those are dynamic and filled in by bookie-sync, automatically.

As an example, the file MLB\_ML\_1.yaml defines betting markets for a Moneyline market group. The betting markets carry the name of the event participants. We encode this in BookieSports using variables:

```text
bettingmarkets:
     - description:
         en: '{teams.away}'
     - description:
         en: '{teams.home}'
```

## Overview of variables

* teams:
  * {**teams.home**}: Home team
  * {**teams.away**}: Away team
* result:
  * {**teams.home**}: Points for home team
  * {**teams.away**}: Points for away team
  * {**teams.hometeam**}: Points for home team
  * {**teams.awayteam**}: Points for away team
  * {**teams.tota**l}: Total Points
* handicaps:
  * {**teams.home**}: Comparative \(symmetric\) Handicaps \(e.g., +-2\) for home team
  * {**teams.away**}: Comparative \(symmetric\) Handicaps \(e.g., +-2\) for away team
  * {**teams.home\_score**}: Absolute handicap for home team \(e.g., 2\)
  * {**teams.away\_score**}: Absolute handicap for away team \(e.g., 0\)
* overunder:
  * {**teams.value**}: The over-/under value

## Internal Processing

The variable parsing is done in bos-sync \(substitutions.py\) and work through `decode_variables` and a few classes that deal with the variables. This allows us to have complex variable substitutions.

The variables all consist of a **module identifier** and the actual **member variable**:

```python
{module.member}
```

All modules are listed in the substitutions variable in `decode_variables`::

```python
substitutions = {
    "teams": Teams,
    "result": Result,
    "handicaps": Handicaps,
    "overunder": OverUnder,
}
```

The modules themselves \(capitalized first letter\) are defined in the same file and can be as easy as:

```python
class Result:
    """ Defines a few variables to be used in conjuctions with {result.X}
    """
    def __init__(self, **kwargs):
        result = kwargs.get("result", [0, 0]) or [0, 0]
        self.hometeam = result[0]
        self.awayteam = result[1]
        self.total = sum([float(x) for x in result])
        # aliases
        self.home = self.hometeam
        self.away = self.awayteam

```

or as complex as:

```python
class Teams:
    """ Defines a few variables to be used in conjuctions with {teams.X}
    """
    def __init__(self, **kwargs):
        teams = kwargs.get("teams", ["", ""]) or ["", ""]
        self.home = " ".join([
            x.capitalize() for x in teams[0].split(" ")])
        self.away = " ".join([
            x.capitalize() for x in teams[1].split(" ")])
```

