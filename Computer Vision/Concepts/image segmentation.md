---
type: concept
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
---

# Image Segmentation

> [!summary] Summary
> Image segmentation partitions a digital image into groups of pixels, called segments, so later steps can work on coherent regions instead of the raw pixel array. It is a parent task for semantic segmentation and a prerequisite for many detection and measurement pipelines.

> [!important] Key Points
> - A segment is a group of pixels treated as one region.
> - The partition is what lets later algorithms detect objects and measure them.
> - Semantic segmentation is one subcategory: it assigns a class label to each pixel.
> - The definition used here follows IBM's overview of the task.

## Definition

Image segmentation partitions a digital image into discrete groups of pixels, called image segments, to inform object detection and related tasks. Parsing an image into specifically shaped segments enables faster and more advanced image processing. (via [IBM](https://www.ibm.com/think/topics/image-segmentation))

## Related Concepts

- [[semantic segmentation]] - assigns a class to each pixel.
- [[Mask R-CNN]] - adds an instance mask on top of detection.
- [[Dice score]] - an overlap score used to judge segmentation.
