# Files and Folders — BaseElements Plugin

## BE_FileExists

```
BE_FileExists ( path )
```

**Description**
Checks if the file or folder given in the *path* exists and returns True or False.  For folders it accepts the name of the folder ( and reports existence correctly ) with or without a trailing /.

**Parameters**
* *path* : a plugin file path.

**Version History**
* 1.0.0 : First Release

**Notes**
PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FileReadText

```
BE_FileReadText ( pathOrContainer { ; start ; to ; eolChar } )
```

**Description**
Reads the contents of the file at *pathOrContainer* as text and returns the content, optionall beginning at point *start*, ending at *to* and treating *eolChar* as line characters.

**Parameters**
* *pathOrContainer* : A plugin path to the file to read, or a container field.
* *start* ( optional ) : The character position to start reading from. The first character is at position 0. Pass empty or a value greater than *to* to read from the end of the file.
* *to* ( optional ) : The character position to stop reading at.
* *eolChar* ( optional ) : Pass empty to read characters. Otherwise used to extract a portion of the file.

Use *start*, *to* and *eolChar* to extract a portion of the file - the first character is at position 0 which is different from the native Position function.

Pass *start* as empty or greater than *to* to read from the end of the file in reverse back to *to* - useful for getting the tail of log files for example.

Pass an empty *eolChar* to use start and to as pure character counts.

If you include a value for *eolChar* it will treat *eolChar* as a line break character, and will return lines of text instead of characters of the text.  So *start* as the first line to start at, and *to* the last line.

**Version History**
* 1.0.0 : First Release
* 4.0.0 : Allow reading text from a container file instead of a path
* 4.0.2 : Renamed from BE_ReadTextFromFile
* 4.2.0 : added start, to and eolChar parameters
* 5.0.0 : Fixed a bug in the way that the function works on container fields to match the external files behaviour
* 5.0.0 : Fixed a bug in the eolChar to allow multiple character eol values

**Notes**
PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

Note that *eolChar* doesn't have to be a normal end of line character such as a line feed or carriage return. You can use any character you want. This also means that text brought into FileMaker may not have multiple lines in the sense of what a FileMaker value list has. Or another way : the plugin doesn't modify the text to replace your *eolChar* with an end of line character.

So you could use "," as the *eolChar* to get certain columns out of a csv file.

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

	BE_FileReadText ( $path )

reads the entire file

	BE_FileReadText ( $path ; 1 ; 100 )

reads the first 100 characters

	BE_FileReadText ( $path ; 1 ; 10 ; char ( 10 ) )

reads the first 10 lines of the file

	BE_FileReadText ( $path ; 10 ;  ; char ( 10 ) )

reads from line 10 to the end of the file

	BE_FileReadText ( $path ;  ; 10 ; char ( 10 ) )

reads the last 10 lines of the file.

---

## BE_FileWriteText

```
BE_FileWriteText ( pathOrContainer ; text ; { appendBoolean } )
```

**Description**
Writes the contents specified in *text* to the file at the *pathOrContainer* or to a container field via Set Field.

**Parameters**
* *pathOrContainer* : either text which contains a path to a file, or binary data from a container field, or file set into a variable.
* *text* : the text to write to the file.
* *appendBoolean* ( optional, default:False ) : True will append to the file, False or no parameter will either write over an existing file or set the contents of a new file.

Using the optional *appendBoolean* parameter you can choose to either write a new file ( or overwrite an existing one ), or to append the text to the end of the file.

**Version History**
* 1.0.0 : First Release
* 1.1.0 : Adds appendBoolean option
* 3.3.0 : Recursively create any directories needed
* 4.0.2 : Renamed from BE_WriteTextToFile
* 4.0.2 : Allows writing to container fields as well as paths

**Notes**
Defaults to UTF-8 ( no BOM ) which can be changed using the BE_SetTextEncoding function.

For writing files to containers, this function works a little differently. First you need to use this as a Set Field step, and the resulting output will then be set into the field that is the destination of the Set Field step.

If you're appending to a file, and setting it into a field, the first parameter pathOrContainer can be either a field with an existing file, or a path to an existing file. The result will then be appended to the original. Set the appendBoolean parameter to True.

If you're setting a field and not appending, the first parameter should contain just the name of the file you want as text.

