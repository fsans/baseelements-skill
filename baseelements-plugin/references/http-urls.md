# HTTP and URLs — BaseElements Plugin

## BE_HTTP_GET

```
BE_HTTP_GET ( url ; { fileName ; username ; password } )
```

**Description**

Does a http, https, ftp, ftps, or sftp GET / download and returns the results.  This uses the same curl library as the built in *Insert From URL*.

**Parameters**

* *url* : The url to retrieve.
* *fileName* ( optional ) : The filename to use when sending binary data.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 1.2.0 : First Release
* 3.3.0 : Renamed to BE_HTTP_GET from BE_GetURL

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

---

## BE_HTTP_GETFile

```
BE_HTTP_GETFile ( url ; { path ; username ; password } )
```

**Description**

Does a http, https, ftp, ftps, or sftp GET / download and saves the result to *path*.  This uses the same curl library as the built in *Insert From URL*.

**Parameters**

* *url* : The url to retrieve.
* *path* ( optional ) : The path to save the file to.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 1.2.0 : First Release
* 3.3.0 : Renamed to BE_HTTP_GET_File from BE_SaveURLToFile

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

---

## BE_HTTP_POST

```
BE_HTTP_POST ( url ; parameters ; { username ; password ; fileName } )
```

**Description**

Does a HTTP POST call at the *url* and returns the response if any. This uses the same curl library as the built in *Insert From URL* using "-X POST" in the curl options.

**Parameters**

* *url* : The url to use for the POST action.
* *parameters* : The data to send to the url.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.
* *fileName* ( optional ) : The filename to send when sending binary data.

**Version History**

* 1.3.0 : First Release
* 2.0.0 : Added optional username and password parameters
* 3.1.0 : Allowed the use of file=@path for file parameters
* 4.0.0 : Added the optional filename parameter

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

When doing basic url parameter values, *parameters* is a text string, which is a list of all of the parameters to send in name value pair format. Put an "=" between the name and value, and an ampersand "&" between each pair. URLEncode the names and values.

Otherwise the type of data being sent is usually determined via a Content_Type header you set via *BE_HTTP_SetCustomHeader*. For example, JSON data : 

	BE_HTTP_POST ( "http://Fictional.Server.com/service.js" ;
	JSONSetElement ( "{}" ; "method" ; "Workgroup.projects.listActive" ; JSONString ) ;
	"Administrator" ; "password123" )

To send a file from disk, use file=@path for external files.  The path is a plugin path, not a FileMaker path.

	BE_HTTP_POST ( $url ; "image=@/Users/nick/Desktop/test.jpg" )

The server must support the sending of files.

You can also do multipart/form data :

	BE_HTTP_SetCustomHeader ( "Content-type"; "multipart/mixed" )
	
	BE_HTTP_POST ( $url ; "a=b&c=d&image=@" & BE_ExportFieldContents ( Table::File ) ; "Administrator" ; "password123" ; GetContainerAttribute ( Table::File ; "filename" ) )

---

## BE_HTTP_PATCH

```
BE_HTTP_PATCH ( url ; parameters ; { username ; password } )
```

**Description**

Does a HTTP PATCH call at the *url* and returns the response if any. This uses the same curl library as the built in *Insert From URL* using "-X PATCH" in the curl options.

**Parameters**

* *url* : The url to use for the PATCH action.
* *parameters* : The data to send to the url.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 3.3.0 : First Release

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

When doing basic url parameter values, *parameters* is a text string, which is a list of all of the parameters to send in name value pair format. Put an "=" between the name and value, and an ampersand "&" between each pair. URLEncode the names and values.

Otherwise the type of data being sent is usually determined via a Content_Type header you set via *BE_HTTP_SetCustomHeader*. For example, JSON data : 

	BE_HTTP_PATCH ( "http://Fictional.Server.com/service.js" ;
	JSONSetElement ( "{}" ; "method" ; "Workgroup.projects.listActive" ; JSONString ) ;
	"Administrator" ; "password123" )

