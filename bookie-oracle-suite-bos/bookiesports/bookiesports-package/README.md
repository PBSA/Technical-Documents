# BookieSports Module Contents

_class_ bookiesports.**BookieSports**\(_chain=None, override\_cache=False, \*\*kwargs_\)

Bases: **dict**

This class allows to read the data provided by BookieSports

On instantiation of this class the following procedure happens internally:

1. Open the directory that stores the sports
2. Load all Sports
3. For each sport, load the corresponding data subset \(event  groups, events, rules, participants, etc.\)
4. Validate each data subset
5. Perform consistency checks
6. Instantiate a dictionary \(`self`\)

As a result, the following call will return a dictionary with all the BookieSports:

```python
from bookiesports 
import BookieSports x = BookieSports()
```

**Parameters:**

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <ul>
          <li><b>chain</b> (string) &#x2013; One out &#x2018;alice&#x2019;, &#x2018;beatrice&#x2019;,
            or &#x2018;charlie&#x2019; to identify which network we are working with.
            Can also be a relative path to a locally stored copy of a sports folder</li>
          <li><b>override_cache</b> (string) &#x2013; if true, cache is ignored and sports
            folder is forcibly reloaded and put into cache</li>
          <li><b>network</b> (string) &#x2013; deprecated, please use chain</li>
        </ul>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>{% hint style="warning" %}
**Note**: It is possible to overload a custom sports\_folder by providing it to BookieSports as parameter.
{% endhint %}

**BASE\_FOLDER** = `'/home/docs/checkouts/readthedocs.org/user_builds/bookiesports/envs/latest/lib/python3.7/site-packages/bookiesports-0.4.10-py3.7.egg/bookiesports/bookiesports'`

**CHAIN\_CACHE** = {}

        Singelton to store data and prevent re- reading if BookieSports is instantiated multiple times

**DEFAULT\_CHAIN** = '_beatrice_'

**JSON\_SCHEMA** = _None_

Schema for validation of the data

**SPORTS\_FOLDER** = _None_

**chain\_id**

_static_ **list\_chains**\(\)

_static_ **list\_networks**\(\)

        @deprecated, use list\_chains

**network**

        @deprecated, use self.index

**network\_name**

        @deprecated, use self.chain

static **version**\(\)[¶](https://bookiesports.readthedocs.io/en/latest/bookiesports.html#bookiesports.BookieSports.version)

