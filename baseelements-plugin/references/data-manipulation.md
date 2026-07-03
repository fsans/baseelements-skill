# Data Manipulation — BaseElements Plugin

## BE_StackPush

```
BE_StackPush ( name ; value )
```

**Description**

Puts a value into the stack called name and "pushes" everything down the stack one value deeper.

**Parameters**

* *name* : The name of the stored stack.
* *value* : The value to store in the stack.

**Version History**

* 4.1.0 : First Release

**Notes**

A stack is an internal plugin storage type, where you push data into a stack ( like a list of values ) and Pop data out. In the plugin, stacks are LIFO ( Last In First Out ) so when you push a value onto the named stack, it will be the next value to POP out of the stack. 

See this for more info : [https://www.thoughtco.com/definition-of-stack-in-programming-958162](https://www.thoughtco.com/definition-of-stack-in-programming-958162)

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

## BE_StackPop

```
BE_StackPop ( name )
```

**Description**

Returns the first value from the stack called *name* and removes it from the stack, reducing the stack count by 1.

**Parameters**

* *name* : the name of the stored stack.

**Version History**

* 4.1.0 : First Release

**Notes**

Be careful when testing code and using the Pop function in the data viewer as once a value is removed from the stack, it's deleted - that's the intention :)

A stack is an internal plugin storage type, where you push data into a stack ( like a list of values ) and Pop data out. In the plugin, stacks are LIFO ( Last In First Out ) so when you push a value onto the named stack, it will be the next value to POP out of the stack. 

See this for more info : [https://www.thoughtco.com/definition-of-stack-in-programming-958162](https://www.thoughtco.com/definition-of-stack-in-programming-958162)

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

## BE_StackCount

```
BE_StackCount ( name )
```

**Description**

This function returns a count of the total number of values remaining in stack *name*.

**Parameters**

* *name* : The name of the stored stack.

**Version History**

* 4.1.0 : First Release

**Notes**

A stack is an internal plugin storage type, where you push data into a stack ( like a list of values ) and Pop data out.  In the plugin, stacks are LIFO ( Last In First Out ) so when you push a value onto the named stack, it will be the next value to POP out of the stack.

Stacks have names, much like variables do.

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

## BE_StackDelete

```
BE_StackDelete ( name )
```

**Description**

This function deletes the stack called *name* regardless of how many values it has in it.  Mostly only for cleanup purposes, stacks are removed when empty.

**Parameters**

* *name* : the name of the stored stack.

**Version History**

* 4.1.0 : First Release

**Notes**

A stack is an internal plugin storage type, where you push data into a stack ( like a list of values ) and Pop data out.  In the plugin, stacks are LIFO ( Last In First Out ) so when you push a value onto the named stack, it will be the next value to POP out of the stack.

Stacks have names, much like variables do.

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

## BE_VariableSet

```
BE_VariableSet ( name ; { value } )
```

**Description**

Sets *value* into a plugin preference with name of *name*.

**Parameters**

* *name* : the name of the variable to return.
* *value* ( optional ) : the value to store.  An empty value will delete that variable.

**Version History**

* 4.1.0 : First Release

**Notes**

Plugin variables are much like $local and $$global FileMaker variables, except the scope is the instance of the plugin, so will persist across FileMaker files and even on closing and opening of files.  They are only lost when the plugin is loaded or unloaded, usually only ever when FileMaker restarts.

On FileMaker Server they exist in a single session, and not across different sessions as each session is a new application launch effectively.

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

## BE_VariableGet

```
BE_VariableGet ( name )
```

**Description**

Gets the value of the previously stored plugin variable with *name*.

**Parameters**

* *name* : the name of the variable to return.

**Version History**

* 4.1.0 : First Release

**Notes**

Plugin variables are much like $local and $$global FileMaker variables, except the scope is the instance of the plugin, so will persist across FileMaker files and even on closing and opening of files.  They are only lost when the plugin is loaded or unloaded, usually only ever when FileMaker restarts.

On FileMaker Server they exist in a single session, and not across different sessions as each session is a new application launch effectively.

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

## BE_TextExtractWords

```
BE_TextExtractWords ( text ; { wordPrefix } )
```

**Description**

This returns a list of all the "words" in the *text* that start with the *wordPrefix* character.

**Parameters**

* *text* :  the text to search on.
* *wordPrefix* ( optional, default:$ or @ ) : a single chararacter to use as the first character of the words to find.

**Version History**

* 1.0.0 : First Release

**Notes**

Originally an internal only function, this was not exposed as an option in the functions list but as of 4.2, we've made this one visible in case it's useful to others.

Our use case for this was to use it inside the BaseElements developer tool engine, to retrieve all the variables from a block of calculation text, but getting anything that starts with $.  We expanded it to @ for other  similar functionality.

Words are delimited ( ended ) by one of the following characters : 

	; +-=*/&^<>\t\r[]()≠≤≥,

Note that this list of characters includes a space.  The ones starting with \u are single unicode characters.

Also note that this function leaves out any text within FileMaker comments, so from // to the endOfLine and from /* to */  So it won't find any words inside those characters.

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

	BE_TextExtractWords ( "apple bear $ball" ) = "$ball"
