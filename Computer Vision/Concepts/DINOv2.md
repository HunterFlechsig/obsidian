---
type: concept
topics:
  - "[[Self-Supervised Learning]]"
status: complete
updated: 2026-09-30
sources:
  - "[[Rad-DINO.pdf]]"
Class: cse-507
---

# DINOv2

> [!info] Supporting Material
> - [[Rad-DINO.pdf]]

> [!summary] Summary
> DINOv2 is Meta's self-supervised training method for visual features. It learns from images without labels. Rad-DINO starts from that method and keeps its ability to represent both global and local structure. DINOv2's KoLeo regularizer pushes representations apart so they do not collapse, which helps retrieval and clustering.

> [!important] Key Points
> - No labels are required.
> - Pretrained checkpoints are used for depth, semantic segmentation, classification, and instance retrieval.
> - The features are dense enough for downstream dense tasks.
> - Rad-DINO uses DINOv2 as its starting point.
> - KoLeo regularization spreads representations out.
> - Method overview via [Roboflow](https://blog.roboflow.com/what-is-dinov2/). The Rad-DINO claims are from that paper.

## Definition

DINOv2 learns visual features directly from images without labels. Pretrained checkpoints support depth estimation, semantic segmentation, image classification, and instance retrieval, and the architecture produces dense embeddings for downstream tasks. (source [Roboflow](https://blog.roboflow.com/what-is-dinov2/))

## Notes

- Used as the starting point for [[Rad-DINO.md|Rad-DINO]]. [[Rad-DINO.pdf#page=8&annotation=1861R|Rad-DINO, p.8]]
- DINOv2 uses a KoLeo regularizer, which encourages representations to spread out rather than collapse. This can improve image retrieval and clustering. [[Rad-DINO.pdf#page=9&annotation=1864R|Rad-DINO, p.9]]
- [[Rad-DINO.md|Rad-DINO]] inherits DINOv2's ability to learn both global and local visual information without labels or text.

## Related Concepts

- [[Rad-DINO.md|Rad-DINO]] - the medical encoder adapted from DINOv2.
- [[Masked Image Modeling]] - the local objective Rad-DINO adds alongside instance discrimination.
- [[Self-supervised instance discrimination]] - the global objective in this family of methods.