To send a file from disk, use file=@path for external files.  The path is a plugin path, not a FileMaker path.

	BE_HTTP_PATCH( $url ; "image=@/Users/nick/Desktop/test.jpg" )

The server must support the sending of files.

---

## BE_HTTP_PUTData

```
BE_HTTP_PUTData ( url ; data ; { username ; password } )
```

**Description**

Does a HTTP PUT at the *url* and returns the response if any. This uses the same curl library as the built in *Insert From URL* using "-X PUT" in the curl options.

**Parameters**

* *url* : The url to use for the PUT action.
* *data* : The data to send to the url.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 2.1.0 : First Release
* 4.0.2 : Renamed from BE_HTTP_PUT_DATA

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

When doing basic url parameter values, *parameters* is a text string, which is a list of all of the parameters to send in name value pair format. Put an "=" between the name and value, and an ampersand "&" between each pair. URLEncode the names and values.

Otherwise the type of data being sent is usually determined via a Content_Type header you set via *BE_HTTP_SetCustomHeader*. For example, JSON data : 

	BE_HTTP_PUTData ( "http://Fictional.Server.com/service.js" ;
	JSONSetElement ( "{}" ; "method" ; "Workgroup.projects.listActive" ; JSONString ) ;
	"Administrator" ; "password123" )

To send a file from disk, use file=@path for external files.  The path is a plugin path, not a FileMaker path.

	BE_HTTP_PUTData ( $url ; "image=@/Users/nick/Desktop/test.jpg" )

The server must support the sending of files.

---

## BE_HTTP_PUTFile

```
BE_HTTP_PUTFile ( url ; path ; { username ; password } )
```

**Description**

Does a HTTP PUT at the *url* and returns the response if any. This uses the same curl library as the built in *Insert From URL* using "-X PUT" in the curl options.

**Parameters**

* *url* : The url to use for the PUT action.
* *path* : The path to read the file to send to the url.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 2.0.0 : First Release
* 2.1.0 : Changed the name to BE_HTTP_PUT_DATA to reflect the actual use of the function parameters
* 4.0.2 : Renamed from BE_HTTP_PUTData

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

The type of data being sent is usually determined via a Content_Type header you set via *BE_HTTP_SetCustomHeader*. Each file type will have it's own Content-Type and the web service will determine acceptable types.  

	BE_HTTP_PUTData ( "http://Fictional.Server.com/service.js" ;
	"/path/to/file.txt" ;
	"Administrator" ; "password123" )

---

## BE_HTTP_DELETE

```
BE_HTTP_DELETE ( url ; { username ; password } )
```

**Description**

Does a HTTP DELETE call at the *url* and returns the response if any. This uses the same curl library as the built in *Insert From URL* using "-X DELETE" in the curl options.

**Parameters**

* *url* : The url to use for the DELETE action.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

**Version History**

* 2.0.0 : First Release

**Notes**

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Not all servers respond with data when doing curl operations so use *BE_GetLastError* after the function call to tell if the call was able to be made, and then *BE_HTTP_ResponseCode* to check for the appropriate response code, and *BE_HTTP_ResponseHeaders* to get the response headers to diagnose any issues.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

---

## BE_HTTP_SetCustomHeader

```
BE_HTTP_SetCustomHeader ( { header ; value } )
```

**Description**

Used for setting a header value before a HTTP, FTP, or SMTP function call. You can call this function multiple times before the POST to set more than one header, and call it with empty parameters to clear them out.

**Parameters**

* *header* ( optional ) : the name of the header to set.
* *value* ( optional ) : the value to set it to.

**Version History**

* 1.3.0 : First Release
* 4.0.1 : Added option to call with no parameters to delete all Headers
* 4.0.2 : Renamed from BE_HTTP_Set_Custom_Header
* 4.1.3 : **Breaking Change** : Change the way empty string headers vs missing value parameters are used - see notes.

**Notes**

