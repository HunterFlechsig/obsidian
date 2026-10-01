---
type: paper
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
sources:
  - "[[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf]]"
---

# Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis

> [!info] Supporting Material
> - [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf]]

> [!summary] Summary
> The paper trains a chest X-ray detector when several experts disagree and no single ground truth exists. Weighted Boxes Fusion estimates a hidden label from the annotators' boxes. A second stage trains the detector with Experts Agreement Re-weighted Loss, so boxes with stronger annotator agreement count more. The notes report gains on a noisy MED-MNIST simulation and on VinDr-CXR.

> [!important] Key Points
> - Do not treat every expert box as equally trustworthy.
> - Stage one estimates the label with weighted boxes fusion. Stage two reweights the detection loss by agreement.
> - Majority voting is the contrast: it throws away uncertainty.
> - Evaluation uses mean average precision on MED-MNIST and [[VinDr-CXR.md|VinDr-CXR]].
> - The notes' limitation: the method is not end-to-end, and fusion under heavy noise can yield poor training boxes.

## Main Idea
> ([[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=1&annotation=558R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14105]])
>  approach for the enhancement of neural networks’ efficiency in abnormal detection tasks by estimating the actually hidden labels from multiple ones.
>  
>  Don't treat every label as equally trustworthy -> weighted loss


## Code
 https://github.com/huyhieupham/learning-from-multiple-annotators

### Abstract
- Labels are noisy, difference in opinion of expert's effect training
- MED-MNIST, VinRe-CXR

![[Pasted image 20260915205642.png]]
### Intro
- expensive to get high quality labels for medical imaging [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=1&annotation=561R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14105]]
- subjective labels by many clinicians *repeat labeling*
- Previous Approaches
	1. Curate labels [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=2&annotation=564R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14106]]
	2. *Truth inference* based on labels [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=2&annotation=567R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14106]]
- Problem
	- multiple annotators give set of nonstandard labels, no ground truth
- Solution
	- > ([[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=2&annotation=570R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14106]]) allow deep learning-based detectors to give an estimate of the actual hidden labels with the aim of improving their detection capacity of abnormalities from chest X-ray scans.
	- 2 steps
		1. [[Weighted Boxes Fusion Algorithm]] estimated true label -> *Truth inference*
		2. > ([[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=2&annotation=573R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14106]]) > The second stage, as shown in the right-handed side of Figure 1, is to train an object detector on estimated labels with a re-weighted loss function using implicit annotators’ agreement, which is represented by the estimated confidence scores -> Don't treat every label as equally trustworthy -> weighted loss
- Evaluate on simulated and real datasets: MED-MNIST, VinDR

## Related Works
EARNING FROM MULTIPLE ANNOTATORS
- Use Majority Voting, Issue: remove uncertainty, more bias [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=3&annotation=576R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14107]]
- Bring more info like annotator proficiency, annotators’ confusion matrix, difficulty
- NN learns from noisy labels using above [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=3&annotation=579R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14107]]
WEIGHTED TRAINING EXAMPLES
1. emphasize hard exmaples
2. emphasize easy exmaples
- Paper use re-weighted loss function according to consensus of annotators [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=3&annotation=582R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14107]]

## Method
**Estimating labels**
- start with (WBF) algorithm 
	- modify to combine bounding boxes with confidence score [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=3&annotation=585R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14107]]
- strain image detectors with a re-weighted loss function

**Training** 
- object detection multitask with 2 loss functions: localization, and classification -> [[one-stage anchor-based detectors]]
	![[Pasted image 20260915211559.png]]
- Experts Agreement Re-weighted Loss (EARL): use boxes confidence score [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=4&annotation=588R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14108]]
	![[Pasted image 20260915211610.png]]

**Experiments**
- use synthetic [[MED-MNIST]] and real-world [[VinDr-CXR.md|VinDr-CXR]]
	- MED-MNSIT: based on MNIST uses number boxed and labeled as if annotated, but adds noise to represent disagreement![[Pasted image 20260915212021.png]]

- Use [[VinDr-CXR.md|VinDr-CXR]]
	- Weird: [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=591R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]
	-  Rads-VinDr-CXR: dataset with the 3 rads that labeled most abonormalities

**Evaluation**
- use [[Standard mean average precision]][[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=594R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]
- Try several different thresh-holds to evaluate [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=597R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]

**IMPLEMENTATION [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=600R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]**
- YOLOv5-S 
- 2 2080 Ti's
-  640 × 640 pixels
- ALSO:  EfficientDet [58] with sizes D3 and D4 [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=603R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]

**Comaprison** 
- claim no other "multiple-annotators model for object detection tasks in the literature" [[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=6&annotation=606R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14110]]
- Validate against Baselines
	- all annotators are ground truth
	- an ensemble of independent models trained on separate radiologists’ annotation sets.

## Results
- outperformed baselines 

> ([[Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis.pdf#page=7&annotation=609R|Learning From Multiple Expert Annotators for Enhancing Anomaly Detection in Medical Image Analysis, p.14111]])
> on the test set of the MED-MNIST dataset, our method reports an overall mAP@0.4 of 0.980 and an overall mAP@[0.5:0.95:0.05] of 0.849. These results are much higher the performance of the baseline with mAP@0.4 = 0.975 and mAP@[0.5:0.95:0.05] = 0.815, TABLE 3. Experimental results on the VinDr-CXR dataset while EfficientDet is used as the detector. The scores are measured in mAP@[0.5:0.95:0.05], with highest values highlighted in red. boosting the mAP scores of the baseline by 0.51% and 4.2%

- better on Vin, and with EfficientDet 

"Simple but effective"
Limitations: Not end-to-end?? (update using [[back propagation]], Applying WBG on with lots of noise may create low quality trainig data, should be fine since notators are competent clinical experts

## Related Concepts

- [[VinDr-CXR.md|VinDr-CXR]] - the real multi-reader chest X-ray set.
- [[Standard mean average precision]] - the detection score used in the experiments.
- [[MED-MNIST]] - the synthetic disagreement set.
- [[Weighted Boxes Fusion Algorithm]] - the label-estimation step.
- [[one-stage anchor-based detectors]] - the detector family named for the two losses.