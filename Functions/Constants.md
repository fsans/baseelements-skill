## Constants used and available in the BE Plugin


| Name | Value | Version | Description |
|-----------|-----------|-----------|-----------|
| &nbsp; | | | |
| **BE_ButtonAlternate** | 3 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the alternateButton option. |
| **BE_ButtonCancel** | 2 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the cancelButton option. |
| **BE_ButtonOK** | 1 | 1.0.0 | Matches the value returned by the *BE_DialogDisplay* function when the user clicks the alternateButton option. |
| &nbsp; | | | |
| **BE_FileTypeAll** | 0 | 1.1.0 | This function is a type constant for use with the *BE_FileListFolder* function. |
| **BE_FileTypeFile** | 1 | 4.0.2 | This function is a type constant for use with the *BE_FileListFolder* function. Renamed from `BE_FileType_File` in v4.0.2. |
| **BE_FileTypeFolder** | 2 | 1.1.0 | This function is a type constant for use with the *BE_FileListFolder* function. |
| &nbsp; | | | |
| **BE_EncodingBase64** | 2 | 1.2.0 | Used in the *BE_MessageDigest* function as the encoding parameter. |
| **BE_EncodingHex** | 1 | 1.2.0 | Used in the *BE_MessageDigest* function as the encoding parameter. |
| &nbsp; | | | |
| **BE_MessageDigestAlgorithmMD5** | 1 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA256** | 2 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. The default algorithm. |
| **BE_MessageDigestAlgorithmMDC2** | 4 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA** | 6 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA1** | 7 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA224** | 8 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA384** | 9 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| **BE_MessageDigestAlgorithmSHA512** | 10 | 1.2.0 | This function is a type constant for use with the *BE_MessageDigest* function. |
| &nbsp; | | | |
| **BE_ScriptControlHalt** | 0 | 5.0.0 | Halts the current script before running the called script. Used with the *scriptControl* parameter of *BE_ScriptExecute*. |
| **BE_ScriptControlExit** | 1 | 5.0.0 | Exits the current script before running the called script. Used with the *scriptControl* parameter of *BE_ScriptExecute*. |
| **BE_ScriptControlResume** | 2 | 5.0.0 | Resumes the current script after the called script completes. Used with the *scriptControl* parameter of *BE_ScriptExecute*. |
| **BE_ScriptControlPause** | 3 | 5.0.0 | Pauses the current script while the called script runs. Used with the *scriptControl* parameter of *BE_ScriptExecute*. This is the default when *scriptControl* is not set or is invalid. |
| &nbsp; | | | |
