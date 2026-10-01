---
type: concept
topics:
  - "[[Foundation X.md|Foundation X]]"
status: draft
updated: 2026-09-30
sources:
  - "[[Foundation X.pdf]]"
---

# Lock-Release Pretraining

> [!info] Supporting Material
> - [[Foundation X.pdf]]

> [!summary] Summary
> Lock-release pretraining is the pretraining strategy Foundation X uses for a chest X-ray model trained on classification, localization, and segmentation together. These notes name the strategy and do not yet record which weights are locked, when they are released, or how that schedule interacts with cyclic learning.

> [!important] Key Points
> - It is a pretraining strategy, not a dataset or a loss by itself.
> - Foundation X uses it.
> - The paper note links it with cyclic learning and a student-teacher setup.
> - The lock and release steps are not yet written down here.

## Notes

- Pretraining strategy used in [[Foundation X.md|Foundation X]]. [[Foundation X.pdf#page=1&selection=47,23,49,54&color=yellow|Foundation X, p.1]]

## Related Concepts

- [[Foundation X.md|Foundation X]] - the paper that defines the strategy.
- [[Overfitting]] - the failure mode that paper associates with training many tasks at once.
