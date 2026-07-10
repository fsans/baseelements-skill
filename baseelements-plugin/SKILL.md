---
name: baseelements-plugin
description: Reference guide for the BaseElements Plugin for FileMaker. Use when working with FileMaker calculations or scripts that call BE_ functions, or when asked to perform HTTP/curl requests, file I/O, XML/XSLT processing, AES/RSA encryption, PDF manipulation, SMTP email, zip/gzip compression, regex, or JavaScript evaluation inside FileMaker. Triggers include any mention of "BaseElements", "BE_" prefixed function names, or FileMaker plugin operations for HTTP, files, encryption, JSON, XML, PDF, SMTP, or compression.
---

# BaseElements Plugin for FileMaker

The BaseElements Plugin (BE) is a free, open-source FileMaker plugin providing over 100 calculation functions. All functions are prefixed `BE_` and called inside FileMaker calculations or `Set Variable` script steps. The plugin must be installed on every FileMaker client or server where the solution runs.

## Safety Boundaries

This skill is a **reference guide only**. It does not execute code, access the filesystem, or make network requests. Specifically, this skill must **not**:

- Execute FileMaker calculations, scripts, or plugin functions on behalf of the user
- Read, write, create, or delete any files or directories
- Make HTTP requests, open network connections, or send email
- Access, decrypt, or exfiltrate credentials, keys, or secrets
- Install, uninstall, or modify the BaseElements Plugin itself
- Modify the user's FileMaker solution, schema, or layout definitions

All code examples in this skill are illustrative snippets to be pasted into the user's FileMaker solution by the user. The skill's sole output is documentation and guidance; any execution happens inside FileMaker under the user's control.

## Critical Patterns

### Error handling — capture immediately

`BE_GetLastError` is overwritten on **every** plugin function call (except `BE_GetLastError`, `BE_GetLastDDLError`, and `BE_DebugInformation`). Capture it as the very next step after each call.

```
Set Variable [ $result ; BE_HTTP_POST ( $url ; $body ) ]
Set Variable [ $error  ; BE_GetLastError ]   // must be the very next call
```

Return values: `0` = success · `1` = user canceled · `3` = OS incompatible.

### HTTP — prefer native `Insert from URL`

FileMaker's native [`Insert from URL`](https://help.claris.com/en/insert-from-url.html) script step uses the **same libcurl library** as the `BE_HTTP_*` functions, requires no plugin, and runs on every client. It accepts curl-style options via its `cURL options` parameter.

**Default to `Insert from URL` for HTTP requests. Do not use `BE_HTTP_*` / `BE_CurlSetOption` to replace any functionality the native step can perform, unless the user explicitly requests the plugin functions.**

The only verified cases where `BE_HTTP_*` / `BE_CurlSetOption` / `BE_CurlGetInfo` are needed (IFU cannot do them) are: `CURLOPT_CERTINFO` + `BE_CurlGetInfo` for TLS cert chain/expiry capture, any `CURLINFO_*` metadata retrieval, raw `CURLOPT_*` constants with no `--option` equivalent, `BE_HTTP_GETFile` for direct-to-disk downloads, and calculation-context HTTP calls. See `references/http-urls.md` for the full list and the `BE_HTTP_*` signatures.

### HTTP (BE_HTTP_*) — configure before, inspect after

When the plugin HTTP functions are used (for the verified BE-only cases above, or per explicit user request), set options and headers before the call and inspect immediately after:

```
// 1. Options and headers (set before the call)
BE_HTTP_SetCustomHeader ( "Content-Type" ; "application/json" )
BE_HTTP_SetCustomHeader ( "Authorization" ; "Bearer " & $token )

// 2. Call
Set Variable [ $response ; BE_HTTP_POST ( $url ; $body ) ]

// 3. Inspect
Set Variable [ $curlError ; BE_GetLastError ]        // 0 = curl ok
Set Variable [ $httpCode  ; BE_HTTP_ResponseCode ]   // 200 / 404 / 500 etc.
Set Variable [ $trace     ; BE_CurlTrace ]           // full transcript for debugging
```

Call `BE_HTTP_SetCustomHeader` or `BE_CurlSetOption` with no arguments to clear all state before reuse. Curl error codes come from libcurl, not FileMaker.

**TLS certificate validation is on by default.** Do not disable it (`BE_CurlSetOption ( "CURLOPT_SSL_VERIFYPEER" ; 0 )`) in production code — that opens the request to man-in-the-middle attacks. Only set it to `0` for local testing against a self-signed cert, and never against a real endpoint.

### Paths — plugin paths ≠ FileMaker paths

File functions use native OS paths (`/Users/name/file.txt` on Mac, `C:\Users\name\file.txt` on Win). Use `BE_GetSystemDrive` on FileMaker Server. Use `BE_ExportFieldContents` to extract a plugin-compatible path from a container field.

### Arrays — handle-based

`BE_ArraySetFromValueList` returns an **integer handle**, not the data. Pass that handle to every subsequent array operation, and free it when done.

