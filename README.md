# BaseElements Plugin — Agent Skill

[![License](https://img.shields.io/badge/license-MIT-black)](./LICENSE)
[![Agent Skills Spec](https://img.shields.io/badge/Agent_Skills_Spec-compliant-blue)](https://agentskills.io/specification)
[![AgentVerus](https://agentverus.ai/api/v1/skill/25e4bfd7-988a-4258-8a8a-9ca587a17fc6/badge)](https://agentverus.ai/skill/25e4bfd7-988a-4258-8a8a-9ca587a17fc6)

An [Agent Skills](https://agentskills.io) skill that gives any skill-aware agent — Claude, Devin, Cursor, or similar — deep, accurate knowledge of the [BaseElements Plugin](https://baseelementsplugin.com) for FileMaker, covering all 100+ `BE_` functions across HTTP, file I/O, encryption, XML/XSLT, PDF, SMTP, zip/gzip, regex, JavaScript evaluation, and more.

This repository contains **both** the source documentation (`Functions/`) and the distributable skill package (`baseelements-plugin/`). Install the package, contribute to the sources.

---

## Table of Contents

- [What This Project Provides](#what-this-project-provides)
- [Repository Layout](#repository-layout)
- [Quick Start — Install the Skill](#quick-start--install-the-skill)
- [How the Skill Works](#how-the-skill-works)
- [Function Categories at a Glance](#function-categories-at-a-glance)
- [The Source Documentation](#the-source-documentation)
- [Updating the Skill](#updating-the-skill)
- [Contributing](#contributing)
- [Project Conventions](#project-conventions)
- [Spec Conformance](#spec-conformance)
- [License & Attribution](#license--attribution)

---

## What This Project Provides

When the skill is active, a skill-aware agent automatically understands:

- **Which function to use** — a categorized index of every active `BE_` function with one-line descriptions.
- **How to call it correctly** — exact signatures with required and optional parameters.
- **Critical cross-cutting patterns** — error-capture timing, HTTP/curl workflow, OS path conventions, array handle semantics, SMTP setup, and version-compatibility pitfalls. These are the things an agent cannot derive from any single function's documentation.
- **What to avoid** — deprecated functions and their native FileMaker replacements.
- **Full details on demand** — version history, platform compatibility tables, and real example code, loaded per function family only when the agent needs them.

The result: the agent writes correct, idiomatic BaseElements Plugin code the first time, without hallucinating signatures or forgetting `BE_GetLastError`.

---

## Repository Layout

```
baseelements-skill/
├── README.md                        ← you are here
├── CLAUDE.md                        ← project rules for AI agents working in this repo
├── LICENSE                          ← MIT
├── Functions.md                     ← original function index (source of truth)
├── Functions/                       ← 126 individual function documentation files
│   ├── BE_*.md                      ← one file per active function
│   ├── Constants.md                 ← plugin constants reference
│   └── Deprecated/                  ← removed/superseded functions
│       └── BE_*.md
└── baseelements-plugin/             ← distributable skill package (install this)
    ├── SKILL.md                     ← skill entry point (lean, ~110 lines)
    ├── README.md                    ← user-facing install and usage guide
    └── references/                  ← 24 reference files, one per function family
        ├── arrays.md
        ├── clipboard.md
        ├── constants.md
        ├── containers.md
        ├── data-manipulation.md
        ├── deprecated.md
        ├── dialogs.md
        ├── encoding-encryption.md
        ├── error-checking.md
        ├── filemaker-sql.md
        ├── files-folders.md
        ├── http-urls.md
        ├── miscellaneous.md
        ├── pdf.md
        ├── preferences.md
        ├── regular-expression.md
        ├── scripts.md
        ├── smtp-email.md
        ├── time.md
        ├── value-lists.md
        ├── vectors.md
        ├── version-compatibility.md
        ├── xml-xslt-json.md
        └── zip-gzip.md
```

Two layers, one source of truth:

| Layer | Path | Purpose |
|-------|------|---------|
| **Source** | `Functions/` + `Functions.md` | Authoritative per-function documentation, edited by humans. |
| **Package** | `baseelements-plugin/` | The installable skill, generated/curated from the source. |

---

## Quick Start — Install the Skill

You only need the `baseelements-plugin/` directory. The recommended approach is to install once into a shared `.agents/skills/` folder, then symlink from there into each agent's own skills directory. This way a single source copy feeds every agent you use.

### Step 1 — Install into a shared `.agents/skills/` folder

**User-level** (available across all your projects):

```bash
mkdir -p ~/.agents/skills
ln -s "$(pwd)/baseelements-plugin" ~/.agents/skills/baseelements-plugin
# or copy, if you don't need live edits:
# cp -r baseelements-plugin ~/.agents/skills/baseelements-plugin
```

**Project-level** (available only in one project):

```bash
mkdir -p /your/project/.agents/skills
ln -s "$(pwd)/baseelements-plugin" /your/project/.agents/skills/baseelements-plugin
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

Repeat for any other skill-aware agent. The symlink means a single update to the `.agents/skills/` copy (or the original repo, if you symlinked that) propagates to every agent immediately.

### Verify

Ask your agent *"What skills do you have?"* or run the agent's skill-listing command (e.g. `/skills` in Claude Code) — `baseelements-plugin` should appear.

**Prerequisites:** the [BaseElements Plugin](https://baseelementsplugin.com) itself must be installed on every FileMaker client or server where your solution runs. The skill teaches the agent how to write the calculations; the plugin executes them.

---

## How the Skill Works

The skill uses **progressive disclosure** to keep context lean:

1. **`SKILL.md`** is always loaded when the skill triggers. It contains the plugin overview and the five critical patterns agents must know before touching any `BE_` function:
   - Error handling — capture `BE_GetLastError` *immediately* after each call.
   - HTTP — configure options/headers *before*, inspect response/trace *after*.
   - Paths — plugin paths are native OS paths, not FileMaker `file:` paths.
   - Arrays — `BE_ArraySetFromValueList` returns an integer *handle*, not the data.
   - SMTP — configure server, then send.
   - Version compatibility — check the installed version before relying on v5.0+ features.

2. **`references/*.md`** files are loaded by the agent only when it needs the full documentation for a specific function family. Each file contains signatures, parameter descriptions, version history, compatibility tables, and example code — faithfully copied from the corresponding `Functions/BE_*.md` source files.

This keeps the always-on footprint small (~110 lines) while still giving the agent access to the complete 100+ function library on demand.

---

## Function Categories at a Glance

The skill organizes the 100+ `BE_` functions into 24 reference families:

| Category | Highlights |
|----------|-----------|
| Arrays | `BE_ArraySetFromValueList` · `BE_ArrayGetValue` · `BE_ArrayFind` |
| Clipboard | `BE_ClipboardGetText` · `BE_ClipboardSetFile` |
| Containers | `BE_ContainerCompress` · `BE_ConvertContainer` · `BE_JPEGRecompress` |
| Data Manipulation | `BE_StackPush/Pop` · `BE_VariableSet/Get` · `BE_TextExtractWords` |
| Dialogs | `BE_DialogDisplay` · `BE_FileSelectDialog` · `BE_FolderSelectDialog` |
| Encoding & Encryption | `BE_Encrypt_AES` · `BE_CipherEncrypt` · `BE_MessageDigest` · `BE_SignatureGenerateRSA` |
| Error Checking | `BE_GetLastError` · `BE_GetLastDDLError` · `BE_CurlTrace` |
| Files & Folders | `BE_FileExists` · `BE_FileReadText` · `BE_FileListFolder` · `BE_FTP_Upload` |
| FileMaker SQL | `BE_FileMakerSQL` |
| HTTP & URLs | `BE_HTTP_GET/POST/PATCH/PUT/DELETE` · `BE_CurlSetOption` · `BE_OpenURL` |
| Miscellaneous | `BE_ExecuteSystemCommand` · `BE_EvaluateJavaScript` · `BE_Version` |
| PDF | `BE_PDFPageCount` · `BE_PDFAppend` · `BE_PDFGetPages` |
| Preferences | `BE_SetPreference` · `BE_PreferenceGet` · `BE_PreferenceDelete` |
| Regular Expression | `BE_RegularExpression` |
| Scripts | `BE_ScriptExecute` · `BE_ScriptStepInstall/Perform/Remove` |
| SMTP Email | `BE_SMTPServer` · `BE_SMTPSend` · `BE_SMTPAddAttachment` |
| Time | `BE_TimeCurrentMilliseconds` · `BE_TimeUTCMilliseconds` · `BE_TimeZoneOffset` |
| Value Lists | `BE_ValuesSort` · `BE_ValuesUnique` · `BE_ValuesFilterOut` |
| Vectors | `BE_VectorDotProduct` · `BE_VectorEuclideanDistance` |
| XML, XSLT & JSON | `BE_XPath` · `BE_XSLT_Apply` · `BE_XMLParse` · `BE_JSON_jq` |
| Zip & Gzip | `BE_Zip` · `BE_Unzip` · `BE_Gzip` · `BE_UnGzip` |
| Constants | `BE_ButtonOK/Cancel/Alternate` · `BE_FileType*` · `BE_ScriptControl*` |
| Version Compatibility | v4.0.2 and v5.0.0 breaking-change rename tables and migration guide |
| Deprecated | `BE_Base64_Encode/Decode` · `BE_HMAC` · `BE_JSONPath` · `BE_ExecuteShellCommand` · and others |

See [`baseelements-plugin/SKILL.md`](baseelements-plugin/SKILL.md) for the complete function-to-reference mapping.

---

## The Source Documentation

Each `Functions/BE_FunctionName.md` file is the authoritative documentation for one plugin function, in this format:

```
## BE_FunctionName

    BE_FunctionName ( param1 ; param2 ; { optional } )

**Description**        — what the function does
**Parameters**         — each parameter explained
**Keywords**           — search terms
**Version History**    — when introduced/changed
**Notes**              — caveats, links, cross-references
**Compatibility**      — platform table (Mac/Win/FMS/iOS/Linux)
**Example Code**       — usage examples
```

- `Functions/Constants.md` lists all named plugin constants and their values.
- `Functions/Deprecated/` holds functions that have been removed or superseded by native FileMaker functions. **Do not use these in new solutions.**
- `Functions.md` is the original function index and the source of truth for what exists.

---

## Updating the Skill

When a new plugin version adds or changes functions:

1. **Update the source.** Edit or add the relevant file(s) in `Functions/`.
2. **Update the package.** Edit the corresponding `baseelements-plugin/references/*.md` file so it reflects the change. The reference files are the agent-facing surface; they must stay in sync with the source.
3. **New category?** Create a new `baseelements-plugin/references/category-name.md` and add a row to the category table in `baseelements-plugin/SKILL.md`.
4. **Deprecated a function?** Move its source file to `Functions/Deprecated/` and update `baseelements-plugin/references/deprecated.md`.
5. **Verify.** Re-read the affected `references/*.md` file against its `Functions/BE_*.md` source to confirm signatures, version history, and compatibility tables match exactly.

See [`CLAUDE.md`](CLAUDE.md) for the full set of project rules that AI agents should follow when working in this repository.

---

## Contributing

Contributions are welcome — particularly:

- Keeping the documentation in sync with new BaseElements Plugin releases.
- Fixing inaccuracies in signatures, parameters, or compatibility tables.
- Improving example code in the source files.

To contribute:

1. Fork the repository and create a branch from `develop`.
2. Make your changes following the conventions in [`CLAUDE.md`](CLAUDE.md) and the [Project Conventions](#project-conventions) below.
3. Ensure the `Functions/` source and the `baseelements-plugin/references/` package stay in sync — a change to one without the other is incomplete.
4. Open a pull request describing what changed and which plugin version it relates to.

---

## Project Conventions

- **Source of truth lives in `Functions/`.** The `baseelements-plugin/references/` files are a curated projection of that source for agent consumption. When in doubt, fix the source first, then propagate.
- **Do not invent functions.** If a function is not in `Functions.md`, it does not exist in this skill. Add new functions only when they appear in an official BaseElements Plugin release.
- **Preserve the source format.** The `## BE_FunctionName` / `**Field**` structure of `Functions/BE_*.md` files is intentional and parsed by humans and tooling alike.
- **Keep `SKILL.md` lean.** It is always loaded into context. Add new always-on guidance sparingly; prefer a new `references/` file for anything that only matters within one function family.
- **Mark version-gated features.** Functions or constants introduced in v5.0.0 or later should be tagged `*(v5.0+)*` in `SKILL.md` and the relevant reference file, so agents can guard usage with `BE_VersionAutoUpdate`.
- **No emojis** in documentation files unless explicitly requested.

---

## Spec Conformance

The `baseelements-plugin/` package conforms to the [Agent Skills format specification](https://agentskills.io/specification) (originally developed by Anthropic, now maintained as an open standard at [agentskills.io](https://agentskills.io)).

Conformance is verified with the official [`skills-ref`](https://github.com/agentskills/agentskills/tree/main/skills-ref) validator:

```bash
npx skills-ref@0.1.5 validate ./baseelements-plugin
# → Valid skill: ./baseelements-plugin
```

This checks that `SKILL.md` frontmatter satisfies the spec's required-field and naming constraints (`name` ≤ 64 chars, lowercase/digits/hyphens only, matches the parent directory; `description` 1–1024 chars). Re-run this command after any structural change to the skill package.

> Note: the Agent Skills spec does **not** issue an official conformance badge. The spec-conformance badge above is a self-asserted claim backed by a passing `skills-ref validate` run. There is no central registry that certifies spec conformance.

### Third-Party Validation

#### AgentVerus — Security Scan (certified)

The skill was scanned by [AgentVerus](https://agentverus.ai) and received a **certified** trust badge with a score of **96/100**.

- **Report:** <https://agentverus.ai/skill/25e4bfd7-988a-4258-8a8a-9ca587a17fc6>
- **Badge:** <https://agentverus.ai/api/v1/skill/25e4bfd7-988a-4258-8a8a-9ca587a17fc6/badge>
- **Scanned:** 2026-07-03 · Scanner v0.8.1

| Category | Score | Weight | Findings |
|----------|-------|--------|----------|
| Injection | 100 | 0.25 | No injection patterns detected |
| Dependencies | 100 | 0.15 | No dependency concerns |
| Behavioral | 100 | 0.15 | No behavioral risk concerns |
| Code safety | 100 | 0.15 | 5 code blocks scanned, no dangerous patterns |
| Permissions | 80 | 0.20 | 3 medium: inferred file-read, network, and doc-ingestion capabilities not declared in frontmatter (false positives for a reference-only skill — the skill documents `BE_` functions but does not execute them) |
| Content | 95 | 0.10 | 2 info (positive): safety boundaries defined, error-handling instructions present |

The permission findings are expected for a documentation-only skill: the scanner sees the words `curl`, `references/`, etc. in the instructional text and infers runtime capabilities, but this skill only *describes* plugin functions — it does not call them. The `SKILL.md` includes an explicit **Safety Boundaries** section declaring what the skill must not do (no code execution, no filesystem access, no network requests, no credential access). Re-scan after any `SKILL.md` change:

```bash
# Option A — scan from GitHub (after pushing to main):
curl -X POST https://agentverus.ai/api/v1/skill/scan \
  -H "Content-Type: application/json" \
  -d '{"url":"https://raw.githubusercontent.com/fsans/baseelements-skill/main/baseelements-plugin/SKILL.md"}'

# Option B — scan pasted content directly (no push required):
python3 -c "
import json, urllib.request
content = open('baseelements-plugin/SKILL.md').read()
data = json.dumps({'content': content}).encode()
req = urllib.request.Request('https://agentverus.ai/api/v1/skill/scan', data=data, headers={'Content-Type': 'application/json'})
print(json.dumps(json.loads(urllib.request.urlopen(req).read()), indent=2))
"
```

#### SkillMill — Not yet submitted

[SkillMill](https://skillmill.ai) certifies skills for catalog publication (spec compliance + safety scan + quality grading A–F). Their REST API requires a Team or Enterprise subscription; the free-tier submission path is the web form at [skillmill.ai/submit](https://skillmill.ai/submit), which requires manual sign-in. To submit:

1. Go to <https://skillmill.ai/submit>
2. Enter the GitHub URL: `https://github.com/fsans/baseelements-skill`
3. Review the analysis (spec compliance, safety scan, quality grade)
4. Add contact info and submit for review

Once approved, a SkillMill certification badge can be added here.

---

## License & Attribution

This project is licensed under the [MIT License](LICENSE).

The **BaseElements Plugin** itself is developed and maintained by [Goya](https://baseelementsplugin.com). It is free to use and open source. Plugin downloads, release notes, and source code are available at the project site.

This skill is an **unofficial community resource** derived from the plugin's own documentation. It is not affiliated with or endorsed by Goya. All `BE_` function names, signatures, and behavior descriptions are properties of the BaseElements Plugin project.
