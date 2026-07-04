# Constants — BaseElements Plugin

These named constants are provided by the plugin for use as parameter values in specific functions.

## Dialog Button Constants

Used with the `BE_DialogDisplay` function to identify which button the user clicked.

| Name | Value | Version | Description |
|---|---|---|---|
| **BE_ButtonOK** | 1 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the OK button option. |
| **BE_ButtonCancel** | 2 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the cancelButton option. |
| **BE_ButtonAlternate** | 3 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the alternateButton option. |

## File Type Constants

Used with the `BE_FileListFolder` function to filter results by entry type.

| Name | Value | Version | Description |
|---|---|---|---|
| **BE_FileTypeAll** | 0 | 1.1.0 | Returns all entries (files and folders) when used with the *BE_FileListFolder* function. |
| **BE_FileTypeFile** | 1 | 4.0.2 | Returns only files when used with the *BE_FileListFolder* function. Renamed from `BE_FileType_File` in v4.0.2. |
| **BE_FileTypeFolder** | 2 | 1.1.0 | Returns only folders when used with the *BE_FileListFolder* function. |

## Encoding Constants

Used with the `BE_MessageDigest` function as the encoding parameter.

| Name | Value | Version | Description |
|---|---|---|---|
| **BE_EncodingHex** | 1 | 1.2.0 | Specifies hexadecimal encoding in the *BE_MessageDigest* function. |
| **BE_EncodingBase64** | 2 | 1.2.0 | Specifies Base64 encoding in the *BE_MessageDigest* function. |

## Message Digest Algorithm Constants

Used with the `BE_MessageDigest` function to select the hashing algorithm.

| Name | Value | Version | Description |
|---|---|---|---|
| **BE_MessageDigestAlgorithmMD5** | 1 | 1.2.0 | Selects the MD5 algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA256** | 2 | 1.2.0 | Selects the SHA-256 algorithm for use with the *BE_MessageDigest* function. This is the default algorithm. |
| **BE_MessageDigestAlgorithmMDC2** | 4 | 1.2.0 | Selects the MDC2 algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA** | 6 | 1.2.0 | Selects the SHA algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA1** | 7 | 1.2.0 | Selects the SHA-1 algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA224** | 8 | 1.2.0 | Selects the SHA-224 algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA384** | 9 | 1.2.0 | Selects the SHA-384 algorithm for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA512** | 10 | 1.2.0 | Selects the SHA-512 algorithm for use with the *BE_MessageDigest* function. |

## Script Control Constants

Used with the `scriptControl` parameter of `BE_ScriptExecute` to control how the called script is queued. Added in v5.0.0.

| Name | Value | Version | Description |
|---|---|---|---|
| **BE_ScriptControlHalt** | 0 | 5.0.0 | Halts the current script before running the called script. |
| **BE_ScriptControlExit** | 1 | 5.0.0 | Exits the current script before running the called script. |
| **BE_ScriptControlResume** | 2 | 5.0.0 | Resumes the current script after the called script completes. |
| **BE_ScriptControlPause** | 3 | 5.0.0 | Pauses the current script while the called script runs. This is the default when *scriptControl* is not set or is invalid. |
