---
type: concept
topics:
  - "[[semantic segmentation]]"
status: complete
updated: 2026-09-30
aliases:
  - PSPNet
  - Pyramid Scene Parsing Network
---

# PSP-Net

> [!summary] Summary
> PSPNet (Pyramid Scene Parsing Network) is a semantic segmentation model from Zhao et al., 2017. Its pyramid pooling module gathers context at several scales so a pixel label can depend on both local appearance and the broader scene.

> [!important] Key Points
> - The task is pixel-wise semantic segmentation.
> - Pyramid pooling is the module that captures context at multiple scales.
> - The chest X-ray notes use a pretrained PSPNet to mark anatomical parts for SAR-Net.
> - Description follows the GeeksforGeeks overview of the model.

## Definition

PSPNet assigns a class to each pixel. The pyramid pooling module is there because a single-scale feature misses context that sits at a different size in the image. (via [GeeksforGeeks](https://www.geeksforgeeks.org/computer-vision/pspnet-pyramid-scene-parsing-network-for-image-segmentation/))

## Related Concepts

- [[semantic segmentation]] - the task PSPNet performs.
- [[Structure Aware Relation Network]] - uses a pretrained PSPNet for lungs, scapulae, and heart.
