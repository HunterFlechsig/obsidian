# Note System

This document explains the knowledge-note conventions used by the Cursor Obsidian Note Converter. It is intentionally human-readable. `SKILL.md` contains the operational rules; this document explains why the system is structured this way and how to extend it.

## Design philosophy

A note should represent one independently useful unit of knowledge. The system should make notes easy to skim, easy to understand in depth, easy to find with Bases, and easy to navigate through links.

The converter is not a transcription tool. Handwritten notes define the scope and emphasis, but the finished note should repair shorthand, missing reasoning, inconsistent terminology, malformed equations, and clear factual mistakes.

Avoid two extremes: giant notes that combine unrelated ideas, and hundreds of tiny notes created merely because every technical noun could have a page. Split when a topic is both independently useful and sufficiently developed.

## Three organizational layers

Folders, properties, and links have different jobs.

**Folders** provide broad physical organization. Keep them shallow.

**Properties** provide structured metadata for filtering and Obsidian Bases.

**Wikilinks** encode the knowledge graph: parents, prerequisites, alternatives, comparisons, methods, and other conceptual relationships.

Do not encode the entire knowledge ontology in folders.

## Note types

The controlled core types are:

- `concept`: reusable knowledge such as Linear Discriminant Analysis or Depthwise Convolution
- `lecture`: a record of what a particular lecture covered
- `paper`: a research paper as a whole, including motivation, method, architecture, results, and takeaways
- `project`: project-specific knowledge or work
- `reference`: material whose primary purpose is lookup

New types should be added only when they represent a recurring distinction that improves organization or Bases queries.

## Core properties

A typical concept note looks like:

```yaml
---
type: concept
topics:
  - "[[Machine Learning]]"
status: complete
created: 2026-09-30
updated: 2026-09-30
aliases:
  - LDA
sources:
  - "[[Lecture 08 - Discriminant Analysis.pdf]]"
---
```

Optional properties are omitted when irrelevant. This is preferable to filling every note with empty keys.

### topics

`topics` contains the most useful immediate organizational parents, not every ancestor.

For example, if `[[Medical Imaging]]` already belongs to `[[Computer Vision]]`, a ChestX-Det note may only need:

```yaml
topics:
  - "[[Medical Imaging]]"
```

The graph can supply the higher-level relationship.

### status

Use:

- `draft`: conversion/content is materially incomplete
- `review`: a meaningful uncertainty or correction needs human attention
- `complete`: no currently known unresolved conversion issue

`complete` does not mean the note can never change.

### dates

`created` and `updated` describe the digital note. A source/lecture date should use a separate property when useful. Preserve `created` when updating an existing note. Change `updated` only when content changes.

### aliases

Prefer a clear canonical title and add useful abbreviations or alternate names as aliases. Avoid redundant variants that add no search value.

### sources

`sources` records files that materially contributed to the note. It supports provenance and lets the vault answer questions such as which notes came from a particular paper or lecture.

## Standard shell

Every normal knowledge note should have:

1. Frontmatter
2. H1 title
3. Supporting Material callout when sources exist
4. Summary callout
5. Key Points callout
6. Adaptive body
7. Related Concepts

Example:

```markdown
# Linear Discriminant Analysis

> [!info] Supporting Material
> - [[Lecture 08 - Discriminant Analysis.pdf]]

> [!summary] Summary
> Linear Discriminant Analysis is a probabilistic classifier that models classes with Gaussian distributions sharing a common covariance matrix. The shared covariance causes the quadratic terms in the discriminant comparison to cancel, producing linear decision boundaries.

> [!important] Key Points
> - Each class has its own mean.
> - Classes share a pooled covariance matrix.
> - The resulting decision boundary is linear.
> - Class priors can shift the discriminant scores.

## Assumptions
...

## Discriminant Function
...

## Related Concepts
- [[Quadratic Discriminant Analysis]] - allows class-specific covariance matrices.
- [[Bayes Classifier]] - provides the probabilistic basis for the discriminant rule.
```

The body is adaptive. A paper note might use Motivation, Method, Architecture, Results, and Limitations. A math note might use Definition, Intuition, Derivation, and Worked Example. Do not force irrelevant headings.

## Summary vs Key Points

The Summary is 2-5 sentences explaining what the note is about and why it matters.

Key Points are 3-7 facts worth seeing during a quick review. Some conceptual overlap is fine, but the two should not simply repeat each other.

## Splitting

A source can produce several notes when several independently useful topics are sufficiently developed. Split notes should remain understandable independently, but substantial common explanations should live in the most appropriate note and be linked rather than copied everywhere.

A lecture can remain as a chronological/context note while reusable concepts are split into permanent concept notes. A paper note represents the paper as a whole; concepts extracted from the paper become separate notes only when sufficiently developed.

## Links

Links should be deliberate. Parent topics belong in `topics`; inline links and Related Concepts capture broader relationships.

Prefer existing canonical note names. Use display aliases naturally:

```markdown
[[Linear Discriminant Analysis|LDA]] assumes a shared covariance matrix.
```

Unresolved links are allowed as future-note placeholders. Do not automatically create empty notes for them.

## Equations

Use MathJax rather than images of equations whenever possible. Clean notation, correct obvious errors, and define symbols when the reader would otherwise have to infer them.

Example:

```markdown
$$
g_i(x) = x^T\Sigma^{-1}\mu_i
- \frac{1}{2}\mu_i^T\Sigma^{-1}\mu_i
+ \ln P(\omega_i)
$$
```

## Callouts

Summary and Key Points are standard. Additional callouts are used only when they convey semantics, for example:

```markdown
> [!warning] Common Mistake
> A depthwise convolution processes channels independently; it does not by itself mix information across channels.
```

Avoid decorative callout overload.

## Folder model

The fallback organization is domain-first and shallow:

```text
Computer Vision/
  Concepts/
  Papers/
  Projects/
Machine Learning/
  Concepts/
  Papers/
  Projects/
Robotics/
  Concepts/
  Papers/
  Projects/
Courses/
  <Course>/
    Lectures/
    Assignments/
__Support/
```

Existing established conventions override this fallback. A note spanning several domains gets one primary physical location; other memberships belong in `topics`.

Course artifacts remain under Courses, while reusable concepts should live in permanent domain folders and be linked from the lecture/course notes.

## Updating older notes

When an old note is directly involved in a conversion, it can be migrated toward this structure gradually. Preserve useful custom sections. Do not rewrite an entire vault just to achieve formatting uniformity.

## Extending the system

Before adding a new property or note type, ask:

1. Will it be used repeatedly?
2. Does it support a real query, Base, or workflow?
3. Is the information already represented by folders, topics, sources, or links?
4. Will adding it reduce ambiguity rather than duplicate metadata?

Prefer small stable schemas over metadata accumulation.