In versions from 4.1.3 or later, setting a header with 

	BE_HTTP_SetCustomHeader ( "name" ; "" ) 

now sets the header to an empty header value, instead of removing that header. Some header options have a default value and there is an accepted header with the name, but an empty string as the value.  To remove the header completely and revert to default call 

	BE_HTTP_SetCustomHeader ( "name" ) 

with no value parameter, which is also the same behaviour as the previous versions.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

**Example Code**

	BE_HTTP_SetCustomHeader ( "Content-Type" ; "text/xml;charset=utf-8" ) 

---

## BE_HTTP_ResponseCode

```
BE_HTTP_ResponseCode
```

**Description**

Returns the value of the Response code from the previous call made via any of the BE curl functions. Eg 403 for not found.

**Parameters**

None

**Version History**

* 1.3.0 : First Release
* 4.0.2 : Renamed from BE_HTTP_Response_Code

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

---

## BE_HTTP_ResponseHeaders

```
BE_HTTP_ResponseHeaders ( { header } )
```

**Description**

Returns the headers set by the server as part of a response to the previous HTTP request made via any of the BE curl functions.

**Parameters**

* *header* ( optional ) : The header name to retrieve or all headers if blank or omitted.

**Version History**

* 1.3.0 : First Release
* 3.3.2 : Added the *header* parameter so that the named header only is returned
* 4.0.2 : Renamed BE_HTTP_Response_Code to BE_HTTP_ResponseCode

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

---

## BE_HTTP_SetProxy

```
BE_HTTP_SetProxy ( proxy { ; port ; userName ; password } )
```

**Description**

Used for setting the HTTP proxy values values before other curl based function calls.

**Parameters**

* *proxy* : The proxy url.
* *port* ( optional, default:80 ) : port number to use.
* *userName* ( optional ) : username for basic HTTP authentication.
* *password* ( optional ) : password for basic HTTP authentication.

**Version History**

* 2.0.0 : First Release
* 4.0.2 : Renamed from BE_HTTP_Set_Proxy

**Notes**

There is no standard way to retrieve the built in OS proxy settings, if they've been applied, so this can only be set manually via this function.

Proxies are used in the HTTP, FTP, and SMTP functions despite the HTTP in the name of this function.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation : 

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

---

## BE_CurlSetOption

```
BE_CurlSetOption ( { option ; value } )
```

**Description**

Sets one of the Curl library optional variables to be used in all subsequent calls to the HTTP/FTP/SMTP functions.

The *value* depends on the type of *option*. Some require explicit values ( such as CURLOPT_USERAGENT ) and others require a boolean. 

