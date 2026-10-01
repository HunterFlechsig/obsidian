---
type: concept
topics:
  - "[[Databases]]"
status: draft
updated: 2026-09-30
---

# Distributed and Parallel Database Systems

> [!summary] Summary
> A distributed database keeps related data at multiple sites on a network. These notes list the usual reasons: availability, scalability, fault tolerance, and transparency. Fragmentation is the mechanism named for splitting that data across sites.

> [!important] Key Points
> - The data lives at multiple sites, not in one table on one machine.
> - More copies and more sites are the availability argument.
> - Smaller pieces can make failure recovery easier.
> - Transparency means the user still sees data independence rather than the network layout.
> - The fragmentation methods named here are distributed fragmentation and derived horizontal fragmentation.

## Properties

- **Availability:** more available.
- **Scalability:**
- **Reliability / fault tolerance:** more pieces, so recovery can be easier.
- **Transparency:** isolated from the user. Provide data independence.

Multiple sites store the data: a collection of related databases, distributed over a network.

## Notes

- Use [[Distributed Fragmentation]].
- Use [[Derived Horizontal Fragmentation]].

## Related Concepts

- [[Distributed Fragmentation]] - the completeness, reconstruction, and disjointness rules for a split.
- [[Derived Horizontal Fragmentation]] - horizontal fragments defined through a related table.
