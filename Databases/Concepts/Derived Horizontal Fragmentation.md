---
type: concept
topics:
  - "[[Distributed Fragmentation]]"
status: draft
updated: 2026-09-30
aliases:
  - DHF
---

# Derived Horizontal Fragmentation

> [!summary] Summary
> Derived horizontal fragmentation splits a member relation by following its link to an owner relation that has already been fragmented horizontally. The member rows go to the fragment that owns them, rather than by a predicate written on the member table itself.

> [!important] Key Points
> - There is an owner relation and one or more member relations.
> - The member split is derived from the owner's horizontal fragments.
> - The note's example is a selection on salary, used to illustrate that owner-side predicate.

![[Pasted image 20260927180717.png]]

- Links between the owner relation ($L_1$) and the member relations.
- Example: select on salary.

## Related Concepts

- [[Primary Horizontal Fragmentation]] - the owner-side split, defined by predicates on the owner's own attributes.
- [[Distributed Fragmentation]] - the parent rules for a valid fragmentation.
- [[Distributed and Parallel Database Systems]] - the setting that uses this split.
