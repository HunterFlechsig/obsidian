---
type: reference
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
sources:
  - "[[ChestX-Det.pdf]]"
---

# ChestX-Det

> [!info] Supporting Material
> - [[ChestX-Det.pdf]]

> [!summary] Summary
> ChestX-Det is a chest X-ray dataset with bounding boxes and masks, built from ChestX-ray14 for fully supervised detection and segmentation. It contains 3,575 images covering 13 common diseases, labeled by board-certified radiologists. SAR-Net is the system these notes pair with the dataset.

> [!important] Key Points
> - 3,575 photos and 13 diseases.
> - Labels are boxes and masks, not image-level tags alone.
> - Derived from [[ChestX-ray14]].
> - Annotation uses a committee of two less experienced radiologists and a more experienced judge.
> - The judge selects from the first two annotations and can add more.

## Notes

- Chest X-ray dataset with boxes and masks created from the [[ChestX-ray14]] dataset. [[ChestX-Det.pdf#page=1&annotation=656R|ChestX-Det, p.1]]
- 3,575 photos.
- 13 common diseases.
- Labeled by 3 board-certified radiologists.

## Annotation Process

- **Committee:** every X-ray is annotated by 2 less experienced radiologists.
- **Judge:** a more experienced radiologist selects annotations from the first two and can add more.

## Related Concepts

- [[Structure Aware Relation Network]] - the detection and segmentation model trained with this dataset.
- [[Image Analysis]] - the broader task the boxes and masks support.
