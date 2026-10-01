---
type: paper
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
aliases:
  - Snakes
  - Active Contour Models
  - Snakes, Active Contour Modeling
sources:
  - "[[Snakes-Contour-Models.pdf]]"
Class: cse-507
---

# Snakes - Active Contour Modeling

> [!info] Supporting Material
> - [[Snakes-Contour-Models.pdf]]

> [!summary] Summary
> A snake is an energy-minimizing spline that moves through an image until it lies on a contour. Image forces pull it toward lines and edges. External constraints, including a user, push it out of a bad local minimum toward the intended contour. The note contrasts this interactive energy with bottom-up edge detection that cannot undo an early mistake.

> [!important] Key Points
> - The target is an image contour: edges, lines, or a subjective contour.
> - The snake always reduces an energy. It slithers rather than solving one global optimization.
> - Internal energy controls the spline. Image energy attracts it to features. External energy encodes constraints.
> - Snake Pit is the interface for start points, springs between snakes, and volcano repulsion.
> - Line attraction depends on the sign of $w_{\text{line}}$. Edge attraction is a separate energy term.

## Notes

Paper title: Snakes: Active Contour Models.

- A snake is guided by features such as lines and edges.
- User-imposed constraints guide it to features.
- Uses include edge and line detection, stereo matching, and motion tracking.
	- Claim: autonomous bottom-up processes.
	- Problem: mistakes propagate, with no ability to correct them.
- Energy minimizing and interactive. The user can push the snake out of a local minimum toward the desired solution.
- Domain: image contours, including edges, lines, and subjective contours.
- It keeps minimizing energy, slithering, instead of committing to one global solution.

## Basic Snake Behavior

- A controlled-continuity spline under image forces and external constraint forces.
	- Image forces push toward important features.
	- External forces push toward the desired local minimum.
- From position, form the energy function.

![[Pasted image 20260920114945.png]]

- Internal spline energy.

![[Pasted image 20260920115222.png]]

- Snake Pit:
	- A UI to place starting points and exert forces that pull snakes toward useful features.
	- Springs connect snakes as constraints.
	- A volcano is a repulsion force.

### Image Forces

- An energy function attracts the snake to features.
- The image energy is a combination of three energy functions.

![[Pasted image 20260920120013.png]]

- **Line function:** the sign of $w_{\text{line}}$ attracts the snake to light or dark.

![[Pasted image 20260920120132.png]]

- **Edge functions:**

![[Pasted image 20260920120228.png]]

## Related Concepts

- [[image segmentation]] - another way to mark object boundaries, without an explicit deforming contour.
- [[Image Analysis]] - the broader task of pulling structure out of an image.
