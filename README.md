# BNI Format Specification

**BNI (Blizzard Notation INI)** is a strictly line-based configuration format used in classic Blizzard games such as
*Warcraft III*. While inspired by the traditional INI format, BNI introduces relaxed syntax rules and game-specific
conventions tailored for structured game data.

## Tools

Related tools that support or build on the BNI format:

- [tree-sitter-bni](https://github.com/WarRaft/tree-sitter-bni) — a Tree-sitter grammar for the BNI format, enabling
  syntax-aware parsing, highlighting, and editor support.
- [JASS-Tree-sitter-Rust](https://github.com/WarRaft/JASS-Tree-sitter-Rust) — a standalone binary providing
  Tree-sitter-powered language support for JASS and BNI via LSP, integrated with VSCode and usable independently.

These tools are maintained in separate repositories and can be used independently or together.

## Key Features

- Section headers using `[SectionName]` syntax.
- Every meaningful line must contain an `=`.
- Keys and/or values may be omitted, but the equals sign is required.
- Values are separated by commas and parsed independently.
- Comments start with `//` and can follow any content.
- Empty lines are allowed anywhere.
- Parsing is performed **line-by-line**, no multi-line constructs.

## Minimal Example

```ini
// a comment

[Units]

unit1="Orc",100,strong
unit2="Elf",80,agile
= value that includes all spaces exactly as written
= value, list, separated, by, comma
="If you need a comma, use quotes."
key_only=
```

## Syntax Rules

BNI is parsed line-by-line. Each line must match one of the following:

- Section header: `[Name]`
- Comment line: starts with `//`
- Key-value line: must contain `=`, with optional key or value
- Empty line: ignored

The equals sign is **mandatory** for all value lines. Even loose values must be written as:

```ini
=some_value
```

## Value Types

Each value is parsed as a raw token between commas. Whitespace around values is **significant**.

### Numbers

- `42`, `-5`, `3.14`, `1e6`

### Quoted Strings

- May use single `'...'` or double `"..."` quotes
- Everything up to the closing quote is included
- Quotes cannot be escaped

```ini
description = "Basic grunt unit"
flavor = 'Strong and brave'
```

### Unquoted Strings

- Any sequence not containing a comma or newline
- Quotes are not required
- Leading/trailing spaces are **not trimmed**
- A leading space before a quote prevents it from being parsed as a quoted string
- Commonly used for: paths, IDs, flags, literals

Examples:

```ini
model=textures/unit.blp,textures/unit1.blp
icon=BTNHero
sound=Sound\Footstep.wav
path="starts with space and quote"
```

In the last line, the value is a literal space followed by a quote — **not** a quoted string.

### Mixed Lists

Multiple values may appear in one line:

```ini
unit="Orc",100,strong,visible
```

## Goals of This Repository

- Define a precise grammar for BNI
- Provide Tree-sitter and other parser implementations
- Build tools for linting, formatting, and transformation
- Serve as reference for modding tools and editors

<p align="center">
  <img src="https://raw.githubusercontent.com/WarRaft/BNI/refs/heads/main/preview/logo.png?1" alt=""/>
</p>
