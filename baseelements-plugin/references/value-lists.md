# Value Lists — BaseElements Plugin

## BE_ValuesSort

```
BE_ValuesSort ( listOfValues ; { ascendingBoolean ; type } )
```

**Description**
Sorts *listOfValues* according to the ascending order *ascendingBoolean* and data *type*.

**Parameters**
* *listOfValues* : The list of values to start with.
* *ascendingBoolean* ( optional, default:1 ) : 1 for ascending, 0 for descending.
* *type* ( optional, default:0 ) : A number corresponding to the value for different types.  At the moment, only two values are defined : 0 for Text, 1 for Numeric.

**Version History**
* 2.1.0 : First Release
* 3.3.0 : Added the ascending and type parameters
* 4.0.2 : Renamed from BE_Values_Sort

**Notes**
In some situations this sort order won't be the same as the order that FileMaker would apply in a sorted field.  Different encoding and "type" arrangements treat values in different ways, so there's no expectation that the BE sort will be the same as the FM one - the technical details into why this is gets really complex as you get into text encoding and storage and values and ... If you're after a good explanation of why this is no longer a simple thing, this is a good reference : [https://tonsky.me/blog/unicode/](https://tonsky.me/blog/unicode/)

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_ValuesUnique

```
BE_ValuesUnique ( listOfValues ; { caseSensitiveBoolean } )
```

**Description**
Removes duplicate values from listOfValues.

**Parameters**
* *listOfValues* : The list of values to start with.
* *caseSensitiveBoolean* ( optional, default:True ) : Whether to treat "ABC" the same as "abc" or not.

**Version History**
* 2.1.0 : First Release
* 3.1.0 : Added caseSensitiveBoolean option
* 4.0.2 : Renamed from BE_Values_Unique

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_ValuesUnique ( "a¶a¶b¶b" ) = "a¶b"

---

## BE_ValuesFilterOut

```
BE_ValuesFilterOut ( textToFilter ; filterValues ; { caseSensitiveBoolean } )
```

**Description**
Does the opposite of the *FilterValues* function : it takes the *textToFilter* parameter, and removes anything in the *filterValues* list and returns the remaining values.

**Parameters**
* *textToFilter* : The list of values to start with.
* *filterValues* : The values to remove from textToFilter.
* *caseSensitiveBoolean* ( optional, default:True ) : Whether to treat "ABC" the same as "abc" or not.

**Version History**
* 2.1.0 : First Release
* 3.1.0 : Added the optional caseSensitive parameter
* 4.0.2 : Renamed from BE_Values_FilterOut

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_ValuesFilterOut ( "a¶b¶c¶d" ; "b¶d" ) = "a¶c"

---

## BE_ValuesContainsDuplicates

```
BE_ValuesContainsDuplicates ( listOfValues ; { caseSensitiveBoolean } )
```

**Description**
Tests a list of values in FileMaker value list format and returns a True ( 1 ) or False ( 0 ) value if it contains duplicate values.

**Parameters**
* *listOfValues* : A FileMaker Value List ( ¶ separated values )..
* *caseSensitiveBoolean* ( optional, default:True ) : Whether to treat "ABC" the same as "abc" or not.

**Version History**
* 3.1.0 : First Release
* 3.1.0 : Renamed from BE_Values_ContainsDuplicates

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---

## BE_ValuesTimesDuplicated

```
BE_ValuesTimesDuplicated ( listOfValues ; numberOfTimes )
```

**Description**
Select only those values from *listOfValues*, where the value has been repeated *numberOfTimes*.  In other words, filter the list to only those values that are repeated numberOfTimes.

**Parameters**
* *listOfValues* : The list of values to look through.
* *numberOfTimes* : The count to check for in the list.

**Version History**
* 3.2.0 : First Release
* 4.0.2 : Renamed from BE_Values_TimesDuplicated

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

**Example Code**

	BE_ValuesTimesDuplicated ( "a¶c¶c¶d¶d" ; 2 ) = "c¶d"
	BE_ValuesTimesDuplicated ( "a¶c¶c¶d¶d" ; 3 ) = ""

---

## BE_ValuesTrim

```
BE_ValuesTrim ( listOfValues )
```

**Description**
Trims leading whitespace from each and every value in listOfValues.  Works the same as if you called the regular Trim recursively on each GetValue ( listOfValues ; n ) and then re-assembled the list result.

**Parameters**
* *listOfValues* : The list of values to start with.

**Version History**
* 3.3.0 : First Release
* 4.0.2 : Renamed from BE_Values_Trim

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

---
