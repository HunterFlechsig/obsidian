---
type: concept
topics:
  - "[[Distributed and Parallel Database Systems]]"
status: complete
updated: 2026-09-30
aliases:
  - Database Fragmentation
---

# Distributed Fragmentation

> [!summary] Summary
> Fragmentation splits one relation into pieces that can live at different sites. A correct fragmentation is complete, reconstructable, and disjoint: every original item appears in some fragment, the fragments can be combined back into the relation, and the same item is not stored twice unless the design says so. Horizontal fragmentation splits by row. Vertical fragmentation splits by column.

> [!important] Key Points
> - Completeness: every data item in the original can be found in a fragment.
> - Reconstruction: some relational operator rebuilds the original from the fragments.
> - Disjointness: fragments do not duplicate items.
> - Horizontal means rows. Vertical means columns, grouped by attributes that are used together.
> - An attribute affinity measure, stored in an affinity matrix, scores how similarly two attributes are accessed.
> - A salary predicate such as $\mathrm{sal} < 10$ together with $\mathrm{sal} > 20$ drops the rows in between. Overlapping predicates duplicate rows.

> [!note] Properties
> **Completeness**: every data item in the original can be found in a fragment.
>
> **Reconstruction**: there must be some relational operator to reconstruct the original relation from all fragments.
>
> **Disjointness**: there are no duplicates in the fragmentation.

## Horizontal

![[Pasted image 20260927151611.png]]

- Split by row.

## Vertical

![[Pasted image 20260927151700.png]]

- Split by column.
- Group related attributes.

![[Pasted image 20260927181051.png]]

- An affinity matrix is used to figure out the splits.
- The attribute affinity measure between $A_i$ and $A_j$ of a relation scores how similar their access patterns are.

## Good Fragmentations

$F = \{F_1, F_2\}$

- $F_1$: select the subset $\mathrm{sal} < 10$
- $F_2$: select the subset $\mathrm{sal} > 20$

This may lose tuples with $10 \le \mathrm{sal} \le 20$.

$F = \{F_1, F_2\}$

- $F_1$: select the subset $\mathrm{sal} < 10$
- $F_2$: select the subset $\mathrm{sal} > 5$

Tuples with $5 < \mathrm{sal} < 10$ are duplicated.

## Methods

- [[Primary Horizontal Fragmentation]]
- [[Derived Horizontal Fragmentation]]

## Related Concepts

- [[Distributed and Parallel Database Systems]] - the systems that place these fragments at different sites.
- [[Primary Horizontal Fragmentation]] - row splits defined by predicates on the table itself.
- [[Derived Horizontal Fragmentation]] - row splits defined through an owner relation.
