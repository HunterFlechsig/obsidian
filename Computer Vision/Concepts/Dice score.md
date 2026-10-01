---
type: concept
topics:
  - "[[image segmentation]]"
status: complete
updated: 2026-09-30
aliases:
  - Dice coefficient
  - DSC
sources:
  - "[[Rad-DINO.pdf]]"
---

# Dice score

> [!info] Supporting Material
> - [[Rad-DINO.pdf]]

> [!summary] Summary
> The Dice score, or Dice-Sørensen coefficient, measures how much two sets overlap. In segmentation it compares a predicted region with the labeled region. The score is $1$ when the regions match and $0$ when they share no pixels.

> [!important] Key Points
> - For sets $A$ and $B$, $\mathrm{Dice} = 2|A \cap B| / (|A| + |B|)$.
> - It was introduced independently by Lee Raymond Dice (1945) and Thorvald Sørensen (1948).
> - Rad-DINO uses it to test segmentation models.
> - Definition via [Wikipedia](https://en.wikipedia.org/wiki/Dice-S%C3%B8rensen_coefficient).

## Definition

The Dice-Sørensen coefficient gauges the similarity of two samples. Dice and Sørensen developed it independently and published in 1945 and 1948. (source [Wikipedia](https://en.wikipedia.org/wiki/Dice-S%C3%B8rensen_coefficient))

$$
\mathrm{Dice}(A, B) = \frac{2 |A \cap B|}{|A| + |B|}
$$

## Notes

- Used to test segmentation models. [[Rad-DINO.pdf#page=8&annotation=1849R|Rad-DINO, p.8]]

## Related Concepts

- [[image segmentation]] - the task this score evaluates.
- [[Rad-DINO.md|Rad-DINO]] - reports Dice on its segmentation benchmarks.
- [[semantic segmentation]] - a pixel-labeling task often scored with Dice.