The full list of options and their possible values is in the curl documentation : [http://curl.haxx.se/libcurl/c/curl_easy_setopt.html](http://curl.haxx.se/libcurl/c/curl_easy_setopt.html) 

**Parameters**

* *option* ( optional ) : The option name as text from the list below.
* *value* ( optional ) : The value to pass to the option.

* When the *value* parameter is left out then the *option* is cleared.
* When the *value* parameter is empty and the *option* supports an empty value the option will be set to the empty string.
* When no parameters are included or *option* is empty, then all values are cleared out and set back to defaults.

**Version History**

* 2.1.0 : First Release.
* 3.1.0 : Made the *option* value optional and added named constants.
* 4.0.2 : Renamed from BE_Curl_Set_Option.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

**Example Code**

Restoring all values to their default can be done with :

	BE_CurlSetOption

To return an option to it's default call the option name with no parameter :

	BE_CurlSetOption ( "CURLOPT_HTTPAUTH" )

The *CURLOPT_HTTPAUTH* and *CURLOPT_PROXYAUTH* options can also be set with these constants :

	CURLAUTH_BASIC
	CURLAUTH_DIGEST
	CURLAUTH_DIGEST_IE
	CURLAUTH_NEGOTIATE
	CURLAUTH_NTLM
	CURLAUTH_NTLM_WB
	CURLAUTH_ANY
	CURLAUTH_ANYSAFE
	CURLAUTH_ONLY

**Common use cases**

Use "Basic" Authentication

	BE_CurlSetOption ( "CURLOPT_HTTPAUTH" ; 1 ) 
	BE_CurlSetOption ( "CURLOPT_HTTPAUTH" ; "CURLAUTH_BASIC" )

**Certificate based Authentication**

To use Certificate based Authentication for HTTP function calls you would do :

	BE_CurlSetOption ( "CURLOPT_SSLCERT" ; "/path/to/cert.pem" )
	BE_CurlSetOption ( "CURLOPT_SSLCERTTYPE" ; "PEM" )
	BE_CurlSetOption ( "CURLOPT_SSLKEY" ; "/path/to/key.pem" )
	BE_CurlSetOption ( "CURLOPT_SSLKEYTYPE" ; "PEM" )

With certificates, "PEM" is the default type, so can be left out. Other possible options for the types are 

	PEM
	DER
	P12
	
P12 is for PKCS#12-encoded files. PEM files that will work with curl doing SFTP are **RSA PRIVATE KEY** files. You can open them in a text editor and you should see that the first line of the text has **RSA PRIVATE KEY** at the beginning. If it says **OPENSSH PRIVATE KEY** instead, it may not work. You can convert keys from one type to another using these instructions : 

[https://federicofr.wordpress.com/2019/01/02/how-to-convert-openssh-private-keys-to-rsa-pem/](https://federicofr.wordpress.com/2019/01/02/how-to-convert-openssh-private-keys-to-rsa-pem/) 

**Using Cookies**

To allow curl to store and re-use cookies, set the cookie jar ( for where to store them ) and the cookie file ( for where to read them to send ).

	BE_CurlSetOption ( "CURLOPT_COOKIEFILE" ; "/path/to/cookieFile.txt" )
	BE_CurlSetOption ( "CURLOPT_COOKIEJAR" ; "/path/to/cookieFile.txt" )

You can use the temp folder for storing the cookie files, but make sure you're using a plugin path and not a FileMaker path created from any of the *Get* function calls.

**Amazon Authentication**

There's now a native option for the complex AWS authentication method using the **CURLOPT_AWS_SIGV4** option. It appears to require a string matching either "provider1:provider2" or "provider1:provider2:region:service" and you set the keys via the option values *CURLOPT_USERPWD* and "MY_ACCESS_KEY:MY_SECRET_KEY". See here for more detail :

[https://curl.se/libcurl/c/CURLOPT_AWS_SIGV4.html](https://curl.se/libcurl/c/CURLOPT_AWS_SIGV4.html) 

Complete List of supported CURL options :

	CURLOPT_PROXY
	CURLOPT_NOPROXY
	CURLOPT_SOCKS5_GSSAPI_SERVICE
	CURLOPT_INTERFACE
	CURLOPT_NETRC_FILE
	CURLOPT_USERPWD
	CURLOPT_PROXYUSERPWD
	CURLOPT_USERNAME
	CURLOPT_PASSWORD
	CURLOPT_PROXYUSERNAME
	CURLOPT_PROXYPASSWORD
	CURLOPT_TLSAUTH_USERNAME
	CURLOPT_TLSAUTH_PASSWORD
	CURLOPT_ACCEPT_ENCODING
	CURLOPT_COPYPOSTFIELDS
	CURLOPT_REFERER
	CURLOPT_USERAGENT
	CURLOPT_COOKIE
	CURLOPT_COOKIEFILE
	CURLOPT_COOKIEJAR
	CURLOPT_COOKIELIST
	CURLOPT_HTTPGET
	CURLOPT_MAIL_FROM
	CURLOPT_MAIL_AUTH
	CURLOPT_FTPPORT
	CURLOPT_FTP_ALTERNATIVE_TO_USER
	CURLOPT_FTP_ACCOUNT
	CURLOPT_RTSP_SESSION_ID
	CURLOPT_RTSP_STREAM_URI
	CURLOPT_RTSP_TRANSPORT
	CURLOPT_RANGE
	CURLOPT_CUSTOMREQUEST
	CURLOPT_DNS_SERVERS
	CURLOPT_SSLCERT
	CURLOPT_SSLCERTTYPE
	CURLOPT_SSLKEY
	CURLOPT_SSLKEYTYPE
	CURLOPT_KEYPASSWD
	CURLOPT_SSLENGINE
	CURLOPT_CAINFO
	CURLOPT_ISSUERCERT
	CURLOPT_CAPATH
	CURLOPT_CRLFILE
	CURLOPT_RANDOM_FILE
	CURLOPT_EGDSOCKET
	CURLOPT_SSL_CIPHER_LIST
	CURLOPT_KRBLEVEL
	CURLOPT_SSH_HOST_PUBLIC_KEY_MD5
	CURLOPT_SSH_PUBLIC_KEYFILE
	CURLOPT_SSH_PRIVATE_KEYFILE
	CURLOPT_SSH_KNOWNHOSTS
	CURLOPT_VERBOSE
	CURLOPT_HEADER
	CURLOPT_NOSIGNAL
	CURLOPT_WILDCARDMATCH
	CURLOPT_FAILONERROR
	CURLOPT_PROXYPORT
	CURLOPT_PROXYTYPE
	CURLOPT_HTTPPROXYTUNNEL
	CURLOPT_SOCKS5_GSSAPI_NEC
	CURLOPT_LOCALPORT
	CURLOPT_LOCALPORTRANGE
	CURLOPT_DNS_CACHE_TIMEOUT // CURLOPT_DNS_USE_GLOBAL_CACHE - removed in 4.2.0
	CURLOPT_BUFFERSIZE
	CURLOPT_PORT
	CURLOPT_TCP_NODELAY
	CURLOPT_ADDRESS_SCOPE
	CURLOPT_TCP_KEEPALIVE
	CURLOPT_TCP_KEEPIDLE
	CURLOPT_TCP_KEEPINTVL
	CURLOPT_NETRC
	CURLOPT_HTTPAUTH
	CURLOPT_TLSAUTH_TYPE
	CURLOPT_PROXYAUTH
	CURLOPT_AUTOREFERER
	CURLOPT_TRANSFER_ENCODING
	CURLOPT_FOLLOWLOCATION
	CURLOPT_UNRESTRICTED_AUTH
	CURLOPT_MAXREDIRS
	CURLOPT_PUT
	CURLOPT_POST
	CURLOPT_POSTFIELDSIZE
	CURLOPT_COOKIESESSION
	CURLOPT_HTTP_VERSION
	CURLOPT_IGNORE_CONTENT_LENGTH
	CURLOPT_HTTP_CONTENT_DECODING
	CURLOPT_HTTP_TRANSFER_DECODING
	CURLOPT_TFTP_BLKSIZE
	CURLOPT_FTPPORT
	CURLOPT_DIRLISTONLY
	CURLOPT_APPEND
	CURLOPT_FTP_USE_EPRT
	CURLOPT_FTP_USE_EPSV
	CURLOPT_FTP_USE_PRET
	CURLOPT_FTP_CREATE_MISSING_DIRS
	CURLOPT_FTP_RESPONSE_TIMEOUT
	CURLOPT_FTP_SKIP_PASV_IP
	CURLOPT_FTPSSLAUTH
	CURLOPT_FTP_SSL_CCC
	CURLOPT_FTP_FILEMETHOD
	CURLOPT_RTSP_REQUEST
	CURLOPT_RTSP_CLIENT_CSEQ
	CURLOPT_RTSP_SERVER_CSEQ
	CURLOPT_TRANSFERTEXT
	CURLOPT_PROXY_TRANSFER_MODE
	CURLOPT_CRLF
	CURLOPT_RESUME_FROM
	CURLOPT_FILETIME
	CURLOPT_NOBODY
	CURLOPT_INFILESIZE
	CURLOPT_UPLOAD
	CURLOPT_MAXFILESIZE
	CURLOPT_TIMECONDITION
	CURLOPT_TIMEVALUE
	CURLOPT_TIMEOUT
	CURLOPT_TIMEOUT_MS
	CURLOPT_LOW_SPEED_TIME
	CURLOPT_MAXCONNECTS
	CURLOPT_FRESH_CONNECT
	CURLOPT_FORBID_REUSE
	CURLOPT_CONNECTTIMEOUT
	CURLOPT_CONNECTTIMEOUT_MS
	CURLOPT_IPRESOLVE
	CURLOPT_CONNECT_ONLY
	CURLOPT_USE_SSL
	CURLOPT_ACCEPTTIMEOUT_MS
	CURLOPT_SSLENGINE_DEFAULT
	CURLOPT_SSLVERSION
	CURLOPT_SSL_VERIFYPEER
	CURLOPT_SSL_VERIFYHOST
	CURLOPT_CERTINFO
	CURLOPT_SSL_SESSIONID_CACHE
	CURLOPT_GSSAPI_DELEGATION
	CURLOPT_SSH_AUTH_TYPES
	CURLOPT_NEW_FILE_PERMS
	CURLOPT_NEW_DIRECTORY_PERMS
	CURLOPT_POSTFIELDSIZE_LARGE
	CURLOPT_RESUME_FROM_LARGE
	CURLOPT_INFILESIZE_LARGE
	CURLOPT_MAXFILESIZE_LARGE
	CURLOPT_MAX_SEND_SPEED_LARGE
	CURLOPT_MAX_RECV_SPEED_LARGE

Added in 4.2.0 :

	CURLOPT_CONNECT_TO
	CURLOPT_TCP_FASTOPEN
	CURLOPT_KEEP_SENDING_ON_ERROR
	CURLOPT_SUPPRESS_CONNECT_HEADERS
	CURLOPT_SOCKS5_AUTH
	CURLOPT_REQUEST_TARGET
	CURLOPT_SSH_COMPRESSION
	CURLOPT_HAPPY_EYEBALLS_TIMEOUT_MS
	CURLOPT_DNS_SHUFFLE_ADDRESSES
	CURLOPT_HAPROXYPROTOCOL
	CURLOPT_DISALLOW_USERNAME_IN_URL
	CURLOPT_TLS13_CIPHERS
	CURLOPT_PROXY_TLS13_CIPHERS
	CURLOPT_UPLOAD_BUFFERSIZE
	CURLOPT_DOH_URL
	CURLOPT_MAXAGE_CONN
	CURLOPT_MAIL_RCPT_ALLLOWFAILS
	CURLOPT_PROXY_ISSUERCERT
	CURLOPT_AWS_SIGV4

---

## BE_OpenURL

```
BE_OpenURL ( url )
```

**Description**

Sends the url to the users default web browser.

**Parameters**

* *url* : the url to open.

**Version History**

* 1.1.0 : URL Open
* 4.0.0 : return error 3, command not available on Linux

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | Yes  |  
| Linux | No  |  

---

## BE_CurlGetInfo

```
BE_CurlGetInfo ( getInfoOption )
```

**Description**

This function returns a value for the most recent curl operation ( HTTP, FTP, SMTP ) using the list of options that are supported by the BE_CurlSetOption function, and documented here :

https://curl.se/libcurl/c/curl_easy_getinfo.html

**Parameters**

* *getInfoOption* : Any of the values as a text string that are documented as working in the BE_CurlSetOption function.

**Version History**

* 5.0.0 : First Release

**Notes**

Not all options are supported in the plugin vs what is on the curl website, but the website documents them well to understand their usage.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  

**Example Code**

    BE_CurlSetOption ( "CURLOPT_CERTINFO" ; 1 ) & 
    BE_HTTP_GET ( "https://goya.com.au" ) & 
    BE_CurlGetInfo ( "CURLINFO_CERTINFO" )