```
Set Variable [ $h   ; BE_ArraySetFromValueList ( $list ) ]
Set Variable [ $val ; BE_ArrayGetValue ( $h ; 2 ) ]
BE_ArrayDelete ( $h )
```

### SMTP — configure then send

```
BE_SMTPServer ( "smtp.example.com" ; 587 ; "user" ; "pass" )
BE_SMTPAddAttachment ( Table::File )       // optional
BE_SMTPSend ( "from@" ; "to@" ; "Subject" ; "Body" )
```

### FileMaker SQL — prefer native `ExecuteSQL` for SELECT

FileMaker's native [`ExecuteSQL`](https://help.claris.com/en/execute-sql.html) function only supports `SELECT` queries, but it should be the **preferred way for every read operation** because it runs without the plugin, is available everywhere (FMP, FMS, Go, WebDirect), and supports **parameterized queries** via the optional `arguments` parameters — which `BE_FileMakerSQL` does not offer. Parameterized queries protect against SQL injection and avoid quoting/escaping headaches with strings, dates, and numbers.

```
// PREFERRED — native, parameterized, no plugin required
ExecuteSQL (
    "SELECT name, email FROM Contacts WHERE status = ? AND created >= ?" ;
    Char ( 9 ) ; Char ( 13 ) ;
    "active" ; Date ( 1 ; 1 ; 2024 )
)
```

Use `BE_FileMakerSQL` only for **non-SELECT** operations that native FileMaker SQL cannot perform — `INSERT`, `UPDATE`, `DELETE`, `CREATE TABLE`, `DROP TABLE`, `ALTER TABLE`, and other DDL/DML statements — or when you need plugin-only features such as targeting another open database (`databaseName`), writing the result straight to disk (`outputPath`), or returning binary/container data (`asText = False`).

```
// BE_FileMakerSQL for writes/DDL — native ExecuteSQL cannot do these
BE_FileMakerSQL ( "UPDATE Contacts SET status = 'inactive' WHERE lastSeen < '2023-01-01'" )
BE_FileMakerSQL ( "DELETE FROM TempImport WHERE importID = " & $id )
BE_FileMakerSQL ( "CREATE TABLE AuditLog ( id INT, msg VARCHAR )" )
```

**Rule of thumb:** `SELECT` → native `ExecuteSQL` (with `?` placeholders and arguments). Anything else → `BE_FileMakerSQL`. See `references/filemaker-sql.md` for the full `BE_FileMakerSQL` signature and notes.

### Version compatibility — check before using v5.0+ features

This skill documents the **v5.x API**. Two releases introduced breaking changes:

- **v4.0.2 (July 2018)** — ~60 function and constant renames (e.g. `BE_ExecuteScript` → `BE_ScriptExecute`, `BE_FileType_File` → `BE_FileTypeFile`)
- **v5.0.0 (March 2024)** — `BE_XSLTApply` → `BE_XSLT_Apply`; 6 new functions; 4 new ScriptControl constants; 3 deprecated functions removed

Check the installed version before relying on v5.0+ functions:

```
GetAsNumber ( BE_VersionAutoUpdate ) ≥ 5000000   // true if v5.0.0+
```

See `references/version-compatibility.md` for the full rename tables and migration guide.

---

## Function Reference

To look up full signatures, parameters, version history, compatibility, and examples for any function, read the corresponding reference file.

