# Audit Mode

Audit mode inspects the Obsidian vault without changing it. It is intended to help the vault converge toward the note system safely over time.

## Two stages

### Stage 1: read-only audit

Inspect and report findings. Do not edit files.

### Stage 2: approved remediation

After the user chooses specific findings/categories, make only the approved changes and validate them afterward.

## Useful audit categories

### Likely duplicate notes

Look for title/alias overlap, near-identical concepts, duplicated source coverage, and strongly overlapping content. A similarity signal is not enough to merge automatically.

### Link integrity

Distinguish:

- intentional unresolved concept links, which are valid future-note placeholders
- unresolved links that closely resemble an existing renamed note or typo
- missing source PDFs/images, which are stronger integrity problems

### Orphan notes

Identify notes with little or no connection to the rest of the vault. Being an orphan is not automatically an error; report it as an organizational finding.

### Properties

Find missing or inconsistent `type`, `topics`, `status`, `sources`, aliases, or other properties that conflict with established Bases/conventions.

### Folder consistency

Identify notes whose placement conflicts with the established domain-first organization. Do not move them during audit.

### Source organization

Identify papers, slides, handwritten material, or images outside established `__Support/` conventions when relevant.

### Naming and aliases

Find likely canonical-name conflicts, duplicate aliases, or inconsistent abbreviations.

### Potentially unused assets

An asset with no detected references may be unused, but never assume that proves safe deletion. Report candidates only.

## Severity

Severity describes practical risk, not subjective quality.

**High** examples:
- broken image/source link used by an active note
- likely duplicate established notes creating conflicting knowledge
- a link that appears broken due to an obvious rename

**Medium** examples:
- inconsistent folder placement
- missing metadata needed by active Bases
- conflicting aliases

**Low** examples:
- optional naming cleanup
- potentially useful alias
- orphan that may be intentional

## Reporting

Audit reports should be actionable and concise. Group related findings and show affected paths. Do not create a permanent audit note unless requested.

For large audits, prioritize findings that affect data integrity/navigation before cosmetic consistency.
