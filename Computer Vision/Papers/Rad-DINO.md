---
type: paper
topics:
  - "[[Image Analysis]]"
status: complete
updated: 2026-09-30
sources:
  - "[[Rad-DINO.pdf]]"
Class: cse-507
---

# Rad-DINO

> [!info] Supporting Material
> - [[Rad-DINO.pdf]]

> [!summary] Summary
> RAD-DINO is a medical image encoder trained on chest X-rays without text reports or labels. It adapts DINOv2's self-supervised learning to medical imaging. The paper argues that radiology reports contain only part of what an image shows, so text supervision can make a model ignore visible detail. Image-only training matches or beats several image-text models on classification, segmentation, and report generation, and it scales with more images.

> [!important] Key Points
> - Text supervision is not necessary to learn strong medical image representations.
> - Self-supervised image-only learning can outperform several language-supervised medical encoders.
> - Combining masked image modeling with instance discrimination produces representations that work well across classification, segmentation, and report generation.
> - Medical reports contain only a subset of the information present in an image, so relying on them during pretraining can discard useful visual information.
> - Image-only pretraining is easier to scale because large collections of medical images are more available than paired image-report datasets.

## Introduction
- text supervision prevelent
- Nature of Medical data not good for paired text, image encoders - hard to get info because anonymity. hard to scale [[Rad-DINO.pdf#page=2&annotation=1816R|Rad-DINO, p.2]]
- limiting, may lack findings, or observations. Not all details captured by radiologist reports [[Rad-DINO.pdf#page=2&annotation=1819R|Rad-DINO, p.2]]
- Don't need text supervision to learn the features [[Rad-DINO.pdf#page=2&annotation=1822R|Rad-DINO, p.2]]
- Can perform many downstream tasks [[Rad-DINO.pdf#page=2&annotation=1825R|Rad-DINO, p.2]]
	- unimodel: classification, loc, seg
	- multi-model: combines multiple types of data, xray + annotation
	- image level, pixel level(seg)
- [[Masked Image Modeling]]: Parts of an image are hidden, and the model learns to predict representations of the hidden patches. This encourages it to understand **local details and textures**.
- **[[Self-supervised instance discrimination]]** The model sees different crops/views of the same X-ray and learns that they represent the **same underlying image**, helping it learn more global information.
- Baseline against other medical image encoders: 
	- Classification: matched or surpassed [[Rad-DINO.pdf#page=2&annotation=1828R|Rad-DINO, p.2]]
	- Seg: "promising" using "off-the-shelf decoder heads" without lots of annotated data [[Rad-DINO.pdf#page=2&annotation=1831R|Rad-DINO, p.2]]
	- Can predict demographic info from encoding  more accurately -> more useful? [[Rad-DINO.pdf#page=2&annotation=1834R|Rad-DINO, p.2]]
- Ablation study
#### Contributions
- can employ self supervised learning to with no text to get comparable or better results to other text encoders
- show from ablation, it scales with more data and can continue to improve as more data is available
- features shoe stronger correlation between patient info

## Results
- linear probing used to compare approaches
- Datasets Tested: VinDr-CXR, CANDID-PTX and RSNA-Pneumonia

### VinDr-CXR benchmark
- **RAD-DINO outperforms the other models on 5/7 pathologies and has the highest aggregate performance.**
- **Exceptions: cardiomegaly (CM) and aortic enlargement (AE).** The authors hypothesize that these are large structures with clear borders that are well described in radiology reports, so **language-supervised models benefit more from text supervision for these findings**.
- **RAD-DINO does especially well on pulmonary fibrosis (PF) and tuberculosis (TB).** These are rarely mentioned in MIMIC-CXR reports (<1%), and these are the two classes where RAD-DINO shows its largest improvement over the other models.
- **Dataset diversity matters.** RAD-DINO was trained on a more diverse collection of chest X-rays, while MRM was trained only on MIMIC-CXR. The authors argue this helps RAD-DINO generalize to chronic/incidental and outpatient findings.
- **Important takeaway:** This benchmark supports the paper's central argument that **text supervision isn't required for strong medical image representations**, particularly when the relevant visual information is poorly represented in radiology reports.

### 2.1.3 CANDID-PTX & RSNA-Pneumonia

- **Tests generalization to external datasets and more localized findings**, such as pneumothorax and chest tubes.
- **RAD-DINO performs particularly well on CANDID-PTX**, especially for pneumothorax and chest-tube detection.
- **Image resolution is important for small/subtle findings.** Performance drops at lower resolutions because fine details can be lost, although RAD-DINO still outperforms image-text contrastive models at 224 px.
- **Rib fractures remain difficult**, partly because <2% of examples are positive and fractures can require very fine-grained visual information.
- **RAD-DINO performs similarly to the strongest models on RSNA-Pneumonia.** The authors suggest the smaller advantage may be because pneumonia/opacities are common and well described in existing datasets and reports.

**Key takeaway:** Image-only learning seems particularly useful for **localized/subtle findings**, but sufficient image resolution is important.

---

### 2.1.4 Lateral CXR scans

- **Previous experiments only used frontal X-rays**, but lateral X-rays can reveal abnormalities that may be difficult to see from the frontal view.
- **Frontal and lateral images often share the same radiology report**, even though some reported findings may not actually be visible in the lateral image.
- This creates **noisy image-text supervision**: the text may describe something that isn't visible in the particular image being paired with it.
- **MIM-based models (RAD-DINO and MRM) substantially outperform CLIP-style models** when detecting abnormalities visible only in lateral scans.

**Key takeaway:** This is another example of **text supervision potentially becoming a limitation** when the report and image don't perfectly correspond.

---

### 2.1.5 Impact of learning objectives

- **RAD-DINO learns local textures and details**, which the authors attribute to its combination of masked image modeling (MIM) and multi-crop instance discrimination.
- **MIM helps learn high-frequency/local information**, such as fine textures and small structures.
- **Instance discrimination helps learn more global information**, such as overall shapes and structures.
- RAD-DINO's **patch embeddings can identify corresponding abnormalities/anatomical structures across X-rays from different patients**. Figure 2 shows examples involving consolidation and a lung nodule.
- **CLIP-style models learn pneumonia relatively quickly**, which the authors suggest is because pneumonia-associated findings such as opacities are common and well described in radiology reports. This helps explain why the performance gap is smaller for pneumonia.

**Key takeaway:** RAD-DINO's two objectives are **complementary: MIM captures fine/local information while instance discrimination captures broader/global information**, producing features useful across different medical imaging tasks.

## Evaluating RAD-DINO for report generation from images

- Generate findings report from CXR

- **Main question:** Is image-text alignment during pre-training actually necessary for a later **vision-language task**, specifically generating the _Findings_ section of a chest X-ray report?
- **MIMIC-CXR is used to train the report generator.** After filtering to frontal X-rays with Findings sections, they use **146,909 training / 7,250 validation / 2,461 test image-text pairs**.
- **IU-Xray is an external test set.** Neither the RAD-DINO image encoder nor the language decoder was trained on it, so it tests how well the system **generalizes to unseen data**.
- **The image encoders are frozen.** They don't update RAD-DINO or the competing encoders during report-generation training. This makes the comparison focus on the **quality of the representations each encoder already learned during pre-training**.
- They use a **LLaVA-style architecture:** `X-ray → frozen image encoder → patch embeddings → MLP projector → Vicuna-7B → generated report`.
- The **MLP projector** translates the image encoder's representations into a form that the language model can use. The projector and Vicuna language model are trained, while the image encoder stays frozen.
- **They deliberately give the LLM minimal extra information.** This prevents additional clinical context from hiding differences in image-encoder quality.
- **MRM is excluded** because its encoder was already trained on all MIMIC-CXR image-text pairs, which would make the comparison unfair.

### Evaluation metrics

- **ROUGE-L and BLEU-4:** Measure **word/text overlap** between the generated report and the real report. Higher means the generated wording more closely resembles the reference report.
- **RGER:** A radiology-specific evaluation metric intended to assess generated reports beyond simple word overlap.
- **Macro-F1-14:** Uses CheXbert to evaluate whether the generated report correctly describes **14 clinical findings**. This is particularly important because a report could use different wording from the reference but still be clinically correct.

**Key takeaway:** They keep the downstream setup essentially the same while swapping the **image encoder**. This lets them test their central question: **Can an image-only pretrained encoder like RAD-DINO provide better visual information for report generation than encoders that were explicitly pretrained using image-text alignment?**

### 2.2.2 Results Analysis

- **RAD-DINO performs best on every evaluated metric on MIMIC-CXR**, including both lexical and clinical/factual metrics.
- On the external **IU-Xray dataset, RAD-DINO performs best on all but one lexical metric**, showing that its representations also generalize to a dataset not used to train either the encoder or language decoder.
- **RAD-DINO outperforms specialized language-supervised encoders** such as BiomedCLIP, BioViL-T, and CheXzero, even though RAD-DINO itself **never used text during image pre-training**.
- The particularly strong **Macro-F1-14** result suggests RAD-DINO's embeddings capture relevant pathologies well, allowing the language model to produce **more factually correct findings**, not just reports with similar wording.
- This supports the paper's argument that **image-text alignment during image pre-training isn't required for later vision-language tasks**. Strong visual features can first be learned from images alone and then connected to language afterward.
- The authors also suggest **higher image resolution may be important for vision-language performance**, potentially more important than simply increasing the number of visual tokens.

**Key takeaway:** RAD-DINO shows that an **image-only encoder can later support strong report generation**, and may actually provide the LLM with better visual representations than encoders originally trained to align images with text.

### 2.2.3 Balancing Training Datasets

- The authors ask an important question: **Is RAD-DINO better simply because it was trained on more and more diverse X-rays?**
- To control for this, they create **RAD-DINO-Control**, trained **only on MIMIC-CXR**, rather than RAD-DINO's much larger Multi-CXR dataset.
- This provides a fairer comparison with models such as **BioViL-T**, which was also trained using MIMIC-CXR.
- **RAD-DINO-Control still outperforms the other encoders**, suggesting RAD-DINO's advantage isn't simply caused by having access to more radiology images. The authors attribute some of the improvement to the **image-only self-supervised training method itself**.
- Interestingly, there is only a **small difference between RAD-DINO-Control and full RAD-DINO** on MIMIC-CXR report generation.
- The authors suggest this is because RAD-DINO-Control was trained on **the same distribution as the report-generation training/test data (MIMIC-CXR)**, so additional dataset diversity provides less of an advantage here.

**Key takeaway:** RAD-DINO's report-generation advantage **cannot be explained only by its larger training dataset**. Even when trained on comparable in-domain data, image-only DINO-style pre-training produces strong representations for later vision-language tasks.

## Evaluating RAD-DINO on segmentation benchmarks [[Rad-DINO.pdf#page=7&annotation=1846R|Rad-DINO, p.7]]

### Key Takeaways
> This experiment asks whether RAD-DINO learns **where structures and abnormalities are located**, rather than only learning whether they exist.
> 
> Text supervision can produce useful global representations while failing to preserve the **fine local information needed to identify exact locations and boundaries**.
> 
> RAD-DINO already contains strong spatial information, but **the decoder determines how effectively that information gets converted into precise segmentation masks**. Strong pre-training + a good multi-scale decoder gets surprisingly close to specialized segmentation networks.


- used CXR dataset for anatomy and pathology segmentation
- try using several different encoder decoders
- Enough information in patch/local info for seg
- Freeze encoder -> train decoder on top
- Choose sever al decoders
	- linear: simple. Useful for seeing how much segmentation information is directly available
	- **ViTDet and UPerNet:** More capable decoders that can combine features to produce better segmentation masks.
- compare against baselines trained specifically for task NN-U-Net and EfficientNet-B6
- **[[Dice score]]**, which measures overlap between the predicted segmentation and the ground-truth segmentation. Higher is better.

- CLIP models perform poorly, using linear encoder
- **The performance gap becomes larger for small structures**, such as chest tubes, where precise local information is especially important.

**Role of [[Masked Image Modeling|MIM]]**


**Role of Encoder-Decoder Choice**
- Dice increases by from **53.4 with a linear decoder → 65.8 with UPerNet** , [[Feature Pyramid Network|FPN]] based encoder
- Becomes competitive when using FPN decoders
- using **hierarchical/multi-scale encoder such as Swin Transformer** could potentially improve segmentation further.

## Methods and Experimental Setup
[[DINOv2]]

>RAD-DINO inherits [[DINOv2]]'s ability to learn **both global and local visual information without labels or text**.
>
>The baseline selection is designed mainly to answer **"Do we actually need text supervision?"**, rather than "Is DINOv2 the best possible SSL algorithm?"
>
>They deliberately evaluate RAD-DINO across **global, local, and multimodal tasks** to see whether one image-only encoder can transfer to very different medical applications.
>
> The evaluation is designed to test **generalization while avoiding patient leakage**, with metrics chosen for the characteristics of each task.
>
>This experiment provides direct support for one of the paper's central arguments: **RAD-DINO appears to preserve visual information that language-supervised models may lose when that information isn't represented in the accompanying text.**


- start from pretrained [[DINOv2]]
- Full training dataset is 838k images
- Images kept same size throughout training because less data and training time [[Rad-DINO.pdf#page=9&annotation=1867R|Rad-DINO, p.9]]
- modify DINOv2's **image augmentations for chest X-rays**. They use larger crops and less severe blurring because medical findings depend heavily on preserving texture and surrounding anatomical context. [[Rad-DINO.pdf#page=9&annotation=1870R|Rad-DINO, p.9]]
- trying to compare to CLIP not all SSL algs

**Downstream** 
- The authors want to determine whether RAD-DINO learns both **global and local visual information**, so they evaluate it across several different kinds of tasks.
- They use **frozen image encoders** for these experiments. This helps test the quality of the representations learned during pre-training rather than allowing extensive fine-tuning to compensate for weak representations. [[Rad-DINO.pdf#page=9&annotation=1873R|Rad-DINO, p.9]]

**Evaluation**
- Patient-level splitting is used: all images belonging to one patient stay within the same train/test/validation split. This prevents **data leakage**, where images from the same patient could otherwise appear in both training and testing.
- Classification uses external datasets: test generalizability
- **AUPRC** because the pathologies are highly imbalanced
- Segmentation performance uses **Dice score**

### RAD-DINO can extract patient demographics
- ask whether an image encoder retains information that is **visible in the X-ray but usually isn't written in the radiology report**.
- examine **sex, age, weight, and BMI**.
- hypothesis **language-supervised encoders may discard some of this information** because it generally isn't included in the text they're trained to align with.
- RAD-DINO substantially outperforms BioViL-T and BiomedCLIP for all four demographic variables.

## Results and conclusion
>**Main conclusion:** High-quality general-purpose medical image encoders **can be trained using images alone**, without paired radiology reports. RAD-DINO achieves comparable or better performance than several language-supervised models across the paper's benchmarks.
>
>**Text supervision can potentially limit visual representations.** Radiology reports don't describe everything visible in an image, so forcing images to align with reports may cause the encoder to ignore visual information that isn't mentioned.
>
>**RAD-DINO preserves information beyond what is normally written in reports**, demonstrated by its stronger association with patient information such as age, sex, weight, and BMI.
>
>The paper's broader argument is not that **text is useless** in medical AI. It's that **text may not be necessary, or ideal, for initially learning the visual encoder**. RAD-DINO suggests you can first learn a rich representation directly from large amounts of medical imaging data, then connect that representation to classifiers, segmentation models, or language models depending on the downstream task.****


### Scalability
- major advantage is **access to much more training data**. Medical images without accompanying reports are easier to obtain than clean image-text pairs, making image-only SSL easier to scale.
- Extended to areas were less paired text, **histopathology, ultrasound, and 3D CT**
- **Image resolution remains important**

### Broader Goal
- **One strong general-purpose image encoder** and attaching different task-specific components to it.
- like ARK

## Related Concepts

- [[DINOv2]] - the self-supervised method Rad-DINO adapts.
- [[Masked Image Modeling]] - the local objective whose removal hurts segmentation.
- [[Feature Pyramid Network|FPN]] - the decoder used on the encoder for segmentation.
- [[Dice score]] - a segmentation metric reported for this encoder.
- [[Self-supervised instance discrimination]] - the global objective paired with masked image modeling.

