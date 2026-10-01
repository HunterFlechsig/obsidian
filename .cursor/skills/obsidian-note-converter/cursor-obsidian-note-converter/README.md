# Cursor Obsidian Note Converter

This is the vault-native version of the Obsidian note converter. It is designed for Cursor or another coding agent opened directly on an Obsidian vault.

Unlike the web-chat version, this skill can search the vault itself. It can find existing notes and aliases, inspect related notes, discover supporting papers in `__Support/`, reuse existing figures, update matching notes in place, and validate links after writing.

## Install

Recommended location:

```text
<your-vault>/.cursor/skills/obsidian-note-converter/
```

Place `SKILL.md` and the `references/` directory there. If your Cursor version uses a different project-skill mechanism, keep the same files together and point the agent at `SKILL.md`.

## Normal use

Open the Obsidian vault as the Cursor workspace and ask things such as:

```text
Convert these handwritten notes.
```

```text
Convert Lecture 8 from __Support/Handwritten/CSE 575/Lecture 08/.
```

```text
Update my ConvNeXt note from these new annotations.
```

The default mode edits the vault directly and reports meaningful changes afterward.

## Other modes

**Plan only** inspects the vault and proposes changes without writing.

**Focused** restricts the task to the explicitly named files/notes and avoids incidental cleanup.

**Audit** inspects organization and integrity without changing anything. Cleanup is a separate approved step.

## Safety model

Normal note creation, clearly matching note updates, links, properties, sources, and figure extraction do not require approval. Moving, renaming, deleting, merging established notes, broad folder restructuring, and bulk remediation do.

The agent treats source PDFs and handwritten originals as read-only during conversion.

## Organization

The skill follows existing clear vault conventions first. Its fallback model is domain-first, shallow folders for knowledge, course folders for course artifacts, and the existing `__Support/` hierarchy for papers, images, slides, handwritten material, and other source assets.

The core distinction is:

- Folders = broad physical organization
- Properties = structured filtering and Obsidian Bases
- `[[wikilinks]]` = conceptual relationships

See the human-readable references for the full rationale and extension guidance.
