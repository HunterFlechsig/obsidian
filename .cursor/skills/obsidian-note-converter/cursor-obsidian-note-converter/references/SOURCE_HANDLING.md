# Source Handling

This document describes how handwritten notes, papers, slides, images, external research, equations, figures, and other evidence should be used.

## Source roles

The user's handwritten or rough notes define scope and emphasis. They are not automatically the strongest factual authority.

Supporting papers, lecture slides, textbooks, and official documentation verify and enrich the material. Existing vault notes provide continuity but can contain mistakes. External research is used only when local material and established knowledge are insufficient or when current verification matters.

## Discovering sources

The vault's established support root is `__Support/`. Preserve its actual subfolder structure. Common categories include Papers, Images, Slides, and Handwritten.

Search existing source relationships first. A PDF already linked from a note is a stronger candidate than a filename-only match. If filenames are opaque, inspect document metadata or searchable text when available.

## Provenance in notes

Material supporting documents appear in two places:

1. the `sources` frontmatter property for structured querying
2. the visible Supporting Material callout for human readers

Use page-specific links when a claim, equation, or figure comes from a particular page.

Example:

```markdown
> [!info] Supporting Material
> - [[ChestX-Det.pdf]]

The relation module aggregates contextual features across candidate regions. ([[ChestX-Det.pdf#page=5]])
```

## Multiple sources

Do not treat all sources as equal. The original paper is usually strongest for its architecture/results; lecture slides may be better for pedagogical explanation. Existing notes can provide terminology and relationships. Choose the best source for the claim at hand.

Sources accumulate when they genuinely contribute. Updating from a new lecture should not erase an existing primary paper from `sources`.

## External research

External research is not the default. Use it for missing, uncertain, current, or verification-sensitive information. Prefer:

1. original papers
2. official documentation
3. textbooks/academic sources
4. reputable explanatory sources

Material external sources should be linked/cited. Do not automatically download pages into the vault.

## Source files are read-only

Normal conversion never modifies source PDFs or handwritten originals. Do not annotate, rename, move, compress, or overwrite them without explicit permission.

If a source is poorly placed, recommend the correct `__Support/` location rather than moving it automatically.

## Figures

Explicit user requests have priority. Search `__Support/Images/` before extracting a duplicate.

A good asset filename contains enough context to remain understandable in a large shared image folder:

```text
ChestX-Det - Figure 4 - Relation Module.png
LDA - Handwritten Decision Boundary Diagram.png
```

Preserve source figure numbers. Use concise note-specific captions and include source/page attribution.

For multi-panel figures, keep only the relevant panel when the rest adds no needed context. Preserve the full figure when panel relationships matter.

Intermediate PDF renders/crops belong in temporary storage. Only finalized assets belong in the vault.

## Hand-drawn diagrams

Use a supporting-document figure when it clearly provides the best version of the same idea, unless the user explicitly wants the hand-drawn version.

Simple flows may be reconstructed as Mermaid when confident. Preserve the handwritten image when its spatial layout, custom annotation, drawing, or uncertainty is important.

## Tables

Simple reliable tables should become Markdown for searchability and editability. Complex visual tables should remain images when reconstruction risks transcription errors or loses important formatting.

## Equations

Convert handwritten equations to MathJax, clean notation, correct obvious mistakes, and define variables where useful. Never preserve a malformed equation merely for transcription fidelity.

## Processing annotations

The user may write directives in the notes. Examples:

```text
[FIGURE: 4]
[SPLIT NOTE]
[EXPAND]
[KEEP SHORT]
```

Natural-language equivalents are also valid. These are commands to the converter, not content, and should disappear from the final note after being applied.
