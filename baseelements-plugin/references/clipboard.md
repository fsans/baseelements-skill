# Clipboard — BaseElements Plugin

## BE_ClipboardFormats

```
BE_ClipboardFormats
```

**Description**
Returns a return value list of the format types of the content of the current clipboard.

Once you've retrieved this list, get one of the types of this output using the native *GetValue* function, and pass that as the parameter to *BE_ClipboardGetText* or *BE_ClipboardGetFile* to get clipboard content.

You can then also use *BE_ClipboardSetText* or *BE_ClipboardSetFile* when setting onto the clipboard using the same type value.

**Version History**

* 1.0.0 : First Release
* 4.0.0 : Returns error 3, command not available on iOS and Linux

**Notes**

The clipboard can copy one type of content, but will often have multiple copies in different formats stored.  For example, copying FileMaker Script Steps on the mac will give the formats :

	dyn.ah62d4rv4gk8zuxnxnq
	CorePasteboardFlavorType 0x584D5353

or copying plain text from FileMaker on the Mac will give :

	com.apple.traditional-mac-plain-text
	CorePasteboardFlavorType 0x54455854
	public.utf16-plain-text
	CorePasteboardFlavorType 0x75747874
	public.utf8-plain-text
	NSStringPboardType
	dyn.ah62d4rv4gk8yqxpdsq
	CorePasteboardFlavorType 0x464D6373
	public.rtf
	NeXT Rich Text Format v1.0 pasteboard type

So it's possible to utilise different types depending on your intended use.  The different formats can be quite different, content wise, for example if you copied RichText with formatting, you clipboard may contain types of :

	public.rtf
	public.utf8-plain-text
	
Only the first type would retain the format information.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

---

## BE_ClipboardGetText

```
BE_ClipboardGetText ( format )
```

**Description**
Tries to convert the item on the current clipboard, designated by the *format* passed to the function.

The *format* used must be one of the values from the BE_ClipboardFormats function.

The function will only deal with text clipboards, so other clipboard types are ignored. To get binary types use BE_ClipboardGetFile.

**Parameters**

* *format* : This has to be one of the formats on the current clipboard.

**Version History**

* 1.0.0 : First Release.
* 4.0.0 : Return error 3, command not available on Linux.
* 4.0.0 : Add utf16 support on Mac.
* 4.0.2 : Renamed from BE_ClipboardText.

**Notes**

The plugin will try to store the result, but cannot guarantee exact text conversion from the clipboard. The clipboard can be of many different types, and even if it's text it can be stored with different encodings. It will depend on where you're storing the result ( field, variable, on disk, back to plugin etc ) and how ( direct in the plugin, via step, via field etc ), as to whether it's an exact match for the clipboard.

The *BE_SetTextEncoding* function does have an effect on how this result is used within the plugin.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

**Example Code**

	BE_ClipboardGetText ( "dyn.ah62d4rv4gk8zuxnxnq" )

Turns a copied FileMaker Script Step into the text XML representation.

---

## BE_ClipboardSetText

```
BE_ClipboardSetText ( text ; format )
```

**Description**
Sets the clipboard to the *text* and applies a clipboard **format**. Clipboard formats are set by the application that does the copy and vary widely based on the content types.

Use the **BE_ClipboardFormats** function to get examples of different types. The format you choose should depend on what you expect the user to do with the clipboard content.

This function only works with text, to set non-text data use **BE_ClipboardSetFile**.

**Parameters**

* *text* : The text to set the clipboard to.
* *format* : The format code for the content.

**Version History**

* 1.0.0 : First Release
* 4.0.0 : Return error 3, command not available on Linux.
* 4.0.2 : Renamed from BE_SetClipboardText

**Notes**

When setting a type it's best to copy a sample and then use *BE_ClipboardFormats* to see what types are used.

Also note that many of the windows text formats are special "null terminated strings". Meaning that they expect the text to have an extra character on the end, and if you don't include them, then they will chop the last character off the end of your text. The full windows docs can be found here : [https://docs.microsoft.com/en-us/windows/win32/dataxchg/standard-clipboard-formats](https://docs.microsoft.com/en-us/windows/win32/dataxchg/standard-clipboard-formats).

See the examples for a method to add the null terminated string to your text.

You can only set a single format when setting the clipboard from BE.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

**Example Code**

	BE_ClipboardSetText ( $ScriptStepXML ; "dyn.ah62d4rv4gk8zuxnxnq" )

Turns XML text into a FileMaker Script Step ready to be pasted back into FileMaker. The example above uses a special code that FileMaker uses "dyn.ah62d4rv4gk8zuxnxnq" to have a unique type of clipboard text, that only FileMaker knows about. So even though it's text, no other text app recognises this and so won't paste it.

	BE_ClipboardSetText ( TABLE::field & Char ( 0 ) ; "CF_TEXT" )

An example of a NULL terminated string that you set onto the clipboard for the specific windows text type.

---

## BE_ClipboardGetFile

```
BE_ClipboardGetFile ( format ; { fileName } )
```

**Description**
Retrieves a binary item from the current clipboard, designated by the *format* passed to the function.

The *format* used must be one of the values from the *BE_ClipboardFormats* function.

Use this function as the calculation for a *Set Field* script step when setting a container field, or when setting a variable to a container type.

**Parameters**

* *format* : The type of the format to get from the list of formats on the current clipboard.
* *fileName* ( optional, default:existing file name if included ) : Optional parameter to set a resulting filename, but can be left out if the clipboard already has a filename.

**Version History**

* 4.0.3 : First Release

**Notes**

If the format is a binary file type, and has a filename, then that parameter is not required - not all types include a filename, and it's not always possible to get the filename from all types. Otherwise a filename is recommended so that the result is useful inside FileMaker.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

**Example Code**

	BE_ClipboardGetFile ( "com.adobe.pdf" ; "Object.pdf" )

Used in a Set Field step to get the pdf of a copied layout object. In this case the clipboard would also contain the text XML description under a different "format", plus possibly a plain text "format" with some of the layout content.

The result is not an image at this point, it's still a file.  You can convert to an image with :

	BE_ConvertContainer ( BE_ClipboardGetFile ( "com.adobe.pdf" ; "Object.pdf" ) ; "PDF " )

Converts the above file container into an image container ( note the space after PDF, all types are 4 characters ).

---

## BE_ClipboardSetFile

```
BE_ClipboardSetFile ( fileData ; format )
```

**Description**
Sets the clipboard to the *fileData* and applies a clipboard *format*. Clipboard formats are set by the application that does the copy and vary widely based on the content types.

Use the *BE_ClipboardFormats* function to get examples of different types. The *format* you choose should depend on what you expect the user to do with the clipboard content.

This function only works with binary data, to set text data use *BE_ClipboardSetText*.

**Parameters**

* *fileData* : A container field or a file set into a variable.
* *format* : The format text code for the content.

**Version History**

* 4.0.3 : First Release

**Notes**

When setting a type it's best to copy a sample of that type from some other application and then use *BE_ClipboardFormats* to see what types are used.

You can only set a single format when setting the clipboard from BE, even though the clipboard allows multiple types.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | No |
| iOS | No |
| Linux | No |

**Example Code**

	BE_ClipboardSetFile ( table::containerField ; "com.adobe.pdf" )

Sets the clipboard with a PDF from a field.
