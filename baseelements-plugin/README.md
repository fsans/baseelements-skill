# BaseElements Plugin — Agent Skill

An [Agent Skills](https://agentskills.io) skill that gives any skill-aware agent — Claude, Devin, Cursor, or similar — deep, accurate knowledge of the [BaseElements Plugin](https://baseelementsplugin.com) for FileMaker, including function signatures, parameters, version history, compatibility, and working examples across all 100+ `BE_` functions.

## What This Skill Does

When this skill is active, skill-aware agents automatically understand:

- **Which function to use** — a full categorized index of all active `BE_` functions with one-line descriptions
- **How to call it correctly** — exact signatures with required and optional parameters
- **Critical patterns** — error handling timing, HTTP/curl workflow, OS path conventions, array handle semantics, SMTP setup
- **What to avoid** — deprecated functions and their native FileMaker replacements
- **Full details on demand** — version history, platform compatibility, and real example code, loaded per function family only when needed

## Prerequisites

- [BaseElements Plugin](https://baseelementsplugin.com) installed on every FileMaker client or server where your solution runs
- Any skill-aware agent (Claude, Devin, Cursor, or similar) that supports the [Agent Skills](https://agentskills.io) format

## Installation

The recommended approach is to install once into a shared `.agents/skills/` folder, then symlink from there into each agent's own skills directory. A single source copy then feeds every agent you use.

### Step 1 — Install into a shared `.agents/skills/` folder

**User-level** (available across all your projects):

```bash
mkdir -p ~/.agents/skills
ln -s /path/to/baseelements-plugin ~/.agents/skills/baseelements-plugin
# or copy, if you don't need live edits:
# cp -r baseelements-plugin ~/.agents/skills/baseelements-plugin
```

**Project-level** (available only in one project):

```bash
mkdir -p /your/project/.agents/skills
ln -s /path/to/baseelements-plugin /your/project/.agents/skills/baseelements-plugin
```

### Step 2 — Symlink from `.agents/skills/` into each agent's skills directory

Different agents look in different locations. Symlink the shared copy into whichever ones you use:

```bash
# Claude Code (user-level)
mkdir -p ~/.claude/skills
ln -s ~/.agents/skills/baseelements-plugin ~/.claude/skills/baseelements-plugin

# Claude Code (project-level)
mkdir -p /your/project/.claude/skills
ln -s /your/project/.agents/skills/baseelements-plugin /your/project/.claude/skills/baseelements-plugin

# Cursor
mkdir -p ~/.cursor/skills
ln -s ~/.agents/skills/baseelements-plugin ~/.cursor/skills/baseelements-plugin

# Devin
mkdir -p ~/.config/devin/skills
ln -s ~/.agents/skills/baseelements-plugin ~/.config/devin/skills/baseelements-plugin
```

Repeat for any other skill-aware agent. The symlink means a single update to the `.agents/skills/` copy propagates to every agent immediately.

### Verify installation

Ask your agent *"What skills do you have?"* or run the agent's skill-listing command (e.g. `/skills` in Claude Code) — you should see `baseelements-plugin` listed.

## Trigger Phrases

The skill activates automatically when you mention:

- `BaseElements` or `BE_` (any function name)
- FileMaker plugin operations: HTTP requests, file I/O, encryption, XML/XSLT, PDF, SMTP, zip/gzip, regex, JavaScript evaluation

Example prompts that trigger the skill:

- *"Write a FileMaker script that POSTs JSON to an API using BE_HTTP_POST"*
- *"How do I encrypt a field value with AES using the BaseElements plugin?"*
- *"List all files in a folder using BE functions and filter for .fmp12 files"*
- *"Set up SMTP email sending with an attachment"*

## Function Categories

| Category | Functions |
|----------|-----------|
| Arrays | `BE_ArraySetFromValueList` · `BE_ArrayGetValue` · `BE_ArrayChangeValue` · `BE_ArrayFind` · `BE_ArrayGetSize` · `BE_ArrayDelete` |
| Clipboard | `BE_ClipboardFormats` · `BE_ClipboardGetText` · `BE_ClipboardSetText` · `BE_ClipboardGetFile` · `BE_ClipboardSetFile` |
| Containers | `BE_ContainerCompress` · `BE_ContainerUncompress` · `BE_ContainerIsCompressed` · `BE_ContainerListTypes` · `BE_ContainerGetType` · `BE_ConvertContainer` · `BE_ExportFieldContents` · `BE_JPEGRecompress` |
| Data Manipulation | `BE_StackPush` · `BE_StackPop` · `BE_StackCount` · `BE_StackDelete` · `BE_VariableSet` · `BE_VariableGet` · `BE_TextExtractWords` |
| Dialogs | `BE_DialogDisplay` · `BE_DialogProgress` · `BE_DialogProgressUpdate` · `BE_FileSaveDialog` · `BE_FileSelectDialog` · `BE_FolderSelectDialog` |
| Encoding & Encryption | `BE_Encrypt_AES` · `BE_Decrypt_AES` · `BE_CipherEncrypt` · `BE_CipherDecrypt` · `BE_MessageDigest` · `BE_SignatureGenerateRSA` · `BE_SignatureVerifyRSA` · `BE_SetTextEncoding` · `BE_CreateKeyPair` · `BE_GetPublicKey` · `BE_EncryptWithKey` · `BE_DecryptWithKey` |
| Error Checking | `BE_GetLastError` · `BE_GetLastDDLError` · `BE_CurlTrace` · `BE_DebugInformation` |
| Files & Folders | `BE_FileExists` · `BE_FileReadText` · `BE_FileWriteText` · `BE_FileCopy` · `BE_FileMove` · `BE_FileDelete` · `BE_FileImport` · `BE_FileListFolder` · `BE_FileModificationTimestamp` · `BE_FileOpen` · `BE_FilePatternCount` · `BE_FileReplaceText` · `BE_FileSize` · `BE_FolderCreate` · `BE_FTP_Delete` · `BE_FTP_Upload` · `BE_FTP_UploadFile` |
| FileMaker SQL | `BE_FileMakerSQL` |
| HTTP & URLs | `BE_HTTP_GET` · `BE_HTTP_GET_File` · `BE_HTTP_POST` · `BE_HTTP_PATCH` · `BE_HTTP_PUTData` · `BE_HTTP_PUTFile` · `BE_HTTP_DELETE` · `BE_HTTP_SetCustomHeader` · `BE_HTTP_ResponseCode` · `BE_HTTP_ResponseHeaders` · `BE_HTTP_Set_Proxy` · `BE_CurlSetOption` · `BE_OpenURL` · `BE_CurlGetInfo` |
| Miscellaneous | `BE_ExecuteSystemCommand` · `BE_EvaluateJavaScript` · `BE_GetMachineName` · `BE_GetSystemDrive` · `BE_Pause` · `BE_Version` · `BE_VersionAutoUpdate` |
| PDF | `BE_PDFPageCount` · `BE_PDFAppend` · `BE_PDFGetPages` |
| Preferences | `BE_SetPreference` · `BE_PreferenceGet` · `BE_PreferenceDelete` |
| Regular Expression | `BE_RegularExpression` |
| Scripts | `BE_ScriptExecute` · `BE_ScriptStepInstall` · `BE_ScriptStepPerform` · `BE_ScriptStepRemove` |
| SMTP Email | `BE_SMTPServer` · `BE_SMTPSend` · `BE_SMTPAddAttachment` · `BE_SMTPSetHeader` |
| Time | `BE_TimeCurrentMilliseconds` · `BE_TimeUTCMilliseconds` · `BE_TimeZoneOffset` |
| Value Lists | `BE_ValuesSort` · `BE_ValuesUnique` · `BE_ValuesFilterOut` · `BE_ValuesContainsDuplicates` · `BE_ValuesTimesDuplicated` · `BE_ValuesTrim` |
| Vectors | `BE_VectorDotProduct` · `BE_VectorEuclideanDistance` |
| XML, XSLT & JSON | `BE_XPath` · `BE_XPathAll` · `BE_XSLT_ApplyInMemory` · `BE_XSLT_Apply` · `BE_XMLParse` · `BE_XMLValidate` · `BE_XMLTidy` · `BE_XML_Canonical` · `BE_XMLStripNodes` · `BE_XMLStripInvalidCharacters` · `BE_JSON_ArraySize` · `BE_JSON_jq` |
| Zip & Gzip | `BE_Zip` · `BE_Unzip` · `BE_Gzip` · `BE_UnGzip` |
| Constants | `BE_ButtonOK/Cancel/Alternate` · `BE_FileTypeAll/File/Folder` · `BE_EncodingHex/Base64` · `BE_MessageDigestAlgorithmMD5/MDC2` |
| Deprecated | `BE_Base64_Encode/Decode` · `BE_HMAC` · `BE_JSON_Encode` · `BE_JSONPath` · `BE_ExecuteShellCommand` · and others |

## How the Skill Is Structured

```
baseelements-plugin/
├── SKILL.md           ← loaded into context when skill triggers (~90 lines)
└── references/        ← 24 files loaded on demand, one per function family
    ├── http-urls.md
    ├── files-folders.md
    ├── encoding-encryption.md
    └── ...
```

`SKILL.md` is kept intentionally lean — it holds the critical cross-cutting patterns that agents cannot derive from any single function's documentation (error capture timing, HTTP multi-step workflow, path conventions, array handles). The bulk of the documentation lives in the `references/` files and is pulled into context only when the agent needs to work with that function family.

## About BaseElements Plugin

The BaseElements Plugin is developed and maintained by [Goya](https://baseelementsplugin.com). It is free to use and open source. Plugin downloads, release notes, and source code are available at the project site.

This skill is an unofficial community resource derived from the plugin's own documentation. It is not affiliated with or endorsed by Goya.

## Contributing / Updating

When a new plugin version is released, update the source files in the parent project's `Functions/` directory and regenerate the affected `references/*.md` files. See `CLAUDE.md` at the project root for the full update workflow.
