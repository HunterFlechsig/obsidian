---
type: concept
topics:
  - "[[Self-Supervised Learning]]"
status: complete
updated: 2026-09-30
aliases:
  - MIM
sources:
  - "[[Rad-DINO.pdf]]"
---

# Masked Image Modeling

> [!info] Supporting Material
> - [[Rad-DINO.pdf]]

> [!summary] Summary
> Masked image modeling hides part of an image and trains a network to reconstruct it from the visible context. The hiding is a pretext task: the point is the representation learned along the way, not the reconstruction itself. Rad-DINO uses it because that pressure teaches local image structure.

> [!important] Key Points
> - Patches are masked, and the network reconstructs the missing content.
> - It is a self-supervised pretext task. Labels are not required.
> - Design choices include what to mask, what to reconstruct, and which architecture does the reconstruction.
> - In Rad-DINO, removing MIM lowers segmentation performance on every tested structure.
> - MIM supplies local structure. Instance discrimination supplies the global counterpart.
> - Overview source: Emergent Mind.

## Definition

Masked image modeling is a self-supervised learning paradigm in computer vision. A neural network reconstructs masked image content from the visible context. It is used both as a pretext task and as an architectural scheme, on natural images, 3D medical imaging, and multimodal settings. Research themes include masking design, reconstruction targets, architecture, locality bias, invariance, and downstream performance. (source Emergent Mind)

## Notes

- MIM contributes to representation for dense tasks such as segmentation. Removing MIM causes segmentation performance to fall across every tested structure in Rad-DINO. [[Rad-DINO.pdf#page=8&annotation=1852R|Rad-DINO, p.8]]
- MIM is one reason Rad-DINO works for segmentation: it forces the encoder to learn local image structure, complementing the global information learned through instance discrimination. [[Rad-DINO.pdf#page=8&annotation=1852R|Rad-DINO, p.8]]

## Related Concepts

- [[Rad-DINO.md|Rad-DINO]] - combines MIM with instance discrimination.
- [[Self-supervised instance discrimination]] - the global objective paired with MIM in that paper.
- [[semantic segmentation]] - a dense task that benefits from the local features.
