---
type: paper
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
aliases:
  - SAR-Net
sources:
  - "[[ChestX-Det.pdf]]"
Class: cse-507
---

# Structure Aware Relation Network

> [!info] Supporting Material
> - [[ChestX-Det.pdf]]

> [!summary] Summary
> SAR-Net extends Mask R-CNN so thoracic disease detection can use more than the appearance of a proposed region. It adds how the region sits relative to anatomy, what the rest of the lung fields show, and which diseases tend to occur together. The same paper introduces ChestX-Det, a fully supervised chest X-ray dataset with boxes and masks.

> [!important] Key Points
> - Three relations: disease to thoracic anatomy, abnormality to lung-field context, and disease to disease.
> - A pretrained PSPNet supplies anatomical parts. Coordinate differences turn each proposal's position into a spatial feature.
> - The contextual module attends over the lung fields and adds that context to the region of interest.
> - The disease module builds a directed co-occurrence graph and passes messages across diseases.
> - Reported gains show up more in localization metrics than in classification.
> - Main detection metric in the notes: box AP, with AP50 as the primary number.

## My Notes

- Extends [[Mask R-CNN]].
- [[ChestX-Det.md|ChestX-Det]] dataset created for the system. [[ChestX-Det.pdf#page=1&annotation=656R|ChestX-Det, p.1]]
- Improves baseline performance of [[Mask R-CNN]].
- First to detect chest X-ray diseases with fully supervised annotations.

## Improving Relation Model

[[ChestX-Det.pdf#page=2&annotation=662R|ChestX-Det, p.2]]

1. Spatial relations between diseases and thoracic anatomical structures.
	- Some diseases are only located in certain areas.
2. Contextual relations between abnormalities and observations in the lung fields.
3. Categorical dependent relations among diseases.
	- Diseases can be caused by many factors.

![[Pasted image 20260929160348.png]] [[ChestX-Det.pdf#page=2&annotation=665R|ChestX-Det, p.2]]

## Structure Aware

[[ChestX-Det.pdf#page=2&annotation=668R|ChestX-Det, p.2]]

**First:** extract anatomical features via a pretrained semantic part segmentation model.

**Add 3 modules:**

1. Spatial relation
	- Encode the spatial relations between disease proposals and anatomical parts.
2. Contextual
	- Extract lung-field features as the context, then use query-key attention to learn attention weights in the lung fields.
3. Disease relation
	- Build a relation graph among diseases from the co-occurrence frequency of each pair. Propagate information through the adjacency matrix among diseases.

## Object Detection and Instance Segmentation

- [[Feature Pyramid Network|FPN]] is used as the backbone. [[ChestX-Det.pdf#page=4&annotation=674R|ChestX-Det, p.4]]
- [[Mask R-CNN]] and [[Feature Pyramid Network|FPN]] fail to capture object-wise and class-wise relations. [[ChestX-Det.pdf#page=5&annotation=680R|ChestX-Det, p.5]]

![[Pasted image 20260929162209.png]]

- Enhance the proposed region with the modules. [[ChestX-Det.pdf#page=5&annotation=683R|ChestX-Det, p.5]]

## Modules In Depth

### Anatomical Structure Module

[[ChestX-Det.pdf#page=6&annotation=686R|ChestX-Det, p.6]]

- Some diseases are related to parts of the body, such as the lungs and the heart.
- Encode relations between disease and anatomy.
- Use a pretrained [[PSP-Net]] segmentation model.
	- Chosen parts: left and right lungs, left and right scapula, heart.
	- For each region of interest, use the coordinate difference to quantify the spatial relation with anatomy.

### Contextual Relation Module

[[ChestX-Det.pdf#page=7&annotation=689R|ChestX-Det, p.7]]

- Applied only on the left and right lung.
- Uses an attention mechanism.
- When certain diseases are in the region of interest, a set of key contents in the lung fields is aggregated according to attention learned on disease-context relations.
- Enriches the original region representation with contextual information.

### Disease Relation Module

[[ChestX-Det.pdf#page=8&annotation=692R|ChestX-Det, p.8]]

- Diseases are correlated. Pulmonary tuberculosis, for example, is a complex disease that might contain nodules, fibrosis, and consolidation at the same time.
- **Relation graph construction:** compute the frequency of one disease in comparison with the other.

	![[Pasted image 20260929164517.png]]

	- Build a directed graph.
	- ![[Pasted image 20260929164625.png]]
- **Message passing via the relation graph**
	- Use global attention.
	- Allows related diseases to share information.
	- Each disease has a semantic embedding $w_j$.
	- Information passed from disease $j$ to disease $i$ is weighted by:
		- $\beta_j$: evidence that disease $j$ is present in the current X-ray
		- $e_{j \rightarrow i}$: strength of the relationship between diseases $j$ and $i$
- **Mapping disease relation to regions**

## Results

- Datasets: [[ChestX-Det.md|ChestX-Det]] and **DR-private**.
- **Segmentation performance:** mean IoU 86.5.
- **Evaluation.** [[ChestX-Det.pdf#page=10&annotation=695R|ChestX-Det, p.10]]
	- Bounding box AP.
	- AP50 is the main evaluation metric.
	- AP75 better reflects localization.
	- AP25 better reflects classification.
	- Mask AP for instance segmentation.
	- FROC.
- **Setup**
	- Random flipping and scale sampling.
	- 6 different backbones.
- **Baseline comparison.** [[ChestX-Det.pdf#page=11&annotation=698R|ChestX-Det, p.11]]
	- Better on the main detection results in Table 3.
	- The claim of a less pronounced improvement on DR-private is attributed to different data distributions.
	- For each category the baseline is slightly better for several. SAR-Net mostly surpasses the baseline, and less so on the private set.
- **Stronger backbones** boost performance. The modules are robust. [[ChestX-Det.pdf#page=14&annotation=701R|ChestX-Det, p.14]]
- **Ablation study:** remove modules to see performance. Little effect on classification, with increases on the other statistics, which indicates localization ability. [[ChestX-Det.pdf#page=15&annotation=704R|ChestX-Det, p.15]]

## Related Concepts

- [[Mask R-CNN]] - the detector SAR-Net extends.
- [[Feature Pyramid Network|FPN]] - the multi-scale backbone.
- [[PSP-Net]] - the pretrained anatomical segmentation model.
- [[ChestX-Det.md|ChestX-Det]] - the fully supervised dataset introduced with the method.
