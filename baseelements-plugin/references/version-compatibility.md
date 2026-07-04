# Version Compatibility — BaseElements Plugin

This skill documents the **current v5.x API**. Two versions introduced breaking renames that affect existing solutions.

## Checking the Installed Version

```
BE_Version            // human-readable string: "5.1.0"
BE_VersionAutoUpdate  // comparable 8-digit number: "05010000"
```

Use `GetAsNumber ( BE_VersionAutoUpdate )` to compare versions numerically. The format is `MMmmppbb` (Major · minor · patch · build).

---

## v5.0.0 Breaking Changes (March 2024)

### Function rename

| Old name (v4.x) | Current name (v5.0+) |
|-----------------|----------------------|
| `BE_XSLTApply` | `BE_XSLT_Apply` |

### New functions added in v5.0.0

| Function | Description |
|----------|-------------|
| `BE_CreateKeyPair` | Generate an RSA private key |
| `BE_GetPublicKey ( privateKey )` | Extract the public key from a private key |
| `BE_EncryptWithKey ( text ; keyText )` | Encrypt text using an X.509 certificate |
| `BE_DecryptWithKey ( encryptedText ; keyText )` | Decrypt text using an X.509 private key |
| `BE_CurlGetInfo ( getInfoOption )` | Get metadata about the most recent curl operation |
| `BE_JSON_jq ( json ; filter { ; options } )` | Process JSON using the jq filter language |

### New constants added in v5.0.0

`BE_ScriptControlHalt`, `BE_ScriptControlExit`, `BE_ScriptControlResume`, `BE_ScriptControlPause` — used with the `scriptControl` parameter of `BE_ScriptExecute`. See `references/constants.md`.

### Functions removed in v5.0.0

`BE_JSONPath_Deprecated`, `BE_JSON_Error_Description_Deprecated`, `BE_WriteTextFileToContainer_Deprecated` — these were already tagged deprecated; v5.0.0 removed them entirely.

---

## v4.0.2 Breaking Changes (July 2018)

This was the largest single rename in the plugin's history — over 60 functions and constants renamed to adopt a consistent naming convention. Any solution built on v3.x or earlier will use the old names below.

### Function renames

| Old name (pre-4.0.2) | Current name (4.0.2+) |
|----------------------|-----------------------|
| `BE_ApplyXSLT` | `BE_XSLTApply` → then `BE_XSLT_Apply` in v5.0 |
| `BE_ApplyXSLTInMemory` | `BE_XSLT_ApplyInMemory` |
| `BE_Array_Change_Value` | `BE_ArrayChangeValue` |
| `BE_Array_Delete` | `BE_ArrayDelete` |
| `BE_Array_Find` | `BE_ArrayFind` |
| `BE_ClipboardText` | `BE_ClipboardGetText` |
| `BE_CopyFile` | `BE_FileCopy` |
| `BE_CreateFolder` | `BE_FolderCreate` |
| `BE_Curl_Set_Option` | `BE_CurlSetOption` |
| `BE_Curl_Trace` | `BE_CurlTrace` |
| `BE_CurrentTimeMilliseconds` | `BE_TimeCurrentMilliseconds` |
| `BE_Decrypt_AES` | `BE_DecryptAES` |
| `BE_DeleteFile` | `BE_FileDelete` |
| `BE_DisplayDialog` | `BE_DialogDisplay` |
| `BE_Encrypt_AES` | `BE_EncryptAES` |
| `BE_ExecuteScript` | `BE_ScriptExecute` |
| `BE_File_Modification_Timestamp` | `BE_FileModificationTimestamp` |
| `BE_FileType_All` | `BE_FileTypeAll` |
| `BE_FileType_File` | `BE_FileTypeFile` |
| `BE_FileType_Folder` | `BE_FileTypeFolder` |
| `BE_Get_Machine_Name` | `BE_GetMachineName` |
| `BE_GetPreference` | `BE_PreferenceGet` |
| `BE_HTTP_PUT_DATA` | `BE_HTTP_PUTData` |
| `BE_HTTP_PUT_FILE` | `BE_HTTP_PUTFile` |
| `BE_HTTP_Response_Code` | `BE_HTTP_ResponseCode` |
| `BE_HTTP_Set_Custom_Header` | `BE_HTTP_SetCustomHeader` |
| `BE_HTTP_Set_Proxy` | `BE_HTTP_SetProxy` |
| `BE_ImportFile` | `BE_FileImport` |
| `BE_JPEG_Recompress` | `BE_JPEGRecompress` |
| `BE_MoveFile` | `BE_FileMove` |
| `BE_OpenFile` | `BE_FileOpen` |
| `BE_PDF_Append` | `BE_PDFAppend` |
| `BE_PDF_GetPages` | `BE_PDFGetPages` |
| `BE_PDF_PageCount` | `BE_PDFPageCount` |
| `BE_ProgressDialog` | `BE_DialogProgress` |
| `BE_ProgressDialog_Update` | `BE_DialogProgressUpdate` |
| `BE_ReadTextFromFile` | `BE_FileReadText` |
| `BE_SaveFileDialog` | `BE_FileSaveDialog` |
| `BE_SelectFile` | `BE_FileSelectDialog` |
| `BE_SelectFolder` | `BE_FolderSelectDialog` |
| `BE_SetClipboardText` | `BE_ClipboardSetText` |
| `BE_SetPreference` | `BE_PreferenceSet` |
| `BE_SignatureGenerate_RSA` | `BE_SignatureGenerateRSA` |
| `BE_SignatureVerify_RSA` | `BE_SignatureVerifyRSA` |
| `BE_SMTP_AddAttachment` | `BE_SMTPAddAttachment` |
| `BE_SMTP_Send` | `BE_SMTPSend` |
| `BE_SMTP_Server` | `BE_SMTPServer` |
| `BE_SMTP_Set_Header` | `BE_SMTPSetHeader` |
| `BE_StripXMLNodes` | `BE_XMLStripNodes` |
| `BE_UTCMilliseconds` | `BE_TimeUTCMilliseconds` |
| `BE_Values_ContainsDuplicates` | `BE_ValuesContainsDuplicates` |
| `BE_Values_FilterOut` | `BE_ValuesFilterOut` |
| `BE_Values_Sort` | `BE_ValuesSort` |
| `BE_Values_TimesDuplicated` | `BE_ValuesTimesDuplicated` |
| `BE_Values_Trim` | `BE_ValuesTrim` |
| `BE_Values_Unique` | `BE_ValuesUnique` |
| `BE_Vector_DotProduct` | `BE_VectorDotProduct` |
| `BE_Vector_EuclideanDistance` | `BE_VectorEuclideanDistance` |
| `BE_WriteTextToFile` | `BE_FileWriteText` |
| `BE_XML_Canonical` | `BE_XMLCanonical` |
| `BE_XML_Parse` | `BE_XMLParse` |
| `BE_XML_Validate` | `BE_XMLValidate` |
