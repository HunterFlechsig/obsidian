---
type: paper
topics:
  - "[[Image Analysis]]"
status: review
updated: 2026-09-30
aliases:
  - Metrics Reloaded
sources:
  - "[[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf]]"
Class: cse-507
---

# Metrics Reloaded

> [!info] Supporting Material
> - [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf]]

> [!summary] Summary
> Metrics Reloaded is a framework for choosing biomedical image-analysis metrics from the problem rather than from habit. A problem fingerprint describes the task, data, target structures, algorithm output, and domain priorities, then maps onto image-level classification, semantic segmentation, instance segmentation, or object detection. The recommendations come from a Delphi-style process with researchers across many institutions. This note is the longer write-up. [[Metrics Reloaded - FROC (short)]] still holds a shorter copy of the opening, and neither note defines the FROC metric.


> [!important] Key Points
>
> - Metric choice is part of the scientific problem, not an afterthought. A metric should be chosen based on what matters in the biomedical application rather than simply because it is commonly used.
> - Problem fingerprinting: Before selecting metrics, describe the problem in a structured way, including the biomedical/domain interest, target structures, input dataset, image dimensionality, and algorithm output.
> - Categorize the task: The framework currently covers four major image-analysis problems: image-level classification, semantic segmentation, instance segmentation, and object detection.
> - Fingerprint → metric selection: Once the task is characterized, a category-specific mapping identifies suitable metrics while accounting for potential problems those metrics may have for the particular application.
> - Focus on the biomedical question rather than the algorithm. Problems are grouped according to their validation similarity, abstracting away from which particular model or algorithm produced the prediction.
> - Practical guidance for researchers: The framework is intended to turn knowledge about metric limitations into an actionable metric-selection process, including for researchers who are not validation experts.
> - Figure 1 on page 2 captures the whole idea: Biomedical problem → problem fingerprint → task category → category-specific metric pool/mapping → recommended metrics.

> [!PDF|187, 97, 229] [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=6&annotation=3803R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.6]]
> > The mission of Metrics Reloaded is to foster reliable algorithm validation through problem-aware choice of metrics with the long-term goal of (1) enabling the reliable tracking of scientific progress and (2) bridging the current chasm between AI research and translation into biomedical imaging practice
> 


> [!danger] Presntation Ideas
> - Choose a flow based on ark or foundation
> - fig 4 could work too

## Introduction
- Lack objective performance metrics that medical treatments have [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3785R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]
- Lack of relevant metrics is could be reason AI not making to clinic [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3788R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]
- Hard to choose correct metric because it based on many factors [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3791R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]
#### pitfalls
- **Inappropriate phrasing of the problem**: metric doesn't always reflect biomedical need  ([[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3794R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]) Dice Similarity Coefficient ( DSC) is frequently chosen as a metric for object detection problems [ 19, 45] although it does not reflect the critical need of detecting as many structure instances as possible
- **Poor metric selection**: ([[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3797R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]) Mathematical metric properties are often neglected, for example when using the DSC in the presence of particularly small structures
- **Poor metric application**: use on wrong dataset, ([[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=4&annotation=3800R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.4]]) ignoring hierarchical data structure, as in data from multiple hospitals or a variable number of images per patient (Fig. 1a, bottom), when aggregating metric values.

![[Pasted image 20260916170106.png]]

- make better, educated desicisons on what metric to choose [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=6&annotation=3809R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.6]]
- **problem fingerprinting** - structured representation of problem
> ([[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=6&annotation=3818R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.6]])
> domain interest-related properties, such as the particular importance of structure boundary, volume or center, target structure-related properties, such as the shape complexity or the size of structures relative to the image grid size, data set-related properties, such as class imbalance, as well as algorithm output-related properties, such as the theoretical possibility of the algorithm output not containing any target structure.

- Then guided through metrics and what risks with each could be

**Problem Categories**
> ([[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=6&annotation=3821R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.6]])
> problem categories: image-level classification, semantic segmentation, object detection or instance segmentation. 

- Designed to be modality independent, can be used in other fields


## Results
- use [[Delphi Process]] to iteratively improve across 5 international workshops, 6 questionnaires, numerous expert group meetings and social media-based crowdsourced feedback processes, all conducted over the past two years.

![[Pasted image 20260916171249.png]]

**Historically grown practices are not always justified**
- mAP inconsistent [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=10&annotation=3824R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.10]]

**Cross-domain approach enables integration of distributed knowledge**
- Aggregate many peoples knowledge [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=10&annotation=3827R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.10]]
### Design decisions  
1. Encapsulating domain knowledge: questions generally same [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=10&annotation=3830R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.10]]
2. Exploiting synergies across classification scales: 
3. Exploiting complementary metric strengths: fill in weakness of metrics with strengths of other in reccomendation. Include standard metrics as well as metrics that have not received much attention [[Metrics reloaded recommendations for image analysis validation_Unknown-1.pdf#page=11&annotation=3833R|Metrics reloaded recommendations for image analysis validation_Unknown-1, p.11]] : Net Benefit (NB ), Expected Cost ( EC )
4. Abstracting from methodology: only based on problem not algorithm
5. Involving and educating users: take user through process, shows pros and cons. make better decisions

### Problem fingerprints encapsulate relevant domain knowledge

1. General Properties: problem category
2. Domain interest-related properties: user preference, application specific
3. Target structure-related properties: size, shape, etc
4. Data set-related properties: class skew, annotation unncertainty
5. Algorithm output-related properties

### Metrics Reloaded addresses all three types of metric pitfalls
how it addresses each [[#pitfalls]]
1. fingerprinting: Inappropriate phrasing of the problem 
2. Metric Selection

## Related Concepts

- [[Metrics Reloaded - FROC (short)]] - shorter notes on the same framework, still a separate file.
- [[Image Analysis]] - the task categories the fingerprint maps onto.
- [[Standard mean average precision]] - one metric these notes say is applied inconsistently.
- [[Delphi Process]] - the iterative expert process used to build the recommendations.

