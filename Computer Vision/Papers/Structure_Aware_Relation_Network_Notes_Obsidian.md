---
type: paper
topics:
  - "[[Image Analysis]]"
status: review
updated: 2026-09-30
sources:
  - "[[ChestX-Det.pdf]]"
Class: cse-507
---

# A Structure-Aware Relation Network for Thoracic Disease Detection and Segmentation

> [!info] Supporting Material
> - [[ChestX-Det.pdf]]

> [!summary] Summary
> SAR-Net extends Mask R-CNN for thoracic disease detection and instance segmentation. A standard detector mostly asks what a proposed region looks like. SAR-Net also asks where that region sits relative to anatomy, what the other lung regions imply, and which other diseases are evidence. Each relation is a module, and the three outputs are concatenated with the original region feature before detection and segmentation. The paper introduces ChestX-Det, with instance boxes and masks for 13 abnormalities. This is the long write-up. The annotated working note is [[Structure Aware Relation Network]], which keeps the alias SAR-Net.

> [!important] Key Points
> - The baseline question is what the region of interest looks like.
> - Spatial: where is it relative to known anatomy?
> - Contextual: what other parts of the lungs help interpret it?
> - Disease: what other diseases provide useful evidence?
> - The three module outputs are added to the original region representation before the box and mask heads.
> - This file duplicates [[Structure Aware Relation Network]] and does not own the SAR-Net alias.

## Motivation

### Problem with Standard Object Detection

-   Uses [[Mask R-CNN]] / [[Feature Pyramid Network|FPN]] as
    the baseline.
-   Standard object detection works primarily from features of
    individual RoIs.
-   Does not explicitly model:
    -   disease ↔ anatomy relationships
    -   disease ↔ surrounding lung context
    -   disease ↔ disease relationships

### Why Relations Matter in Chest X-Rays

**Spatial / Anatomical** - Some diseases have characteristic anatomical
locations. - Knowing where a proposal lies relative to the lungs, heart,
etc. can help determine whether the proposal makes sense.

**Contextual** - Radiologists use other regions of the X-ray to
interpret an abnormality. - Example: compare an abnormal-looking region
with the corresponding region of the opposite lung.

**Disease** - Diseases and abnormalities are not independent. - Certain
abnormalities frequently occur together. - Presence of one abnormality
can therefore provide evidence about another.

------------------------------------------------------------------------

# Overall SAR-Net Architecture

## Base Model

-   Based on [[Mask R-CNN]].
-   Uses [[Feature Pyramid Network|FPN]].
-   RPN generates disease region proposals.
-   [[RoI Align]] extracts a feature vector for each proposed
    region.

Original RoI feature:

$$
f
$$

SAR-Net generates three additional feature representations:

$$
f_{spa} = \text{spatial/anatomical information}
$$

$$
f_{cxt} = \text{lung contextual information}
$$

$$
f_{cate} = \text{disease relationship information}
$$

Final enhanced RoI representation:

$$
f' = [f : f_{spa} : f_{cxt} : f_{cate}]
$$

-   `:` represents concatenation.
-   The enhanced feature is passed to the box and mask heads.
-   Goal: give the detector more information than the RoI's appearance
    alone.

------------------------------------------------------------------------

# 1. Anatomical Structure Relation Module

## Purpose

**Question being answered:**

> Where is this disease proposal relative to important anatomical
> structures?

-   Some diseases are strongly associated with particular anatomical
    structures or locations.
-   Spatial location can therefore help identify and localize
    abnormalities.

## Anatomical Structures

Uses a pretrained [[PSP-Net]] segmentation model to identify:

1.  Left lung
2.  Right lung
3.  Left scapula
4.  Right scapula
5.  Heart

-   SAR-Net does **not** directly use the segmentation masks afterward.
-   Bounding boxes around these anatomical structures are used.

## Disease ↔ Anatomy Relationship

For every disease RoI:

-   Compare the RoI bounding-box coordinates with each anatomical-part
    bounding box.
-   Coordinate differences describe the relative position between them.

For one anatomical structure:

$$
M_p =
\left(
\frac{x_1^a-x_1^p}{w_l},
\frac{y_1^a-y_1^p}{h_l},
...
\right)
$$

where: - $a$ = abnormality / disease RoI - $p$ = anatomical part -
$w_l,h_l$ = dimensions of the bounding box covering the lungs

-   8 relative-coordinate values are calculated per anatomical
    structure.
-   5 structures × 8 values = **40-dimensional spatial vector**.

## Spatial Embedding

-   The coordinate representation is transformed into a
    higher-dimensional embedding using sine/cosine functions.
-   Similar idea to positional encoding.

Produces:

$$
f_{spa}
$$

### Main Idea

**Disease RoI location + known anatomy → spatial embedding**

This tells the detector:

> Does the location of this proposed disease make anatomical sense?

------------------------------------------------------------------------

# 2. Contextual Relation Module

## Purpose

**Question being answered:**

> Which other parts of the lungs provide useful information for
> interpreting this disease proposal?

