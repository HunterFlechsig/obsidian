---
type: concept
topics:
  - "[[image segmentation]]"
status: complete
updated: 2026-09-30
aliases:
  - Mask RCNN
---

# Mask R-CNN

> [!summary] Summary
> Mask R-CNN is a two-stage model that detects objects, draws a box around each one, and predicts a mask of its shape. One forward pass answers what is present, where it is, and which pixels belong to that instance.

> [!important] Key Points
> - It is a two-stage detector.
> - It does object detection, box regression, and instance segmentation together.
> - The mask is an instance mask, so separate objects of the same class stay separate.
> - SAR-Net uses it as the baseline and then adds relation features the baseline does not model.
> - Overview via [Medium](https://medium.com/@deepvisionkararhaider/the-brain-behind-object-segmentation-a-complete-guide-to-mask-r-cnn-77f5016140d8).

## What it predicts

1. Object detection: what objects are present.
2. Bounding box regression: where they are.
3. Instance segmentation: the shape of each object.

![[Pasted image 20260929155336.png]]

(via [Medium](https://medium.com/@deepvisionkararhaider/the-brain-behind-object-segmentation-a-complete-guide-to-mask-r-cnn-77f5016140d8))

## Related Concepts

- [[Feature Pyramid Network|FPN]] - the multi-scale backbone used with Mask R-CNN in the chest X-ray notes.
- [[image segmentation]] - the broader family of pixel-grouping tasks.
- [[Structure Aware Relation Network]] - extends Mask R-CNN with anatomical, contextual, and disease relations.
