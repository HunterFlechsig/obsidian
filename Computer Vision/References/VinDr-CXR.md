---
type: reference
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
aliases:
  - VinDr CXR
sources:
  - "[[VinDr-CXR.pdf]]"
  - "[[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf]]"
---

# VinDr-CXR

> [!info] Supporting Material
> - [[VinDr-CXR.pdf]]
> - [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf]]

> [!summary] Summary
> VinDr-CXR is a public chest X-ray dataset with radiologist boxes and global disease labels, released so detection models are not limited to image-level tags. Of more than 100,000 collected scans, 18,000 are released: 15,000 for training and 3,000 for testing. Training scans were labeled independently by 3 radiologists. Test scans were labeled by a consensus of 5.

> [!important] Key Points
> - 22 local labels are rectangles around abnormalities. 6 global labels are suspected diseases.
> - 17 radiologists annotated the released set. Images are DICOM.
> - Training: 15,000 scans, 3 independent readers. Test: 3,000 scans, consensus of 5.
> - The multi-annotator paper flags one imbalance: 94.28% of abnormal training scans (3,315 of 3,516) were annotated by one group of three radiologists.
> - Abstract via the VinDr-CXR paper, page 1.

> [!PDF|255, 208, 0] [[VinDr-CXR.pdf#page=1&annotation=387R|VinDr-CXR, p.1]]
> Most of the existing chest X-ray datasets include labels from a list of findings without specifying their locations on the radiographs. This limits the development of machine learning algorithms for the detection and localization of chest abnormalities. In this work, we describe a dataset of more than 100,000 chest X-ray scans that were retrospectively collected from two major hospitals in Vietnam. Out of this raw data, we release 18,000 images that were manually annotated by a total of 17 experienced radiologists with 22 local labels of rectangles surrounding abnormalities and 6 global labels of suspected diseases. The released dataset is divided into a training set of 15,000 and a test set of 3,000. Each scan in the training set was independently labeled by 3 radiologists, while each scan in the test set was labeled by the consensus of 5 radiologists. We designed and built a labeling platform for DICOM images to facilitate these annotation procedures. All images are made publicly available in DICOM format along with the labels of both the training set and the test set.

## Notes

- Largest public chest X-ray database with radiologist-generated annotations, as stated in the local notes.
- 18,000 chest X-ray scans, with 15,000 for training and 3,000 for testing.
- One oddity: 94.28% of the abnormal scans in the training set (3,315 out of 3,516) were annotated by a group of three radiologists. [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=591R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]

## Related Concepts

- [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis]] - uses VinDr-CXR as the real multi-reader detection set.
- [[Image Analysis]] - the detection and localization tasks the boxes are for.