-   Restricted to the **left and right lung fields**.
-   Uses an [[Attention Mechanism]].
-   Designed to mimic how radiologists use contextual information.

### Example: Contralateral Comparison

-   Suppose an area in the left lung looks abnormal.
-   Looking at the corresponding region in the right lung can provide
    useful context.
-   Similar bilateral appearance may indicate normal anatomy or artifact
    rather than disease.

## Lung Grid Features

-   Left and right lung fields are divided into grid regions.
-   A feature vector is extracted for each grid location.

**Query** - Current disease RoI feature.

**Keys / contextual features** - Grid features throughout both lungs.

The disease RoI is compared against all of these lung locations.

## Appearance Relation

For disease RoI $a_i$ and lung grid $g_j$:

$$
F(a_i,g_j)
=
\langle
W_a f_{a_i},
W_g f_{g_j}
\rangle
$$

-   Measures **feature/appearance compatibility**.
-   Asks: "How relevant does this lung region look to this disease
    proposal?"

## Spatial Relation

The model also considers where each lung grid is located relative to the
disease RoI:

$$
P(a_i,g_j)
$$

This asks:

> Is this lung region spatially relevant to the proposed abnormality?

## Attention

Appearance and spatial relationships are combined to calculate an
attention weight:

$$
\omega_j^{attn}
=
\frac{
\exp(\log P(a_i,g_j)+F(a_i,g_j))
}{
\sum_k \exp(\log P(a_i,g_k)+F(a_i,g_k))
}
$$

Each lung grid receives an attention weight.

-   **High attention** → this lung region contains useful context for
    the disease RoI.
-   **Low attention** → this region contributes little information.

## Context Aggregation

The lung-grid features are combined using their attention weights:

$$
f_{cxt}
=
W_k
\left(
\sum_j
\omega_j^{attn} f_{g_j}
\right)
$$

Produces:

$$
f_{cxt}
$$

### Main Idea

**Disease proposal → attend to relevant areas of lungs → aggregate their
features → contextual embedding**

Attention determines **which areas of the lungs should enrich the
disease RoI representation**.

------------------------------------------------------------------------

# 3. Disease Relation Module

## Purpose

**Question being answered:**

> Given the other diseases that may be present, what additional
> information should this disease proposal receive?

-   Diseases and abnormalities can be correlated.
-   Some commonly co-occur.
-   Some have causal relationships.

Examples from the paper: - Pulmonary tuberculosis may involve nodules,
fibrosis, and consolidation. - Rib fracture can be associated with
pneumothorax.

Therefore disease predictions should not necessarily be treated
independently.

## Disease Relation Graph

### Graph Construction

Each disease category becomes a **node**.

Edges represent relationships between disease categories.

Relationship strength is estimated using conditional probability:

