---
type: concept
topics:
  - "[[Image Analysis]]"
status: draft
updated: 2026-09-30
aliases:
  - mAP
  - MAP
---

# Standard mean average precision

> [!summary] Summary
> Mean average precision (mAP) summarizes ranking or detection quality as one score from 0 to 1. Average precision compresses the precision-recall tradeoff for one class or query. The mean is that value averaged across classes or queries.

> [!important] Key Points
> - The score combines precision and recall rather than reporting either one alone.
> - It is used for information retrieval, ranking, and object detection.
> - The range is 0 to 1.
> - These notes do not yet record a particular operating point, such as mAP at IoU 0.50.

Mean average precision is a standard performance metric for information retrieval, ranking, and object detection. It combines precision and recall into a single score from 0 to 1.

## Related Concepts

- [[Image Analysis]] - the evaluation setting for the detection notes.
- [[Mask R-CNN]] - a detector of the kind mAP is used to score.