| Category | Functions | Reference |
|----------|-----------|-----------|
| Arrays | `BE_ArraySetFromValueList` · `BE_ArrayGetValue` · `BE_ArrayChangeValue` · `BE_ArrayFind` · `BE_ArrayGetSize` · `BE_ArrayDelete` | `references/arrays.md` |
| Clipboard | `BE_ClipboardFormats` · `BE_ClipboardGetText` · `BE_ClipboardSetText` · `BE_ClipboardGetFile` · `BE_ClipboardSetFile` | `references/clipboard.md` |
| Containers | `BE_ContainerCompress` · `BE_ContainerUncompress` · `BE_ContainerIsCompressed` · `BE_ContainerListTypes` · `BE_ContainerGetType` · `BE_ConvertContainer` · `BE_ExportFieldContents` · `BE_JPEGRecompress` | `references/containers.md` |
| Data Manipulation | `BE_StackPush` · `BE_StackPop` · `BE_StackCount` · `BE_StackDelete` · `BE_VariableSet` · `BE_VariableGet` · `BE_TextExtractWords` | `references/data-manipulation.md` |
| Dialogs | `BE_DialogDisplay` · `BE_DialogProgress` · `BE_DialogProgressUpdate` · `BE_FileSaveDialog` · `BE_FileSelectDialog` · `BE_FolderSelectDialog` | `references/dialogs.md` |
| Encoding & Encryption | `BE_EncryptAES` · `BE_DecryptAES` · `BE_CipherEncrypt` · `BE_CipherDecrypt` · `BE_MessageDigest` · `BE_SignatureGenerateRSA` · `BE_SignatureVerifyRSA` · `BE_SetTextEncoding` · `BE_CreateKeyPair` · `BE_GetPublicKey` · `BE_EncryptWithKey` · `BE_DecryptWithKey` *(v5.0+)* | `references/encoding-encryption.md` |
| Error Checking | `BE_GetLastError` · `BE_GetLastDDLError` · `BE_CurlTrace` · `BE_DebugInformation` | `references/error-checking.md` |
| Files & Folders | `BE_FileExists` · `BE_FileReadText` · `BE_FileWriteText` · `BE_FileCopy` · `BE_FileMove` · `BE_FileDelete` · `BE_FileImport` · `BE_FileListFolder` · `BE_FileModificationTimestamp` · `BE_FileOpen` · `BE_FilePatternCount` · `BE_FileReplaceText` · `BE_FileSize` · `BE_FolderCreate` · `BE_FTP_Delete` · `BE_FTP_Upload` · `BE_FTP_UploadFile` | `references/files-folders.md` |
| HTTP & URLs | `BE_HTTP_GET` · `BE_HTTP_GETFile` · `BE_HTTP_POST` · `BE_HTTP_PATCH` · `BE_HTTP_PUTData` · `BE_HTTP_PUTFile` · `BE_HTTP_DELETE` · `BE_HTTP_SetCustomHeader` · `BE_HTTP_ResponseCode` · `BE_HTTP_ResponseHeaders` · `BE_HTTP_SetProxy` · `BE_CurlSetOption` · `BE_OpenURL` · `BE_CurlGetInfo` *(v5.0+)* | `references/http-urls.md` |
| FileMaker SQL | `BE_FileMakerSQL` | `references/filemaker-sql.md` |
| Miscellaneous | `BE_ExecuteSystemCommand` · `BE_EvaluateJavaScript` · `BE_GetMachineName` · `BE_GetSystemDrive` · `BE_Pause` · `BE_Version` · `BE_VersionAutoUpdate` | `references/miscellaneous.md` |
| Regular Expression | `BE_RegularExpression` | `references/regular-expression.md` |
| Scripts | `BE_ScriptExecute` · `BE_ScriptStepInstall` · `BE_ScriptStepPerform` · `BE_ScriptStepRemove` | `references/scripts.md` |
| PDF | `BE_PDFPageCount` · `BE_PDFAppend` · `BE_PDFGetPages` | `references/pdf.md` |
| Preferences | `BE_PreferenceSet` · `BE_PreferenceGet` · `BE_PreferenceDelete` | `references/preferences.md` |
| SMTP Email | `BE_SMTPServer` · `BE_SMTPSend` · `BE_SMTPAddAttachment` · `BE_SMTPSetHeader` | `references/smtp-email.md` |
| Time | `BE_TimeCurrentMilliseconds` · `BE_TimeUTCMilliseconds` · `BE_TimeZoneOffset` | `references/time.md` |
| Value Lists | `BE_ValuesSort` · `BE_ValuesUnique` · `BE_ValuesFilterOut` · `BE_ValuesContainsDuplicates` · `BE_ValuesTimesDuplicated` · `BE_ValuesTrim` | `references/value-lists.md` |
| Vectors | `BE_VectorDotProduct` · `BE_VectorEuclideanDistance` | `references/vectors.md` |
| XML, XSLT & JSON | `BE_XPath` · `BE_XPathAll` · `BE_XSLT_ApplyInMemory` · `BE_XSLT_Apply` · `BE_XMLParse` · `BE_XMLValidate` · `BE_XMLTidy` · `BE_XMLCanonical` · `BE_XMLStripNodes` · `BE_XMLStripInvalidCharacters` · `BE_JSON_ArraySize` · `BE_JSON_jq` *(v5.0+, no Win/iOS)* | `references/xml-xslt-json.md` |
| Zip & Gzip | `BE_Zip` · `BE_Unzip` · `BE_Gzip` · `BE_UnGzip` | `references/zip-gzip.md` |
| Constants | `BE_ButtonOK` · `BE_ButtonCancel` · `BE_ButtonAlternate` · `BE_FileTypeAll` · `BE_FileTypeFile` · `BE_FileTypeFolder` · `BE_EncodingHex` · `BE_EncodingBase64` · `BE_MessageDigestAlgorithmMD5/SHA256/MDC2/SHA/SHA1/SHA224/SHA384/SHA512` · `BE_ScriptControlHalt` · `BE_ScriptControlExit` · `BE_ScriptControlResume` · `BE_ScriptControlPause` *(v5.0+)* | `references/constants.md` |
| Version Compatibility | Breaking changes in v4.0.2 and v5.0.0, full rename tables, migration guide | `references/version-compatibility.md` |
| Deprecated | `BE_Base64_Encode/Decode` · `BE_HMAC` · `BE_JSON_Encode` · `BE_JSONPath` · `BE_ExecuteShellCommand` · `BE_FileMaker_Fields/Tables` · `BE_XeroSetTokens` | `references/deprecated.md` |
