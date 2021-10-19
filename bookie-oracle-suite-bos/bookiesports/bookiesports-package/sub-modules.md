# Sub Modules

## bookiesports.cli module

## bookiesports.datestring module

`bookiesports.datestring.date_to_string`(_date\_object=None_)

rfc3339 conform string representation of a date can also be given as str YYYY-mm-dd HH:MM:SS

`bookiesports.datestring.string_to_date`(_date\_string=None_)

assumes rfc3339 conform string and creates date object

## bookiesports.exceptions module

_exception _`bookiesports.exceptions.SportsNotFoundError`

Bases: `Exception`

## bookiesports.log module



## bookiesports.normalize module

_exception _**`bookiesports.normalize.EventGroupNotNormalizableException`**

Bases: **`bookiesports.normalize.NotNormalizableException`**

_class _**`bookiesports.normalize.IncidentsNormalizer`**(_chain=None_)

Bases: **`object`**

This class serves as the normalization entry point for incidents. All events / event group and participant names are replaced with the counterpart stored in the BookieSports package.

**`DEFAULT_CHAIN`**_ = 'beatrice'_

&#x20;       default chosen chain for BookieSports

**`NOT_FOUND`**_ = {}_

&#x20;       As class variable to have one stream for missing normalization entries

**`NOT_FOUND_FILE`**_ = None_

&#x20;      If normalization errors should be written to file, set file here

**`normalize`**(_incident_, _errorIfNotFound=False_)

_        static _**`not_found`**(_key_)

_        static _**`use_chain`**(_chain_, _not\_found\_file=None_)

_exception _**`bookiesports.normalize.NotNormalizableException`**

&#x20;       Bases: **`Exception`**

_exception _**`bookiesports.normalize.ParicipantNotNormalizableException`**

&#x20;       Bases: **`bookiesports.normalize.NotNormalizableException`**

_exception _**`bookiesports.normalize.SportNotNormalizableException`**

&#x20;       Bases: **`bookiesports.normalize.NotNormalizableException`**