$$
P(A|B)
=
\frac{\#(A,B)}{\#(B)}
$$

where: - $\#(A,B)$ = samples where A and B occur together - $\#(B)$ =
samples containing B

The graph is **directed**:

$$
e_{A\rightarrow B}=P(B|A)
$$

$$
e_{B\rightarrow A}=P(A|B)
$$

These values can differ.

The presence of A could provide strong evidence for B without the
reverse being equally true.

------------------------------------------------------------------------

# Message Passing Through Disease Graph

## Problem

The graph contains **dataset-level prior knowledge**, but not every
relationship is relevant to every individual X-ray.

SAR-Net therefore also determines which diseases appear likely in the
**current image**.

## Global Disease Evidence

The entire X-ray is processed to obtain a global feature representation.

A classifier generates:

$$
\beta \in \mathbb{R}^{C}
$$

where: - $C$ = number of disease categories - $\beta_j$ = evidence for
disease $j$ in the current X-ray

This acts like **global attention over disease categories**.

## Disease Semantic Embeddings

Each disease has a semantic representation:

$$
w_j
$$

The paper uses the classifier weights from the box head as these disease
embeddings.

## Message Passing

For disease $i$:

$$
z_i =
\sum_{j \in C}
\beta_j
e_{j\rightarrow i}
w_j
$$

Three pieces determine the information passed:

### $w_j$

Information representing disease $j$.

### $e_{j\rightarrow i}$

How strongly disease $j$ is related to disease $i$.

### $\beta_j$

How much evidence exists for disease $j$ in this particular X-ray.

Therefore:

**message strength = current-image evidence × disease relationship
strength**

### Main Idea

Related diseases **share information** through the graph.

If there is strong evidence for disease $j$, and $j$ is strongly related
to disease $i$, then disease $j$ contributes more information to the
representation of disease $i$.

------------------------------------------------------------------------

# Mapping Disease Relations Back to RoIs

The disease graph operates at the **disease-category level**, but Mask
R-CNN operates on individual **region proposals**.

Each RoI already has classification probabilities:

$$
P \in \mathbb{R}^{N_r \times C}
$$

For region $k$:

$$
r_k =
\sum_{i \in C}
p_{ki}z_i
$$

where: - $p_{ki}$ = probability that region $k$ belongs to disease
category $i$ - $z_i$ = enhanced representation of disease $i$

Then:

$$
f_{cate}=W_t r_k
$$

This produces the disease-relation feature for that RoI.

------------------------------------------------------------------------

# Final RoI Representation

Each proposal now has four sources of information:

  Feature      Information
  ------------ -----------------------------------------
  $f$          Original RoI appearance
  $f_{spa}$    Relationship with anatomical structures
  $f_{cxt}$    Relevant context throughout lung fields
  $f_{cate}$   Relationships with other diseases

They are concatenated:

$$
f' = [f:f_{spa}:f_{cxt}:f_{cate}]
$$

The enhanced feature is then used by: - Box head - Mask head

for final detection and segmentation.

------------------------------------------------------------------------

# [[ChestX-Det.md|ChestX-Det]] Dataset

-   Created as part of the work.
-   Subset of NIH ChestX-ray14.
-   **3,575 images**.
-   **13 disease/abnormality categories**.
-   Instance-level annotations:
    -   bounding boxes
    -   segmentation masks
-   Annotated by three board-certified radiologists.

Data split: - 3,025 training images - 553 testing images - 10% of
training samples used for validation

Also evaluated on: - **DR-Private** - 6,629 chest X-rays collected from
multiple Chinese hospitals

------------------------------------------------------------------------

# Evaluation

## Detection

Uses bounding-box Average Precision.

### $AP_{25}^{bb}$

-   Lower IoU requirement.
-   Authors interpret it as reflecting classification performance more
    strongly.

### $AP_{50}^{bb}$

-   **Primary detection metric**.
-   Balance of classification and localization.

### $AP_{75}^{bb}$

-   High IoU requirement.
-   Authors interpret it as reflecting localization accuracy more
    strongly.

Also reports: - Recall at fixed false positives per image.

## Segmentation

Uses:

$$
AP^{mask}
$$

with AP50 and AP75 also reported.

------------------------------------------------------------------------

# Main Results

SAR-Net is tested with several two-stage detection configurations: -
Faster R-CNN - Cascade R-CNN - [[Mask R-CNN]] - C4 / [[Feature Pyramid Network|FPN]] - [[Feature Pyramid Network|FPN]] + DCN

SAR-Net improves the baseline across the configurations reported in
Table 3.

### [[Mask R-CNN]] + [[Feature Pyramid Network|FPN]] + DCN on [[ChestX-Det.md|ChestX-Det]]

Baseline:

$$
AP_{50}^{bb}=45.0
$$

SAR-Net:

$$
AP_{50}^{bb}=47.7
$$

**+2.7 points**

For segmentation:

Baseline:

$$
AP_{50}^{mask}=38.3
$$

SAR-Net:

$$
AP_{50}^{mask}=40.6
$$

------------------------------------------------------------------------

# Stronger Backbones

Tested with: - ResNet-50 - ResNet-101 - ResNeXt-101

Relation modules improve results with all three backbones.

Example:

### ResNeXt-101

Baseline:

$$
AP_{50}^{bb}=46.5
$$

SAR-Net:

$$
AP_{50}^{bb}=48.9
$$

Shows that the modules are not tied to one particular backbone.

------------------------------------------------------------------------

# Ablation Study

Remove one relation module at a time to determine its contribution.

Full SAR-Net:

$$
AP_{25}=57.0,\quad AP_{50}=47.7,\quad AP_{75}=19.0
$$

## Spatial Relation Module

Removing SRM: - AP25: −0.0 - AP50: −0.4 - AP75: **−1.5**

Interpretation from authors: - Largest effect occurs at high IoU. -
Spatial relations mainly improve **localization**.

## Disease Relation Module

Removing DRM: - AP25: **−1.7** - AP50: −0.7 - AP75: −0.6

Interpretation: - Largest effect occurs at AP25. - Disease relationships
contribute more strongly to **classification**.

## Contextual Relation Module

Removing CRM: - AP25: −1.6 - AP50: **−1.1** - AP75: −1.3

Context contains both: - appearance information - spatial information

Therefore it contributes to both classification and localization.

------------------------------------------------------------------------

# Module Roles

  -----------------------------------------------------------------------
  Module                  Main Question           Main Contribution
  ----------------------- ----------------------- -----------------------
  Spatial                 Where is the disease    Localization
                          relative to anatomy?    

  Contextual              What other lung regions Classification +
                          help interpret this     localization
                          RoI?                    

  Disease                 What related diseases   Classification
                          provide useful          
                          evidence?               
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## Related Concepts

- [[Structure Aware Relation Network]] - the annotated working note for the same paper. It keeps the SAR-Net alias.
- [[Mask R-CNN]] - the detector this method extends.
- [[Feature Pyramid Network|FPN]] - the multi-scale backbone.
- [[PSP-Net]] - the pretrained model used for anatomical parts.
- [[ChestX-Det.md|ChestX-Det]] - the instance-labeled dataset introduced with the method.
- [[RoI Align]] - the operation that extracts a feature for each proposed region.