Obviously this first parameter can be many things, a path, a container field containing a file, or a text string ( from a field, or variable etc ) containing a file name.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

	BE_FileWriteText ( $path ; "sometexthere" )

creates a new file at $path with the content "sometexthere"

	BE_FileWriteText ( $path ; "sometexthere" ; True )

appends to an existing file at $path and adds "sometexthere" to the end of the document

	SetField [ table::myField ; BE_FileWriteText ( $path ; "sometexthere" ; True )

sets myField with a file, that is the content of the existing file at $path with "sometexthere" appended. Filename will be as per the existing file at $path.

	SetField [ table::myField ; BE_FileWriteText ( "myfile.txt" ; "sometexthere" )

Creates a new file called "myfile.txt" and sets the content to "sometexthere" and puts it into the myField field.

---

## BE_FileCopy

```
BE_FileCopy ( fromFilePath ; toFilePath {; replaceDestinationFile } )
```

**Description**
Copies the file specified in the *fromFilePath* path parameter, to the location in the *toFilePath* path parameter. Both paths are full paths not folders, including the output filename.

**Parameters**
* *fromFilePath* : a plugin file path to copy from.
* *toFilePath* : a plugin file path destination, including filename.
* *replaceDestinationFile* : whether to overwrite an existing file.

**Version History**
* 1.1.0 : First Release
* 1.2.0 : Added support for copying directories
* 4.0.2 : Renamed from BE_CopyFiles

**Notes**
This function does not behave the same as /bin/cp.

Important to note that the 'to' path needs to end with the desired name of the file.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

	BE_FileCopy ( "/Users/username/Desktop/fileA.fp7" ; "/Users/username/path/to/file/fileA_copy.fp7" )

---

## BE_FileMove

```
BE_FileMove ( fromFilePath ; toFilePath {; replaceDestinationFile } )
```

**Description**
Moves the file specified in the *fromFilePath* parameter, to the location in the *toFilePath* parameter optionally replacing according to *replaceDestinationFile*.

**Parameters**
* *fromFilePath* : a plugin path to a file.
* *toFilePath* : a plugin path destination.
* *replaceDestinationFile* ( optional, default:False ) : boolean to replace the file or not.

**Version History**
* 1.1.0 : First Release
* 4.0.2 : Renamed from BE_MoveFile

**Notes**
On Mac OS X, the Move operation only works if the source and Destination are on the same volume. To move files across volumes, use a Copy and then Delete the original.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FileDelete

```
BE_FileDelete ( path )
```

**Description**
Deletes the file at location path.

**Parameters**
* *path* : a plugin file path.

**Version History**
* 1.0.0 : First Release
* 4.0.2 : Renamed from BE_DeleteFile

**Notes**
This function doesn't send files to the trash or recycle bin, so use with caution. Files deleted are gone.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

	BE_FileDelete ( "/Users/username/Desktop/myNewFolder" )

deletes a folder and all of it's contents

	BE_FileDelete ( "/Users/username/Desktop/fileA.fp7" )

deletes a file

---

## BE_FileImport

```
BE_FileImport ( filePath ; { compressBoolean } )
```

**Description**
Imports the contents of the file at *filePath* into a container field or variable, and optionally compresses the field using the built in gzip compression. This provides a similar functionality to the Insert File script step, but the plugin function is also available via FileMaker Server whereas the step doesn't work on server.

**Parameters**
* *filePath* : The path to locate the file.
* *compressBoolean* ( optional, default:True ) : True for compression or False for uncompressed.

**Version History**
* 3.0.0 : First Release
* 4.0.2 : Renamed from BE_ImportFile

**Notes**
Also note that you can convert container types within a container via the *BE_ConvertContainer* function, so you can convert the resulting file to an image if required.

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

	Set Field [ Imports::Image; BE_FileImport ( $file ) ]

---

## BE_FileListFolder

```
BE_FileListFolder ( path { ; type ; includeSubdirBoolean ; useFullPathBoolean ; includeHiddenBoolean } )
```

**Description**
Lists the contents of a folder at the *path*, both files and folders by default or use a *type* from *BE_FileTypeAll*, *BE_FileTypeFile*, or *BE_FileTypeFolder*.

**Parameters**
* *path* : A plugin path to the folder to list.
* *type* : One of `BE_FileTypeAll`, `BE_FileTypeFile`, or `BE_FileTypeFolder` to filter the results by entry type.
* *includeSubdirBoolean* ( optional, default:False ) : whether to scan sub directories.
* *useFullPathBoolean* ( optional, default:False ) : whether to include the full path in the output.
* *includeHiddenBoolean* ( optional, default:False ) : whether to include files or folder not normally visible.

With the *includeSubdirectories* parameter set to true, it will recursively go into every sub folder and return all the results it finds. Be aware that this could go on for a long time, and is not recommended.

With *useFullPath* set to True it will change the output to include full paths instead of just filenames.

**Version History**
* 1.1.0 : First Release
* 1.2.0 : Added the optional type parameter
* 2.3.0 : Added the optional includeSubdirectories and useFullPath parameters
* 4.0.0 : Added the optional includeHidden parameter
* 4.1.3 : Correctly handle file names containing Unicode Characters on Windows

**Notes**
The includeSubdirectories option means that it will try every single subfolder. Be cautious when using this as it may take a long time to traverse all the sub folders.

Also it is more than likely that at some point it will throw an error as it will come across a folder or file it doesn't have access to. Then the function will stop and return error 13, and no data. Managing individual errors like that amongst a potentially large set of files is beyond the scope of this function as implemented.

If you're getting error 13 when using this flag, consider doing without it and traversing the sub folders via script or recursion and ignoring the access error codes instead.

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

	BE_FileListFolder ( BE_FolderSelectDialog ( "" ) )

	BE_FileListFolder ( "/Users/nick/Desktop" )

	BE_FileListFolder ( $path ; BE_FileTypeFolder ; False ; True ; True )

This last one will start at $path, but only return folders, and will not include subdirectories, will return a full path not just the folder names, and will include any hidden folders. :

	BE_FileListFolder ( $path ; BE_FileTypeFolder ;
	False ; //don't scan sub folders
	True ; //include a full path
	True ) //include hidden files

---

## BE_FileModificationTimestamp

```
BE_FileModificationTimestamp ( path )
```

**Description**
Returns the OS file modification time of the file at *path*.

**Parameters**
* *path* : a system file path.

**Version History**
* 3.1.2 : First Release
* 4.0.2 : Renamed from BE_File_Modification_Timestamp

**Notes**
It is up to the Operating system as to what time it returns, and some OS versions may be more precise ( down to the millisecond ) and so the result could be different in across OS versions, or change in future OS versions.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FileOpen

```
BE_FileOpen ( path )
```

**Description**
Opens the file or folder at *path* using the default application chosen by the operating system.

**Parameters**
* *path* : the plugin path to the file.

**Version History**
* 1.3.0 : First Release
* 4.0.0 : return error 3, command not available on iOS and Linux
* 4.0.2 : Renamed from BE_OpenFile

**Notes**
There are some quirks to the way this function works on Mac OS : it sends an open request to the file, which is either able to be sent or not, and the response/error only reflects whether or not the request was sent. So for example if the file doesn't exist, or if you don't have permissions to open it, you get an error.

But whether the file actually opens or not is another thing, the OS will check things like apps for security and that can take some time. So whether or not the open is successful is different from whether or not the plugin tried to open the file. The two can be different, and the plugin only handles the sending of the open request, it doesn't poll to figure out if the open was successful or not.

In technical terms, the two are asynchronous - meaning we get back the first "done and ok" response and would have to continuously query the OS to find out the final state. The plugin doesn't do that. You'd need to use some other function to work out if the open did what you were expecting.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | No |
| Linux | No |

---

## BE_FilePatternCount

```
BE_FilePatternCount ( path ; searchText )
```

**Description**
Search inside files on disk. Much like the native *PatternCount* function, but within files on disk.

**Parameters**
* *path* : the plugin path to the file.
* *searchText* : the text to find in the text document - can be a value list, where it will count each of the values in turn.

**Version History**
* 4.2.0 : First Release

**Notes**
PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FileReplaceText

```
BE_FileReplaceText ( pathOrContainer ; expression ; replaceString { ; options } )
```

**Description**
Much like the native Substitute function, but for a text file on disk or stored inside a container field.

**Parameters**
* *pathOrContainer* : either a full path to the file on disk, or a container field.
* *expression* : The text string to look for in the file.
* *replaceString* : The text string to replace it with in the file.
* *options* ( optional, defaut:gi ) : A string of characters from the options below , eg "igm".

If the *pathOrContainer* is a container field which contains only text, then it will be read as a path, and that path will be used to locate the file.

**Version History**
* 4.2.0 : First Release

**Notes**
Options :
* i - case insensitive
* m - multiline
* s - dot matches all characters, including newline<
* x - ignore whitespace
* g - replace all

For options: Default is "gi" which matches the native FileMaker substitute. These options work exactly the same as the RegularExpression function.

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FileSize

```
BE_FileSize ( path )
```

**Description**
Returns the number of bytes in the file at *path*.

**Parameters**
* *path* : the plguin path to the file..

**Version History**
* 2.0.0 : First Release

**Notes**
PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

## BE_FolderCreate

```
BE_FolderCreate ( path )
```

**Description**
Creates a new folder at the location *path*, creating sub folders along the way as required.

**Parameters**
* *path* : A plugin file path ( not a FileMaker path )..

**Version History**
* 1.0.0 : First Release
* 2.1.0 : changed the folder to be recursive, so any required subfolders are also created
* 4.0.2 : Renamed from BE_CreateFolder

**Notes**
PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

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

	BE_FolderCreate ( "/Users/username/Desktop/myNewFolder" )

---

## BE_FTP_Delete

```
BE_FTP_Delete ( url ; { username ; password } )
```

**Description**
Deletes a file at a specific ftp or sftp url.

**Parameters**
* *url* : The url to connect to the server and the path to locate the file to delete.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

The url is both the server connection detail as :

	protocol://server:port/

and the path the file to delete, eg :

	protocol://server:port/path/to/file/to/delete.txt

See the notes for more info about paths.

**Version History**
* 3.2.0 : First Release

**Notes about paths**

Our limited testing seems to be that when using the *BE_FTP_Delete* function, the path is a relative path, not an absolute path.  So when you login to the server, you often get put into a "home" folder, such that the current path at login time is :

	/home/nick

So to delete a file at the path /home/nick/folder/file.txt you will need to use the url as :

	sftp://example.com:2000/folder/file.txt

and leave out the current folder.  This is the opposite to the *BE_FTP_Upload* and *BE_FTP_UploadFile* functions where the path is absolute.  With an upload of the same file, you would use :

	sftp://example.com:2000/home/nick/folder/file.txt

To upload the same file as the delete example.

The simple way to check for paths and default folders is to use an FTP client with a graphical user interface which shows the current folder hierarchy.

This may be an artefact of the particular server we test with, or may be a default with curl, or may be because of the way that we've implemented the functions with curl, and so may be different from other FTP clients.

**Notes**

This function supports ftp, sftp and ftps.  The url prefix determines what type of connection it uses, see examples below.

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Different servers may or may not respond with any data on this function call, so use *BE_GetLastError* to determine if the call was able to be made or not, and then the other HTTP functions for more information 

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation :

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

Connecting to a standard FTP server to delete a file :

	BE_FTP_Delete ( "ftp://example.com/path/folder/file.txt" )

Connecting to an SFTP server :

	BE_FTP_Delete ( "sftp://example.com:2000/path/folder/file.txt" ; "account" ; "password" )

---

## BE_FTP_Upload

```
BE_FTP_Upload ( url ; data ; { username ; password } )
```

**Description**
Uploads the to the ftp/sftp/ftps *url* specified, using the *data* from a container field.

**Parameters**
* *url* : The url to connect to the server, with the path to locate the file to upload, and the filename to upload as.
* *data* : The container field file to upload.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

The url is both the server connection detail as :

	protocol://server:port/

and the path is both the location of the file to upload and the name of the file to upload, eg :

	protocol://server:port/path/to/file/to/upload.txt

See the notes for more info about paths.

**Version History**
* 3.0.0 : First Release

**Notes about paths**

Our limited testing seems to be that when using the *BE_FTP_Upload* function, the path is an absolute path, not an relative path to the users default.  So when you login to the server, you often get put into a "home" folder, such that the current path at login time is :

	/home/nick

So to upload a file at the path /home/nick/folder/file.txt you will need to use the url as :

	sftp://example.com:2000/home/nick/folder/file.txt

So you need to use the full path to the location.  Also note that you can't often get outside your own home folder, so any other path would fail.  This is the opposite to the *BE_FTP_Delete* and function where the path is a relative path.  So to delete the same file you uploaded above, you would use :

	sftp://example.com:2000/folder/file.txt

The simple way to check for paths and default folders is to use an FTP client with a graphical user interface which shows the current folder hierarchy.

This may be an artefact of the particular server we test with, or may be a default with curl, or may be because of the way that we've implemented the functions with curl, and so may be different from other FTP clients.

**Notes**

This function supports ftp, sftp and ftps.  The url prefix determines what type of connection it uses, see examples below.

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Different servers may or may not respond with any data on this function call, so use *BE_GetLastError* to determine if the call was able to be made or not, and then the other HTTP functions for more information afterwards.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation :

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

	BE_FTP_Upload ( "ftp://example.com/path/folder/" & GetContainerAttribute ( FTP::File ; "filename" ) ; FTP::File )

	BE_FTP_Upload ( "sftp://example.com:2000/path/folder/" & GetContainerAttribute ( FTP::File ; "filename" ) ; FTPTest::File ; "account" ; "password" )

---

## BE_FTP_UploadFile

```
BE_FTP_UploadFile ( url ; pathToFile ; { username ; password } )
```

**Description**
Uploads the to the ftp/sftp/ftps *url* specified, using the *pathToFile* as the location of the file to upload from disk.

**Parameters**
* *url* : The url to connect to the server, with the path to locate the file to upload, and the filename to upload as.
* *pathToFile* : A plugin path to the file on disk.
* *username* ( optional ) : The username if the url requires authentication.
* *password* ( optional ) : The password if the url requires authentication.

The url is both the server connection detail as :

	protocol://server:port/

and the path is both the location of the file to upload and the name of the file to upload, eg :

	protocol://server:port/path/to/file/to/upload.txt

See the notes for more info about paths.

**Version History**
* 4.2.0 : First Release

**Notes about paths**

Our limited testing seems to be that when using the *BE_FTP_UploadFile* function, the destination path is an absolute path, not an relative path to the users default.  So when you login to the server, you often get put into a "home" folder, such that the current path at login time is :

	/home/nick

So to upload a file at the path /home/nick/folder/file.txt you will need to use the url as :

	sftp://example.com:2000/home/nick/folder/file.txt

So you need to use the full path to the location.  Also note that you can't often get outside your own home folder, so any other path would fail.  This is the opposite to the *BE_FTP_Delete* and function where the path is a relative path.  So to delete the same file you uploaded above, you would use :

	sftp://example.com:2000/folder/file.txt

The simple way to check for paths and default folders is to use an FTP client with a graphical user interface which shows the current folder hierarchy.

This may be an artefact of the particular server we test with, or may be a default with curl, or may be because of the way that we've implemented the functions with curl, and so may be different from other FTP clients.

**Notes**

This function supports ftp, sftp and ftps.  The url prefix determines what type of connection it uses, see examples below.

Headers can be set beforehand with *BE_HTTP_SetCustomHeader*.  Authentication types and other options can be set beforehand with the *BE_CurlSetOption* function.

Different servers may or may not respond with any data on this function call, so use *BE_GetLastError* to determine if the call was able to be made or not, and then the other HTTP functions for more information afterwards.

Error codes that you get from the *BE_GetLastError* function after this function call comes from the curl library itself and not the plugin.  To find a specific error code use this documentation :

[http://curl.haxx.se/libcurl/c/libcurl-errors.html](http://curl.haxx.se/libcurl/c/libcurl-errors.html)

Also use the *BE_CurlTrace* function to see the transcript of the connection to diagnose any other issues that may have come up and can't be determined from the error value alone.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |

This function has been superseded by the Insert From URL script step, and may be deprecated in a future release.  However the BE functions allow for a larger number of curl options, have a better response mechanism with the separate BE_HTTP_ResponseCode function, and have the advantage of not being a single script step.  Whether or not to deprecate the BE functions depends on end user needs.

**Example Code**

	BE_FTP_UploadFile ( "ftp://example.com/path/folder/myfilname.txt" ; "/Volumes/HD/Path/To/myfilname.txt" )

	BE_FTP_UploadFile ( "sftp://example.com:2000/path/folder/myfilname.txt" ; "/Volumes/HD/Path/To/myfilname.txt" ; "account" ; "password" )
