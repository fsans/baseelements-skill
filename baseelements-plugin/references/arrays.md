# Arrays — BaseElements Plugin

## BE_ArraySetFromValueList

```
BE_ArraySetFromValueList ( valueList ; { retainEmptyValues } )
```

**Description**

Stores the valueList as an array within the plugin memory space and returns an index number that you can use to reference the array via memory.

**Parameters**

* *valueList* : The standard FileMaker return separated list of values to store.
* *retainEmptyValues* ( optional, default:False ) : Whether or not to include empty values as array elements.

**Version History**

* 3.3.0 : First Release
* 4.0.0 : added the *retainEmptyValues* parameter

**Notes**

BE plugin value lists have by default a difference when compared to FileMaker value lists.  The default option for *retainEmptyValues* ignores empty values, so "a¶¶b¶" becomes an array of [a,b] - in other words an array of two values.  Be careful if you're assuming that empty values are retained, or set the *retainEmptyValues* parameter to True if you want to preserve FileMaker native list expectations.

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

## BE_ArrayGetValue

```
BE_ArrayGetValue ( array ; valueNumber )
```

**Description**

Retrieves a value from the *array* number specified, at the *valueNumber* specified.

Arrays are stored via BE_ArraySetFromValueList and the index number used as the *array* parameter is the result of that function call.

**Parameters**

* *array* : A previously returned array number.
* *valueNumber* : The index number into the array to retrieve.

**Version History**

* 3.3.0 : First Release

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

## BE_ArrayChangeValue

```
BE_ArrayChangeValue ( array ; valueNumber ; newValue )
```

**Description**

Changes the *array* item at *valueNumber* to *newValue*.

**Parameters**

* *array* : the number of the array that you got back when you created the array initially.
* *valueNumber* : the element number to change.
* *newValue* : the new value to change the element to.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from 'BE_Array_Change_Value'

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

## BE_ArrayFind

```
BE_ArrayFind ( array ; value )
```

**Description**

Searches through the array in numerical order until it finds an array item with value value, and returns the item number.

**Parameters**

* array : the number of the array to search in.
* value : the value to search for.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from BE_Array_Find

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

## BE_ArrayGetSize

```
BE_ArrayGetSize ( array )
```

**Description**

Returns the size in number of elements in a stored array.  Technically equivalent to ValueCount for the list before it was stored as an array.

**Parameters**

* *array* : The index number of the previously stored array.

**Version History**

* 3.3.0 : First Release

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

## BE_ArrayDelete

```
BE_ArrayDelete ( array )
```

**Description**

Removes *array* from memory.

**Parameters**

* *array* : the number of the array to be deleted.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from 'BE_Array_Delete'

**Notes**

Be careful, there is no undo

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |
