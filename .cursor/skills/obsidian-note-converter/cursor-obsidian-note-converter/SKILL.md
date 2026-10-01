# Obsidian Note Converter - Cursor / Vault-Native

Use this skill to convert handwritten notes, rough notes, annotated documents, lectures, papers, and supporting material into a maintained Obsidian knowledge base. This version assumes the agent is operating inside, or has explicit access to, the Obsidian vault.

## Core goal

Turn source material into accurate, polished, connected Obsidian notes. Optimize for understanding and long-term retrieval, not literal transcription. Preserve the user's scope and emphasis, fill important gaps, correct clear errors, and integrate with the existing vault instead of creating isolated files.

## Instruction priority

When instructions conflict, use this order:
1. Explicit current user instruction.
2. Explicit processing annotations in the user's notes, such as `[FIGURE: 4]`, `[SPLIT NOTE]`, `[EXPAND]`, `[KEEP SHORT]`.
3. Applicable repository instructions such as `AGENTS.md` or Cursor rules.
4. Clear existing vault conventions, including relevant Bases.
5. Existing note structure when updating a note.
6. Rules in this skill.
7. General defaults.

For factual content, handwritten/rough notes define scope, not factual authority. Prefer supporting primary documents for verification, then consistent existing vault knowledge, established background knowledge, and external research only when needed. Correct clear factual errors rather than preserving them.

## Before editing

1. Locate the vault root. Prefer the workspace containing `.obsidian/`. If multiple vaults are plausible, ask which one. Do not guess.
2. Read applicable repository/Cursor instructions.
3. Treat the vault as the default filesystem boundary. Do not proactively search unrelated directories outside it unless the user points to them.
4. Inspect relevant Obsidian configuration read-only when it affects behavior, such as attachment settings. Never modify `.obsidian/` during normal conversion.
5. Search the vault before creating anything.

Use staged retrieval rather than reading the whole vault:
1. Search filenames and aliases.
2. Search frontmatter such as `topics`, `type`, `sources`, and course metadata.
3. Search wikilinks/backlinks and explicit source relationships.
4. Search contents for key terminology.
5. Read the strongest relevant matches until additional exploration is unlikely to change note boundaries, naming, linking, or duplication decisions.

For `__Support/`, similarly search filenames, metadata/title, indexed/extracted text, relevant pages/sections, and figures only as needed. Existing sources explicitly linked from a note have priority over inferred matches.

Subagents may be used for independent retrieval, source discovery, duplicate checks, figure inspection, or audits when available. Subagents should report findings; the primary agent owns edits and final validation.

## Source hierarchy

Use this practical hierarchy:
- User's handwritten/rough/annotated notes define what the conversion should cover and what the user emphasized.
- Supplied or discovered supporting documents verify and enrich the material.
- Existing vault notes preserve terminology, boundaries, relationships, and prior knowledge, but do not override stronger primary evidence when wrong.
- Established background knowledge fills small gaps.
- External research is a last resort for missing, uncertain, current, or verification-sensitive facts. Prefer original papers, official documentation, textbooks/academic sources, then reputable explanations.

Material external sources used substantially should be linked/cited in the note. Do not automatically download web pages. Treat source PDFs and handwritten originals as read-only during normal conversion.

## Determine note boundaries

One note should represent one independently useful unit of knowledge.

Do not split merely because a term could have its own page. Split when a topic is both independently useful and sufficiently developed. Keep each split note understandable on its own, but use `[[links]]` instead of duplicating substantial explanations.

A handwritten title wins when present. Otherwise infer the dominant concept. Supporting material helps interpret the notes but does not redefine their primary topic.

When material clearly belongs to a lecture or paper, preserve that context:
- A `paper` note represents the paper as a whole.
- A `lecture` note records what was taught and can link to reusable concept notes.
- Reusable concepts live independently when sufficiently developed.

Physical input boundaries do not determine conceptual boundaries. Multiple images/pages may be one source set; one page may produce multiple notes. Infer page order only when evidence is strong. Flag consequential uncertainty instead of guessing.

## Note types

Use a small controlled vocabulary:
- `concept`
- `lecture`
- `paper`
- `project`
- `reference`

Extend only when a genuinely recurring need appears.

## Standard note shell

Use a fixed shell with an adaptive body:

```markdown
---
type: concept
topics:
  - "[[Machine Learning]]"
status: complete
created: YYYY-MM-DD
updated: YYYY-MM-DD
aliases:
  - LDA
sources:
  - "[[Supporting Source.pdf]]"
---

# Linear Discriminant Analysis

> [!info] Supporting Material
> - [[Supporting Source.pdf]]

> [!summary] Summary
> 2-5 sentences explaining what the topic is and why it matters.

> [!important] Key Points
> - 3-7 high-value review points.

## Adaptive Body

Use headings appropriate to the material.

## Related Concepts

- [[Related Concept]] - concise relationship.
```

