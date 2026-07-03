# XML, XSLT, and JSON — BaseElements Plugin

## BE_XPath

```
BE_XPath ( xmlText ; xpathText ; { namespaceListText ; asTextBoolean } )
```

**Description**

This finds the first instance of a node at the path *xpathText* within the text *xmlText*.

**Parameters**

* *xmlText* : the XML text to use.
* *xpathText* : the path to the node you require.
* *namespaceListText* ( optional ) : Namespace list as "prefix1=href1 prefix2=href2...".
* *asTextBoolean* ( optional, default:False ) : Whether to return the XML nodeset as text instead of the standard XPath result.

The *asTextBoolean* parameter when set to True allows you to get raw XML from the source - normally where the XPath function would return a node, with it's opening and closing tag, this will return the value inside that tag instead.

**Version History**

* 1.0.0 : First Release
* 1.2.0 : 
* 2.2.0 : Added the optional asText parameter

**Notes**

This function is based on the libxml library which only supports XPath 1.0 [http://xmlsoft.org]

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

	BE_XPath ( $XML ; "/abc:checkUserResponse/abc:result/abc:accountDetails/@type" ; "abc=http://xml.m4u.com.au/2009" )

---

## BE_XPathAll

```
BE_XPathAll ( xmlText ; xpathText ; { namespaceListText } )
```

**Description**

This finds all instances of a node at the path *xpathText* within the text *xmlText*.

**Parameters**

* *xmlText* : the XML text to use.
* *xpathText* : the path to the node you require.
* *namespaceListText* ( optional ) : Namespace list as "prefix1=href1 prefix2=href2...".

**Version History**

* 1.2.0 : First Release
* 2.2.0 : support objects of type XPATH_BOOLEAN, XPATH_NUMBER and XPATH_STRING
* 2.2.1 : return an empty string when getting an empty node set as xml

**Notes**

This function is based on the libxml library which only supports XPath 1.0 [http://xmlsoft.org]

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

	BE_XPath ( $XML ; "/abc:checkUserResponse/abc:result/abc:accountDetails/@type" ; "abc=http://xml.m4u.com.au/2009" )

---

## BE_XSLT_ApplyInMemory

```
BE_XSLT_ApplyInMemory ( xmlText ; xsltText )
```

**Description**

Applies the XSLT given by the *xsltText* parameter, to the XML given by the *xmlText* parameter, and and returns the result.

**Parameters**

* *xmlText* : the xml to process.
* *xsltText* : the text content of the xslt to process with.

**Version History**

* 1.2.0 : First Release

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

## BE_XSLT_Apply

```
BE_XSLT_Apply ( xmlFilePath ; xsltText ; outputFilePath {; scriptName ; databaseName ; [ xsltText ; outputFilePath ] ; ... } )
```

**Description**

Applies the XSLT given by the *xsltText* parameter, to the XML file at *xmlFilePath*, and writes the output to *outputFilePath*.

**Parameters**

* *xmlFilePath* : the plugin path to the xml file to process.
* *xsltText* : the text content of the xslt to process with.
* *outputFilePath* : the plugin path to write the output result to.
* *scriptName* : a script to call on completion of the transform.
* *databaseName* : the database where scriptName resides.

To launch the XSLT process in its own thread add the *scriptName* and optional *databaseName* parameter.  When the *scriptName* parameter is used, then the XSLT process is launched in a background thread and control returns to FileMaker immediately.

This function also allows you to launch multiple threads of the XSLT processing to happen simultaneously by appending groups of two extra parameters after the databaseName parameter, with each additional *xsltText* and *outputFilePath* in pairs.

**Version History**

* 1.0.0 : First Release
* 5.0.0 : Added scriptName, and optional extra parameters.

**Notes**

When the *scriptName* option is used, the script parameter sent to *scriptName* is the response from the XSLT function if present, or the *outputFilePath* otherwise.

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

This is a complete list of all the xslt functions you can use in BE, as dug out of the code by Beverly Voth :

	<?xml version="1.0"?>
	
	=== 24 Standard elements:
	xsl:apply-templates available
	xsl:apply-imports available
	xsl:call-template available
	xsl:element available
	xsl:attribute available
	xsl:text available
	xsl:processing-instruction available
	xsl:comment available
	xsl:copy available
	xsl:value-of available
	xsl:number available
	xsl:for-each available
	xsl:if available
	xsl:choose available
	xsl:sort available
	xsl:copy-of available
	xsl:message available
	xsl:variable available
	xsl:param available
	xsl:with-param available
	xsl:decimal-format available
	xsl:when available
	xsl:otherwise available
	xsl:fallback available
	
	=== 5 Extension elements:
	xsl:element available
	saxon:output available
	xalanredirect:write available
	xt:document available
	libxslt:debug available
	
	=== 6 Extension functions:
	libxslt:node-set() available
	saxon:node-set() available
	xt:node-set() available
	saxon:evaluate() available
	saxon:expression() available
	saxon:eval() available

---

## BE_XMLParse

```
BE_XMLParse ( pathOrXMLText )
```

**Description**

Parses an XML file at *pathOrXMLText* to determine if it's properly formed. Does not check XSD details, just whether every tag is properly opened and closed.

The result will be empty when successful with a *BE_GetLastError* returning 0.

**Parameters**

* *pathOrXMLText* : the xml text or a plugin path to locate the file to parse.

**Version History**

* 2.2.0 : First Release
* 4.0.2 : Renamed from BE_XML_Parse

**Notes**

This function will look at the content of the text in the pathOrXMLText parameter, and if the first character is < then it assumes it is XML and treats it as such.  Any other first character means it assumes it's a path and will try to use it as a path on disk.

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

## BE_XMLValidate

```
BE_XMLValidate ( xmlText ; schemaText )
```

**Description**

Validates the XML in *xmlText* against an XSD *schemaText* for correctness.

**Parameters**

* *xmlText* : the XML text to be validated.
* *schemaText* : the XSD schema text to validate against.

**Version History**

* 4.0.0 : First Release
* 4.0.2 : Renamed from BE_XML_Validate

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

## BE_XMLTidy

```
BE_XMLTidy ( xml )
```

**Description**

Does a "tidy" to reformat nicely the XML text.

**Parameters**

* *xml* : the xml text to be cleaned up.

**Version History**

* 4.2.0 : First Release

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

## BE_XML_Canonical

```
BE_XML_Canonical ( xml )
```

**Description**

Generates the Canonical version of given *xml* document - Canonical XML is otherwise known as a "normal form" of XML, intended to allow relatively simple comparison of pairs of XML documents for equivalence.

Result is the output Canonical XML text.

**Parameters**

* *xml* : The text content of the XML to be used.

**Version History**

* 4.1.2 : First Release

**Notes**

Canonical XML is a specially structured version of XML that applies certain rules and will therefore be able to compared or used in certain validation processes.

See [https://en.wikipedia.org/wiki/Canonical_XML](https://en.wikipedia.org/wiki/Canonical_XML) for more information about Canonical XML.

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

## BE_XMLStripNodes

```
BE_XMLStripNodes ( inputFilePath ; outputFilePath ; nodeNames )
```

**Description**

This function removes the supplied list *nodeNames* of XML nodes from the source document at *inputFilePath* and writes the result to *outputFilePath*.

**Parameters**

* *inputFilePath* : the plugin path to the XML file to read from.
* *outputFilePath* : the plugin path to write the output to.
* *nodeNames* : a space separated list of xml node names to remove.

**Version History**

* 4.2.0 : First Release

**Notes**

Previously an internal hidden function, was made public in 4.2.0.

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

To remove the *HexData* and *PlatformData* nodes from an XML DDR file, you can use :

	BE_XMLStripNodes ( $inputFilePath ; $outputFilePath ; "HexData PlatformData" )

---

## BE_XMLStripInvalidCharacters

```
BE_XMLStripInvalidCharacters ( path ; { resultFilePath } )
```

**Description**

This function removes characters considered invalid in XML from the file at *path*, and optionally writes the fixed file to the *resultFilePath*.

**Parameters**

* *path* : The path to the text XML file to read.
* *resultFilePath* ( optional, default:path ) : If supplied, this will write the output to a new file. If not supplied then the original file will be overwritten.

**Version History**

* 4.2.0 : First Release

**Notes**

Used originally internal as a BaseElements developer tool function, likely not useful for anyone else.

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

## BE_JSON_ArraySize

```
BE_JSON_ArraySize ( json { ; path } )
```

**Description**

Will return the number of values in a JSON array.

**Parameters**

* *json* : the array to count.
* *name* ( optional ) : the path within the JSON to locate the array to count.

**Version History**

* 2.1.0 : First Release
* 4.1.0 : Added the optional path parameter

**Notes**

If *path* is not included, it counts the array at the root of the JSON, and will return 1 for a non array type.

If *path* is included, the format is as per the *BE_JSONPath* function, which is NOT the same as the internal FileMaker JSON functions introduced in version 16.

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

## BE_JSON_jq

```
BE_JSON_jq ( json ; filter { ; options } )
```

**Description**

jq is a lightweight and flexible command-line JSON processor akin to sed,awk,grep, and friends for JSON data. It allows you to easily slice, filter, map, and transform structured data.

**Parameters**

* *json* : the input to the jq function.
* *filter* : the filter(s) to run on the input.
* *options* : one or more of the following characters : V c j r - see notes.

**Version History**

* 5.0.0 : First Release

**Notes**

More info is here : https://jqlang.github.io/jq/

And a manual with all the details is here : https://jqlang.github.io/jq/manual/

For testing your filters use : https://jqplay.org

The option values are : 

c - compact - By default, jq pretty-prints JSON output. Using this option will result in more compact output by instead putting each JSON object on a single line.

r - raw output ( ie not quoted text strings ) - With this option, if the filter's result is a string then it will be written directly to standard output rather than being formatted as a JSON string with quotes. This can be useful for making jq filters talk to non-JSON-based systems.

j - join output : Like -r but jq won't print a newline after each output.

V - version info.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes |  
| Win FMP | No |  
| FMS | Yes |  
| iOS | Yes |  
| Linux | Yes |  

**Example Code**

Get all the names of product types : 

    BE_JSON_jq ( $json ; ".productTypes[].name" )

In an array of prices select the ones where quantity is 1, then sort by price, and find the largest, and return that price :

    BE_JSON_jq ( $json ; "[ .prices[] | select(.quantity == 1 ) ] | max_by(.price) | .price" ; "r" )

Find all possible values of a node, deep in a json object : 

    BE_JSON_jq ( $json ; ".data.products.edges[].node.category.name" ; "r" )
