# Dialogs — BaseElements Plugin

## BE_DialogDisplay

```
BE_DialogDisplay ( title ; message ; defaultButton ; { cancelButton ; alternateButton } )
```

**Description**

Puts up a dialog almost exactly the same as the one that you can get with the Show Custom Dialog script step, minus the fields.  Use this function when you want to display a dialog from a calculation instead of a script step.

Result is the number of the button clicked - use the Constants as text replacements for button numbers.

**Parameters**

* *title* : The dialog title.
* *message* : The content of the dialog.
* *defaultButton* : Right most button text.
* *cancelButton* : Left most button text.
* *alternateButton* : Alternate button text.

**Version History**

* 1.0.0 : First Release
* 4.0.2 : Renamed from BE_DisplayDialog

**Notes**

The original intent of this function was that the built in step didn't allow you to customise some parts of the dialog, so this was critical for things like translated solutions.  With newer changes to FileMaker's native script step, this isn't as necessary however as a function imstead of a script step you can have other options to display dialogs.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

---

## BE_DialogProgress

```
BE_DialogProgress ( title ; description ; { maximum } )
```

**Description**

Shows either a barber pole progress dialog by leaving the *maximum* parameter out, or a regular start to finish progress dialog.

**Parameters**

* *title* : The dialog title.
* *description* : The content of the dialog.
* *maximum* ( optional ) : A positive integer for a progress dialog, or empty for barber pole.

**Version History**

* 2.1.0 : First Release
* 4.0.0 : Return error 3, command not available on iOS, Linux and under FMS.
* 4.0.2 : Renamed from BE_ProgressDialog

**Notes**

You can disable the "Cancel" button by calling the Allow User Abort [Off] script step prior to calling this function.

Barber pole dialogs are closed by calling BE_DialogProgressUpdate with a positive integer "number" parameter. Regular dialogs are closed with any value greater than maximum.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

**Example Code**

	BE_DialogProgress ( "title" ; "description" )
	
Will show the barber pole dialog.

	BE_DialogProgress ( "title" ; "description" ; 9 ) 

Will show a progress dialog with 10 steps ( 0 through 9 inclusive ).

---

## BE_DialogProgressUpdate

```
BE_DialogProgressUpdate ( number ; { description } )
```

**Description**

Used to either close a *BE_DialogProgress* dialog, or advance a progress dialog to the next step.

**Parameters**

* *number* : The number to move the progress bar to.
* *description* ( optional, default:previous value ) : The text to put into the dialog description.  If not included it will use the last value sent to this function, or will use the value that initially was set via *BE_DialogProgress*.

**Version History**

* 2.1.0 : First Release
* 4.0.0 : Return error 3, command not available on iOS, Linux and under FMS.
* 4.0.2 : Renamed from BE_ProgressDialog_Update

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

**Example Code**

	BE_DialogProgressUpdate ( 1 ; "description" )
	
Will close a dialog if created with empty as the maximum - eg Barber Pole.

	BE_DialogProgressUpdate ( $nextValue ; "description" )
	
Move the progress dialog, along to the next value, while updating the description text.

	BE_DialogProgressUpdate ( 10 )

Close a progress dialog that started with maximum 9, and had 10 steps ( 0 through 9 ).

---

## BE_FileSaveDialog

```
BE_FileSaveDialog ( prompt ; { fileName ; inFolder } )
```

**Description**

Displays the standard OS save file dialog. Changes the title of the dialog to the *prompt* specified, and defaults to *fileName* and sets the starting location as *inFolder*.

Returns the file path and filename that the user selected.  Check BE_GetLastError = 1 for when the user hits cancel or a value other than 0 for any other error.

**Parameters**

* *prompt* : The text to display in the dialog.
* *fileName* ( optional ) : The filename to start with. Leave empty to get the user to name the file.
* *inFolder* ( optional ) : The folder path to start in when opening the dialog. Defaults to the last used folder as determined by the operating system.

**Version History**

* 2.3.0 : First Release
* 4.0.0 : Return error 3, command not available on iOS, Linux and under FMS
* 4.0.2 : Renamed from BE_SaveFileDialog

**Notes**

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

---

## BE_FileSelectDialog

```
BE_FileSelectDialog ( prompt { ; inFolderPath } )
```

**Description**

Displays the standard OS select file dialog with the title of the dialog to the *prompt* specified, and starting path of the *inFolderPath*.

Result is the path to the file selected by the user.  Check BE_GetLastError = 1 for when the user hits cancel or a value other than 0 for any other error.

**Parameters**

* *prompt* : The text to display in the dialog.
* *inFolderPath* ( optional ) : The folder to start in when opening the dialog. Defaults to the last used folder as determined by the operating system.

**Version History**

* 1.0.0 : First Release
* 2.0.0 : Added the optional inFolderPath parameter.
* 2.2.0 : Added the ability to select multiple files.
* 2.2.0 : Update dialogs for new Windows OS versions
* 4.0.0 : Return error 3, command not available on iOS, Linux and under FMS
* 4.0.2 : Renamed from BE_SelectFile

**Notes**

PLUGIN PATHS ARE NOT FILEMAKER PATHS. The plugin uses the same path structure as your operating system, and you cannot pass to the plugin paths that start with file:/ or filewin:/ etc.  Read the FAQ for more info about paths.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

---

## BE_FolderSelectDialog

```
BE_FolderSelectDialog ( prompt { ; inFolderPath } )
```

**Description**

Displays the standard OS select folder dialog with the title of the dialog to the *prompt* specified, and starting path of the *inFolderPath*.

Result is the path to the folder selected by the user.  Check BE_GetLastError = 1 for when the user hits cancel or a value other than 0 for any other error.

**Parameters**

* *prompt* : The text to display in the dialog.
* *inFolderPath* ( optional ) : The folder to start in when opening the dialog. Defaults to the last used folder as determined by the operating system.

**Version History**

* 1.0.0 : First Release
* 1.3.0 : Added the optional inFolderPath parameter.
* 2.0.0 : Add New Folder button.
* 2.2.0 : Update dialogs for new Windows OS versions
* 4.0.0 : Return error 3, command not available on iOS, Linux and under FMS
* 4.0.2 : Renamed from BE_SelectFolder

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |  
| Mac FMP | Yes  |  
| Win FMP | Yes  |  
| FMS | No  |  
| iOS | No  |  
| Linux | No  |  

---