Rules:
- Omit optional properties that are unknown or irrelevant.
- `topics` are linked immediate organizational parents, not every ancestor.
- Body links represent conceptual relationships beyond organizational parents.
- Use properties for structured filtering/Bases, folders for broad physical organization, and `[[links]]` for knowledge relationships.
- `tags` are not required and should not duplicate `topics`.
- Entity-like properties such as `course` should use `[[wikilinks]]` when a corresponding note is meaningful.
- `sources` contains source files that materially contributed to the note, including retained handwritten source files.
- Preserve existing `created` when updating. Update `updated` only when content changes.
- `status` values: `draft`, `review`, `complete`. `complete` means no currently known unresolved conversion issue, not final forever. Set `review` when a meaningful ambiguity remains.
- Normalize aliases conservatively. Prefer canonical full titles with useful abbreviations as aliases, while keeping names commonly known by acronym/model name as canonical when appropriate.
- Existing Bases may provide strong evidence for expected properties. Inspect them when relevant but never modify Bases unless explicitly asked.

## Writing and enrichment

Write concise, polished explanatory notes. Preserve the user's information and intent, not the physical order or shorthand wording.

Fill understanding gaps, not space. Add definitions, intuition, intermediate reasoning, examples, comparisons, or prerequisites only when they materially improve understanding of the topics actually present.

Preserve worked examples and reasoning. Correct clear arithmetic, factual, syntax, or notation errors. Report meaningful corrections afterward.

Preserve useful custom sections such as `Questions for Professor` or `My Experiment Results`. The standard structure is a baseline, not a heading whitelist.

Use additional Obsidian callouts sparingly when semantically useful, such as `[!warning] Common Mistake`, `[!tip] Intuition`, or `[!example] Example`.

Handwritten emphasis matters. Circled, boxed, starred, or strongly emphasized material should influence Key Points. Arrows and spatial relationships may help reconstruct relationships.

Questions written by the user may become explanatory headings when useful.

## Wikilinks

Always build meaningful connections to overarching and related subjects.

Before creating a link or note, search the vault for existing canonical names and aliases. Prefer exact existing note titles. Use display aliases naturally in prose, e.g. `[[Linear Discriminant Analysis|LDA]]`.

Unresolved links are allowed when a concept clearly deserves a future note. Do not create empty placeholder notes merely because a link is unresolved. Create a missing parent/hub only when it would contain meaningful navigation or content.

When new notes are split from the same source, preserve their relationships with explicit links where useful.

Do not modify unrelated existing notes merely to create reciprocal links. Obsidian backlinks already provide reverse navigation.

## Existing notes

If a clearly matching existing note exists, read it and merge the new material into it automatically. Preserve useful existing content and custom sections. Bring the directly updated note toward the current structure when safe.

Do not create a duplicate note for the same concept. Ambiguous matches require a user decision.

Clear factual errors in an existing note may be corrected when evidence is strong; report the correction afterward.

Do not move or rename established notes automatically. If placement/naming is inconsistent, report the recommended change and continue editing in place unless approval is given.

## Folder organization

Prefer shallow, domain-first organization. Existing clear vault conventions override these defaults.

Fallback pattern:

```text
Vault/
├── Inbox/
├── Courses/
│   └── <Course>/
│       ├── Lectures/
│       └── Assignments/
├── Computer Vision/
│   ├── Concepts/
│   ├── Papers/
│   └── Projects/
├── Machine Learning/
│   ├── Concepts/
│   ├── Papers/
│   └── Projects/
├── Robotics/
│   ├── Concepts/
│   ├── Papers/
│   └── Projects/
└── __Support/
```

Use one primary physical domain for a note. Represent additional memberships through `topics`; never duplicate the same note across folders. Reusable knowledge should not be trapped under course folders merely because a course taught it. Use `Inbox/` only when placement is genuinely ambiguous.

The user's existing support-material root is `__Support/`. Preserve its existing subfolder structure. Search it before inventing new paths. Typical categories include Papers, Images, Slides, and Handwritten.

## Supporting documents and provenance

Every note using supporting material should show it near the top in the Supporting Material callout and include material sources in `sources`.

Use page/heading-specific Obsidian links when possible, e.g. `[[Paper.pdf#page=4]]`, for specific claims, equations, or figures. Use the general file link in Supporting Material.

When multiple sources exist, choose the strongest source for each purpose rather than treating all sources equally. Sources accumulate when they genuinely contribute; do not delete old valid sources merely because new ones are added.

Handwritten annotations on supporting documents count as user notes and receive extra interpretive weight.

## Equations

Convert equations to clean MathJax. Fix notation, malformed expressions, and obvious errors. Define unfamiliar variables and fill omitted reasoning when useful.

Validate obvious MathJax issues such as unmatched delimiters or malformed environments before finishing.

## Code

Convert handwritten or rough code into clean fenced code blocks with the appropriate language. Correct obvious syntax when intent is clear. Report corrections that change behavior rather than merely formatting/syntax.

Do not execute code snippets as part of normal note conversion.

## Figures and diagrams

Explicit figure requests in the user's notes override automatic figure selection. Recognize natural-language directives and conventions such as:
- `[FIGURE: 4]`
- `[SPLIT NOTE]`
- `[EXPAND]`
- `[KEEP SHORT]`

