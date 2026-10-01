---
type: concept
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
aliases:
  - FPN
sources:
  - "[[Rad-DINO.pdf]]"
  - "[[ChestX-Det.pdf]]"
Class: cse-507
---

# Feature Pyramid Network

> [!info] Supporting Material
> - [[Rad-DINO.pdf]]
> - [[ChestX-Det.pdf]]

> [!summary] Summary
> A Feature Pyramid Network builds feature maps at several scales and combines them, so a detector or segmentation head can use both fine spatial detail and high-level semantics. Deeper convolutional layers know more about what is in the image and less about exactly where small structures are. The pyramid is the repair for that tradeoff.

> [!important] Key Points
> - Deep features are semantic and spatially coarse. Shallow features are detailed and less semantic.
> - FPN merges those levels into multi-scale maps.
> - That merge helps both large and small structures.
> - On Rad-DINO, an FPN decoder on the encoder improves segmentation.
> - ChestX-Det uses the same reason: FPN can represent small and large findings.

A Feature Pyramid Network is a neural network architecture for detection and segmentation when objects vary in size. Deeper CNN layers capture high-level features and lose spatial resolution, which makes small objects harder to detect. FPN aggregates high-level semantic information and low-level spatial detail at multiple scales. (source [GeeksforGeeks](https://www.geeksforgeeks.org/computer-vision/feature-pyramid-network-fpn/))

## Notes

- Combine information at different spatial and semantic levels, which is useful for identifying both large and small structures. [[Rad-DINO.pdf#page=8&annotation=1855R|Rad-DINO, p.8]]
- Using an FPN decoder on an encoder like [[Rad-DINO.md|Rad-DINO]] substantially improves segmentation. [[Rad-DINO.pdf#page=8&annotation=1858R|Rad-DINO, p.8]]
- Known to get features for small and large objects. [[ChestX-Det.pdf#page=4&annotation=677R|ChestX-Det, p.4]]

## Related Concepts

- [[Rad-DINO.md|Rad-DINO]] - uses an FPN decoder on its encoder for segmentation.
- [[Mask R-CNN]] - the detector that uses FPN as a backbone in the chest X-ray notes.
- [[Structure Aware Relation Network]] - says Mask R-CNN with FPN still misses object-wise and class-wise relations.
