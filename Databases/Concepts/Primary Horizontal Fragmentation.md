---
type: concept
topics:
  - "[[Distributed Fragmentation]]"
status: complete
updated: 2026-09-30
aliases:
  - PHF
---

# Primary Horizontal Fragmentation

> [!summary] Summary
> Primary horizontal fragmentation splits one table into row subsets by predicates on that table's own attributes. Each fragment is a selection. Every tuple of the original relation should satisfy exactly one minterm predicate when the fragmentation is both complete and disjoint.

> [!important] Key Points
> - The predicates use the table being split, not a related table.
> - Fragment $R_j$ is the selection of relation $R$ for fragment predicate $j$, with $i \le j \le w$.
> - A minterm predicate is a conjunction of simple predicates or their negations.
> - Example minterms: $\mathrm{PNAME} = \text{``maintenance''} \land \mathrm{BUDGET} \le 200000$, and the same budget bound with the name predicate negated.

Primary horizontal fragmentation splits a single database table row by row into smaller subsets using conditions applied directly to that table's own attributes.

## Notes

- $R_j = \sigma_{\mathrm{frag}_j}(R)$, for $i \le j \le w$.
- All tuples of $R$ satisfying a minterm predicate $m$.
- $m_1$: $\mathrm{PNAME} = \text{``maintenance''} \land \mathrm{BUDGET} \le 200000$
- $m_2$: $\lnot (\mathrm{PNAME} = \text{``maintenance''}) \land \mathrm{BUDGET} \le 200000$

## Related Concepts

- [[Distributed Fragmentation]] - the completeness, reconstruction, and disjointness rules these fragments must meet.
- [[Derived Horizontal Fragmentation]] - the row split that is defined from a related owner table instead.
