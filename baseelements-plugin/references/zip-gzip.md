# Zip and Gzip — BaseElements Plugin

## BE_Zip

```
BE_Zip ( filePathList ; { archiveFilePath } )
```

**Description**

Compresses the file(s) found at the *filePathList* and optionally saves the zip file to *archiveFilePath*.

**Parameters**

* *filePathList* : a plugin file path, or a container field.
* *archiveFilePath* : a plugin file path to put the result.

If the *archiveFilePath* parameter is not specified, then the zip file is put into the same folder as first filename in the list, and has the .zip extension appended to the filename. This will overwrite an existing file if it exists with that name already.

**Version History**

* 1.3.0 : First Release
* 2.0.0 : Bug Fixes
* 2.2.0 : added the archiveFilePath parameter
* 2.3.0 : modified the archiveFilePath parameter to allow a list of file paths, instead of a single file/folder
* 3.0.0 : improved error handling, create empty directories in archives
* 4.1.3 : don't crash when there's nothing to archive
* 4.1.3 : return an error if more than one file with the same name is added to the archive
* 4.2.0 : added the option to allow filePathList to be a container field

**Notes**

The zip file format doesn't have a provision for adding multiple files of the same name. So if you run : 

	BE_Zip ( List ( "myfile.txt" ; "folder/myfile.txt" ) ) 

Then it will store both files in the zip, BUT when you unzip the second file will overwrite the first one. This is a limitation of the zip format, not a plugin limitation. 

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

Example : you have 2 files in the documents folder called file1.pdf and file2.pdf that you want to add to a zip called archive.zip in the same folder. You need to convert the docs path into a plugin path first, and then do :

	Let ( [ 
	path = ConvertFromFileMakerPath ( Get ( DocumentsPath ) ; PosixPath ) ; 
	pathlist = List ( path & "file1.pdf" ; path & "file2.pdf" ) ] ; 
	BE_Zip ( pathlist ; path & "archive.zip" )
	)

---

## BE_Unzip

```
BE_Unzip ( archiveFilePath ; { outputFolderPath } )
```

**Description**

Unzips the archive file found at the path *archiveFilePath*. The unzipped file(s) are put into the same location as the zip file itself by default, or into the *outputFolderPath* if specified. Any existing file(s) will be overwritten.

**Parameters**

* *archiveFilePath* : a plugin file path, or a container field.
* *filename* : a system folder path to put the result..

If the *archiveFilePath* container field contains text, it's treated as a path to a file.

**Version History**

* 1.3.0 : First Release
* 2.2.0 : added the outputFolderPath parameter
* 4.2.0 : added the option to allow archiveFilePath to be a container field

**Notes**

If you're unzipping a file, then this function should be used with a *Set Field* script step and the results stored as a container field.  If you're expecting a text result then this could be used anywhere text is expected.

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

	BE_Unzip ( BE_FileSelectDialog ( "Select a Zip file to unzip." ) )

	BE_Unzip ( "/Users/nick/Desktop/MyFile.zip" )
	BE_Unzip ( "/Users/nick/Desktop/MyFile.zip" ; "/Users/nick/Desktop/ResultFolder")

---

## BE_Gzip

```
BE_Gzip ( data ; { filename } )
```

**Description**

Compresses the file/text found in *data* and optionally applies the filename, otherwise uses the container data appended with the .gzip extension.

**Parameters**

* *data* : a container field with an unzipped file in it, or text data.
* *filename* : optional filename to use, appending the .gzip extension.

**Version History**

* 3.3.0 : First Release

**Notes**

This function should be used with a *Set Field* script step and the results stored as a container field.

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

## BE_UnGzip

```
BE_UnGzip ( gzip_data ; { filename } )
```

**Description**

Uncompresses the file found in *gzip_data* and optionally applies the *filename*, otherwise uses the detail found in the container field if it exists.

**Parameters**

* *gzip_data* : a container field with an gzipped file in it.
* *filename* : optional filename to use, appending the .gzip extension.

**Version History**

* 3.3.0 : First Release

**Notes**

If you're unzipping a file, then this function should be used with a *Set Field* script step and the results stored as a container field.  If you're expecting a text result then this could be used anywhere text is expected.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | Yes  |  
| iOS | Yes  |  
| Linux | Yes  |  
