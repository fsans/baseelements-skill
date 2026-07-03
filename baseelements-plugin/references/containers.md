# Containers — BaseElements Plugin

## BE_ContainerCompress

```
BE_ContainerCompress ( data ; { filename } )
```

**Description**

Converts stored container field data between the internal un-compressed and compressed formats - does not alter the content of the container as far as the end user sees.  This only adjusts how much storage it takes up on disk.

The internal compression format is gzip, there are no other compress options.

**Parameters**

* *data* : the uncompressed container field.
* *filename* : the filename to use.  If left out, the plugin will attempt to return text, which probably won't work for most uses of gzip.

**Version History**

* 3.1.0 : First Release
* 3.2.0 : Changed name from BE_Gzip to BE_ContainerCompress

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

## BE_ContainerUncompress

```
BE_ContainerUncompress ( gzip_data ; { filename } )
```

**Description**

Converts stored container field *gzip_data* between the internal compressed and un-compressed formats.

**Parameters**

* *gzip_data* : the compressed container field.
* *filename* : the filename to use.

**Version History**

* 3.1.0 : First Release
* 3.2.0 : Changed name to BE_ContainerUncompress from BE_UnGzip.

**Notes**

The internal compression format is gzip.

When inserting files into containers in FileMaker, you have the option to "compress" the file.  This function can change an compressed file into a uncompressed file.

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

## BE_ContainerIsCompressed

```
BE_ContainerIsCompressed ( containerField )
```

**Description**

Verifies if a FileMaker *containerField* with a file or image in it, was compressed when inserted into the field.

**Parameters**

* *containerField* : the container Field to test.

**Version History**

* 3.1.0 : First Release

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

## BE_ContainerListTypes

```
BE_ContainerListTypes ( container )
```

**Description**

Lists the internal FileMaker stored types of the field *container*.

**Parameters**

* *container* : the field to use as the source.

**Version History**

* 4.1.0 : First Release

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

## BE_ContainerGetType

```
BE_ContainerGetType ( container ; type )
```

**Description**

Gets the content of the field container in the format of type. Types are only what is available from doing  *BE_ContainerListTypes* first. If the type does not exist, it cannot be retrieved.

**Parameters**

* *container* : the container field or binary variable to use as the source.
* *type* : the type of binary data to get.

**Version History**

* 4.1.0 : First Release

**Notes**

Container field types are available by doing BE_ContainerListTypes and might change depending on FileMaker version, storage method, or other factors beyond the plugins control.

Mostly if you've stored a file, then the only useful format will be "FILE".  When images are stored FileMaker often converts the image to other formats such as "JPEG".

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

	BE_ContainerGetType ( table::field ; "JPEG" )

If you've stored a PDF in the field, then this will often retrive the first page of the PDF as a JPEG.

---

## BE_ConvertContainer

```
BE_ConvertContainer ( field ; { type } )
```

**Description**

This is to convert container field contents between the internal FileMaker "image" type and the internal "file" type. So you've used the Insert File script step to store an image file in a container, but then want that container to display the image content instead of a file icon.

**Parameters**

* *field* : a container field with a file or image in it.
* *type* ( optional, default:empty ) : either blank for a type of "file" or an option from the list below.

**Version History**

* 3.1.0 : First Release
* 4.2.0 : Removed the width and height parameters, the plugin now performs this natively.

**Notes**

This is not an image format conversion function so you cannot use to convert a png to jpeg etc.  Nor does it resize images.

It only converts between the two container types, of file and image.  When going from file to image, FileMaker needs to be told what sort of image it is, and that's what you need to pass in the parameters.

If you need to resize a JPEG image file, you can use the *BE_JPEGRecompress* function.

Use this function with a Set Field script step to convert from a container field stored as a file into a container image or vice versa. It can be used to set into the same field as the one referenced in the function parameter.

1. If only the field is supplied then it is converted to "FILE"
2. If "ZLIB" is supplied the file is compressed
3. Other known formats are ( all four character codes - note the space after PDF ):

"snd " *
"JPEG""
"GIFf" *
"EPS " *
"META" *
"PNGf"
"BMPf" *
"PDF "

 * = not tested.

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

## BE_ExportFieldContents

```
BE_ExportFieldContents ( field ; { outputPath }  )
```

**Description**

Exports the contents of the container *field* to disk at the *outputPath* specified. Container fields should be inserted via "Insert File". This provides a similar functionality to the *Export Field Contents* script step, but this funciton is also available on FileMaker Server, whereas the script step is not.

**Parameters**

* *field* : The field to export.
* *outputPath* ( optional, default:temp ): The path to write to.

**Version History**

* 3.0.0 : First Release
* 3.3.0 : Recursively create any directories needed
* 4.0.0 : Allowed for images with resource forks
* 4.0.0 : Changed the outputPath parameter to be optional.

**Notes**

If not supplied it will write the file to the temp folder and return the path to the file.

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

## BE_JPEGRecompress

```
BE_JPEGRecompress ( jpeg ; { compressionLevel ; scale } )
```

**Description**

Re-compresses the jpeg image file found at the container field *jpeg*, using a *scale* factor and *compressionLevel*.

**Parameters**

* *jpeg* : A JPEG file stored in a container.
* *compressionLevel* ( optional, default:75 ) : A value between 1 and 100 (inclusive). The default value, for no compelling reason, is 75.
* *scale* ( optional, default:0.125 ) : A number for the scale of the resulting image.  The default value is 0.125.

**Version History**

* 3.1.0 : First Release
* 3.2.0 : Changed the height and width to a scaling factor to better match the actual workings of the library used.
* 3.1.0 : Renamed from BE_JPEG_Recompress.

**Notes**

Use this function with a *Set Field* script step to convert one container to another.  It can be used to set the same field as the one referenced in the function parameter.

Values for the "scale" parameter are :

2
1.875
1.75
1.625
1.5
1.375
1.25
1.125
1
0.875
0.75
0.625
0.5
0.375
0.25
0.125

Any other values are rounded down to the nearest value, except values below 0.125 where it will use 0.125.

**Compatibility**

| Platform | Compatibility |
|-----------|-----------|
| Status | Active |
| Mac FMP | Yes |
| Win FMP | Yes |
| FMS | Yes |
| iOS | Yes |
| Linux | Yes |
