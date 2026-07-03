# Deprecated Functions — BaseElements Plugin

These functions have been removed or superseded. Do not use them in new solutions. Use the native FileMaker replacement noted under each function.

## BE_Base64_Encode

> **Deprecated** — Use the native FileMaker 16 function *Base64Encode* instead.

```
BE_Base64_Encode ( text { ; name } )
```

**Description**
Decodes text from Base64 into it's original value.

**Parameters**
* *text* : the text to decode.
* *name* ( optional ) : the name of the output file for binary content.

**Version History**
* 1.3.0 : First Release
* 4.0.0 : Deprecated. Use the FileMaker 16 function *Base64Encode* instead.
* 4.2.0 : Removed.

---

## BE_Base64_Decode

> **Deprecated** — Use the native FileMaker 16 function *Base64Decode* instead.

```
BE_Base64_Decode ( text { ; name } )
```

**Description**
Decodes text from Base64 into it's original value.

**Parameters**
* *text* : the text to decode.
* *name* ( optional ) : the name of the output file for binary content.

**Version History**
* 1.3.0 : First Release
* 4.0.0 : Deprecated. Use the FileMaker 16 function *Base64Decode* instead.
* 4.2.0 : Removed.

---

## BE_Base64_URL_Encode

> **Deprecated** — Use the native FileMaker 16 function *Base64EncodeRFC* instead.

```
BE_Base64_URL_Encode ( data )
```

**Description**
Encodes data as Base64 values with url safe encoding. URL safe encoding gives output that is consistent with other online tools such as php.

**Parameters**
* *data* : the data to encode.

**Version History**
* 2.0.0 : First Release
* 4.0.0 : Deprecated. Use the FileMaker 16 function *Base64EncodeRFC* instead.
* 4.2.0 : Removed.

---

## BE_HMAC

> **Deprecated** — Use the internal FileMaker function *CryptAuthCode* instead.

```
BE_HMAC ( text ; key ; { algorithm ; outputEncoding ; inputEncoding } )
```

**Description**
Does HMAC encoding on the *text*, using the *key* with a specific *algorithm* and *outputEncoding*, assuming a certain *inputEncoding* for the text and key.

**Parameters**
* *text* : the text to be encoded.
* *key* : the key to encode with.
* *algorithm* ( optional, default:BE_MessageDigestAlgorithm_SHA1 ) : Any of the BE_MessageDigestAlgorithm options.
* *outputEncoding* ( optional, default:BE_Encoding_Hex ) : *BE_Encoding_Hex* or *BE_Encoding_Base64*.
* *inputEncoding* ( optional, default:BE_Encoding_Hex ) : *BE_Encoding_Hex* or *BE_Encoding_Base64*.

**Version History**
* 3.0.0 : First Release
* 4.0.0 : Deprecated. Use internal *CryptAuthCode* instead.
* 4.2.0 : Removed.

---

## BE_JSON_Encode

> **Deprecated** — Use the internal FileMaker JSON functions instead.

```
BE_JSON_Encode ( key ; { value ; type } )
```

**Description**
Encodes a single value as JSON encoded text.

**Parameters**
* *key* : the name of the key value to encode.
* *value* ( optional ) : The value of the key to encode.
* *type* ( optional ) : The type is determined automatically from fields content.

For text that you want to force to be a different type, use the *type* parameter. Possible values are Boolean ( JSON True or False ), Text or Number ( JSON integer or real, but also includes FileMaker date, time or timestamp ). Any other values will error.

**Version History**
* 2.1.0 : First Release
* 4.0.0 : Deprecated. Use internal JSON functions instead.
* 4.2.0 : Removed.

---

## BE_JSONPath

> **Deprecated** — Use the internal FileMaker JSON functions instead.

```
BE_JSONPath ( json ; path )
```

**Description**
Query json structure using an XPath like syntax.

**Parameters**
* *json* : the json to parse.
* *path* : the path to the data you want.

**Version History**
* 2.1.0 : First Release
* 4.0.0 : Deprecated. Use internal JSON functions instead.
* 5.0.0 : Removed.

---

## BE_JSON_Error_Description

> **Deprecated** — Use the internal FileMaker JSON functions instead.

```
BE_JSON_Error_Description
```

**Description**
A text description of the last JSON error.

**Parameters**
None

**Version History**
* 2.1.0 : First Release
* 4.0.0 : Deprecated. Use internal JSON functions instead.
* 5.0.0 : Removed.

---

## BE_ExecuteShellCommand

> **Deprecated** — Use *BE_ExecuteSystemCommand* instead.

```
BE_ExecuteShellCommand ( command { ; waitForResponse } )
```

**Description**
Performs a command line script of the *data* parameter.

**Parameters**
* *command* : the command.
* *waitForResponse* : True or False.

**Version History**
* 1.1.0 : First Release
* 1.2.0 : Modified to include an optional waitForResponse parameter
* 2.0.0 : Deprecated. Use *BE_ExecuteSystemCommand* instead.
* 3.0.0 : Removed.

---

## BE_FileMaker_Fields

> **Deprecated** — Use the internal FileMaker *ExecuteSQL* function instead.

```
BE_FileMaker_Fields
```

**Description**
Returns a list of the fields in the current open file.

**Parameters**
None

**Version History**
* 1.1.0 : First Release
* 2.0.0 : Deprecated. Use internal *ExecuteSQL* instead.
* 3.0.0 : Removed.

---

## BE_FileMaker_Tables

> **Deprecated** — Use the internal FileMaker *ExecuteSQL* function instead.

```
BE_FileMaker_Tables
```

**Description**
Returns a list of the Tables in the current open file.

**Parameters**
None

**Version History**
* 1.1.0 : First Release
* 2.0.0 : Deprecated. Use internal *ExecuteSQL* instead.
* 3.0.0 : Removed.

---

## BE_XeroSetTokens

> **Deprecated** — Xero no longer uses this auth mechanism and has switched to an OAuth2 compatible process.

```
BE_XeroSetTokens ( consumer_key ; private_key )
```

**Description**
Allows authentication to the Xero API using private keys, for use in any subsequent HTTP calls.

**Parameters**
* *consumer_key* : the json to parse.
* *private_key* : the path to the data you want.

**Version History**
* 3.0.0 : First Release
* 4.1.4 : Deprecated. Xero no longer uses this auth mechanism, and has switched to a OAuth2 compatible process.
* 4.2.0 : Removed.

---
