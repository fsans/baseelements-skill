# BaseElements Plugin Skill — Project

This project produces an [Agent Skills](https://agentskills.io) skill for the [BaseElements Plugin](https://baseelementsplugin.com), a free open-source FileMaker plugin that extends FileMaker calculations with over 100 `BE_` functions for HTTP, file I/O, encryption, XML/XSLT, PDF, SMTP, zip, regex, and more. The skill works with any skill-aware agent — Claude, Devin, Cursor, or similar.

## Project Structure

```
baseelements-skill/
├── CLAUDE.md                        ← this file
├── Functions.md                     ← original function index (source of truth)
├── Functions/                       ← individual function documentation files
│   ├── BE_*.md                      ← one file per active function
│   ├── Constants.md                 ← plugin constants reference
│   └── Deprecated/                  ← removed/superseded functions
│       └── BE_*.md
└── baseelements-plugin/             ← distributable skill package (install this)
    ├── SKILL.md                     ← skill entry point (lean, ~125 lines)
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
        ├── xml-xslt-json.md
        ├── zip-gzip.md
        └── version-compatibility.md
```

## Source Files (`Functions/`)

Each `BE_FunctionName.md` file is the authoritative documentation for one plugin function. The format is:

```
## BE_FunctionName

    BE_FunctionName ( param1 ; param2 ; { optional } )

**Description** — what the function does
**Parameters** — each parameter explained
**Keywords** — search terms
**Version History** — when introduced/changed
**Notes** — caveats, links, cross-references
**Compatibility** — platform table (Mac/Win/FMS/iOS/Linux)
**Example Code** — usage examples
```

`Functions/Constants.md` lists all named plugin constants and their values.

`Functions/Deprecated/` holds functions that have been removed or superseded by native FileMaker functions. Do not use these in new solutions.

## Skill Architecture

The skill uses progressive disclosure:

1. **`SKILL.md`** — always loaded into context. Contains: plugin overview, 5 critical patterns agents must know (error capture timing, HTTP workflow, path conventions, array handles, SMTP setup), and a category index table that maps function names to reference files.

2. **`references/*.md`** — loaded by the agent only when needed for a specific function family. Each file contains the full documentation for all functions in that category: signatures, parameter descriptions, version history, compatibility tables, and example code, faithfully copied from the source `Functions/` files.

## Updating the Skill

When a new plugin version adds or changes functions:

1. Update or add the relevant file(s) in `Functions/`.
2. Update the corresponding `references/` file in `baseelements-plugin/references/` to reflect the change.
3. If a new function category is introduced, create a new `references/category-name.md` and add a row to the table in `SKILL.md`.
4. If a function is deprecated, move its source file to `Functions/Deprecated/` and update `references/deprecated.md`.

## Distributing the Skill

The `baseelements-plugin/` directory is the installable unit. See `baseelements-plugin/README.md` for installation instructions.
