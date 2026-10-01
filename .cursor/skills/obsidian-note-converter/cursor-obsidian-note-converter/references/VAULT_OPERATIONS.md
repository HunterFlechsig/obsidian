# Vault Operations

This document explains how the Cursor-native converter should interact with an existing Obsidian vault.

## Why direct vault access matters

The web version of the converter needs an exported vault index to know which notes exist. Cursor can inspect the actual vault, so it should search before creating notes, use existing canonical names and aliases, read relevant related notes, and update clearly matching notes in place.

Direct access also creates risk. The converter therefore distinguishes normal additive work from destructive or broad organizational work.

## Retrieval strategy

Do not read the whole vault for every conversion. Search in stages:

1. filenames and aliases
2. frontmatter metadata
3. links/backlinks and source references
4. content search
5. full reads of the strongest matches

Stop when more retrieval is unlikely to change the decision.

This same principle applies to supporting documents: locate likely sources first, then inspect only the pages/sections/figures needed.

## Existing-note behavior

When a clear existing note matches the concept, update it rather than creating a duplicate. Read the note first. Preserve useful content and custom sections, integrate the new information, update metadata where appropriate, and validate the result.

When two existing notes may be duplicates but the match is not certain, do not choose one silently. Surface the ambiguity.

## Safe automatic operations

Normal conversion may create notes, update clearly matching notes, add properties and links, add sources, extract figures, correct clear factual/notation errors, and improve structure inside the note being processed.

## Approval-required operations

Require approval for deleting files, merging established notes, renaming/moving established notes or sources, removing substantial content, reorganizing folders, broad remediation, or modifying many unrelated notes.

This boundary exists because those operations can affect backlinks, external references, history, and user organization beyond the immediate conversion.

## Misplaced notes

If a note is clearly in an inconsistent folder, continue working with it in place and recommend the better destination. Moving it is a separate approved action.

## Git

When the vault already uses Git, status/diff/history can be useful safety tools. The skill may inspect them. It must not initialize Git, commit, push, reset, or alter history without explicit instruction.

Do not create backup copies throughout the vault by default.

## Obsidian configuration

`.obsidian/` may be inspected read-only when needed to understand attachment behavior or other relevant conventions. Normal note conversion never changes Obsidian settings.

Existing `.base` files may be inspected to understand which properties the user's Bases consume. They should not be modified unless explicitly requested.

## Batch work

Large conversions should be incremental. Process one coherent lecture/source set, validate its notes and assets, then continue. If one source is blocked by ambiguity, continue independent clear work and report the blocked item.

A source appearing in a note's `sources` property proves that it contributed to that note, not that every topic in the source has already been converted.

## Validation

After writes, check the filesystem-facing aspects that a web converter cannot:

- intended existing-note links resolve
- source files exist
- embedded assets exist
- duplicate filenames were not introduced
- frontmatter remains parseable
- only intended files changed

Intentional unresolved concept links are valid and should not be "fixed" merely because no file exists yet.