Processing annotations trigger behavior and are removed from the final note.

Figure priority:
1. Explicit requested figure.
2. Existing relevant figure already in `__Support/Images/`.
3. Relevant figure from a supporting document.
4. Clean Mermaid reconstruction of a simple flow/structure diagram when confident.
5. Preserved/cropped handwritten diagram when its drawing, layout, annotations, or uncertainty matter.
6. A new simple explanatory Mermaid diagram only when it materially improves understanding.

Before extracting, search for an existing reusable asset. New finalized images go under the existing `__Support/Images/` convention. Use descriptive collision-resistant filenames such as `ChestX-Det - Figure 4 - Relation Module.png`. Preserve original figure/table numbers.

For multi-panel figures, crop only the relevant panel when surrounding panels are unnecessary; otherwise preserve context.

Crop irrelevant whitespace and avoid unnecessarily huge assets while preserving readable labels/equations. Use temporary storage for intermediate extraction artifacts; never leave temporary files in the vault.

Use concise note-specific captions rather than copying long source captions. Attribute external figures with source and page when available. Handwritten diagrams get descriptive captions without fabricated citations.

Reconstruct simple tables as Markdown when reliable. Preserve complex/visual tables as images when reconstruction risks errors or destroys meaning.

Validate Mermaid syntax when practical.

## Source-file handling

Source PDFs and handwritten originals are read-only during conversion. Do not annotate, modify, compress, rename, or move them automatically.

If an existing source is poorly placed, recommend the appropriate `__Support/` destination but do not move it without approval. If explicitly asked to acquire/create a new source file, place it correctly from the start.

Suggest clean names for newly imported sources when useful, but preserve existing indexed filenames.

## Modes

### Default
Inspect relevant vault context, create/update notes and finalized assets directly, validate the result, then give a concise change report.

### Plan only
Perform read-only inspection and describe the proposed creates, updates, extractions, moves, or issues. Do not modify files.

### Focused
Only process the explicitly named notes/files/source set. Avoid incidental cleanup outside that scope.

### Audit
Read-only vault audit. Never modify files during the audit stage. See Audit rules below.

## Safety and approval boundary

No approval is normally required to:
- create a new note
- update a clearly matching note
- add or clean properties
- add links and sources
- correct a clear factual/notation error
- extract a new figure into `__Support/Images/`
- create a sufficiently developed new concept note
- make small structural improvements inside the note being processed

Require explicit approval before:
- deleting any file
- merging established notes
- renaming an established note
- moving an established note or source
- removing substantial existing content
- reorganizing folders
- bulk cleanup/remediation
- modifying many unrelated notes

Never automatically delete potentially unused images.

If Git is already configured, it may be used read-only for status/diff/history awareness. Never initialize Git, commit, push, reset, or alter history unless explicitly requested. Do not create `.bak` files by default.

## Batch conversion

Support folders and multi-source batches. Process one coherent source set at a time, validate it, then continue. Do not stop independent clear work because another source is ambiguous; complete what can be completed and report the blocked item.

Use `sources` relationships to help determine prior processing, but do not assume a source is fully processed merely because one note cites it. Compare identifiable topics in the source with notes that reference it when batch-processing "unprocessed" material.

## Audit mode

Audit is read-only and should identify practical issues such as:
- likely duplicate notes
- broken source/image links
- unresolved links that resemble renamed/typo targets
- intentional unresolved concept links versus actual breakage
- orphan notes
- missing/inconsistent properties
- folder inconsistencies
- source organization issues
- naming/alias inconsistencies
- potentially unused assets

Classify findings by practical severity, e.g. High/Medium/Low, based on navigation/data-integrity risk rather than subjective note quality.

Audit has two stages:
1. Read-only findings.
2. Remediation only after the user approves specific categories/items.

Do not create a persistent audit note unless requested.

## Validation after writes

After creating/updating files, perform a lightweight validation pass:
- existing-target links intended to resolve do resolve
- source PDFs and embedded images exist
- no accidental duplicate filename was created
- frontmatter remains valid
- MathJax delimiters/environments are structurally sound
- code fences are closed
- Mermaid is syntactically plausible/validated when practical
- intentional unresolved conceptual links remain allowed

Inspect Git diff when available and useful, without altering Git state.

## Change report

In default mode, report only meaningful actions/findings. Omit unchanged files.

Useful categories:
- Created
- Updated
- Assets added
- Sources used
- Corrections
- Organization recommendations
- Attention needed

Do not create a permanent change-log file unless requested.

## Invocation examples

The user may simply say:
- `Convert these handwritten notes.`
- `Convert Lecture 8.`
- `Convert everything in __Support/Handwritten/CSE 575/Lecture 08/.`
- `Update my ConvNeXt notes from these annotations.`
- `Convert this, focused mode.`
- `Inspect this conversion but plan only.`
- `Audit my vault.`
- `Audit my Computer Vision notes.`
- `Find duplicate notes, but don't change anything.`

When the request is clear, work without interrogating the user. Ask only when an unresolved decision would materially change the result or when approval is required by the safety boundary.
