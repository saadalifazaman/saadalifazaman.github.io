---
title: "Biofouling & Corrosion Identification System Using Deep Learning"
excerpt: "Built a real-time hull inspection prototype using YOLOv8 segmentation variants on a self-collected fouling dataset and a public corrosion dataset. Achieving fouling detection F1 of 0.90 at confidence 0.68, with qualitative validation on both fouling and multi-class corrosion detection. Funded by RISE, BUET."
collection: portfolio
---

**Role:** Undergraduate Thesis: NAME 400 Project and Thesis (June 2023 – July 2024)  
**Supervisor:** Dr. Kazi Naimul Hoque, Assistant Professor, Dept. of Naval Architecture and Marine Engineering, BUET  
**Collaborator:** Ashraful Alam Suny  
**Funding:** Research and Innovation Centre for Science and Engineering (RISE), BUET  
**Submitted:** June 2024, in partial fulfillment of the B.Sc. in Naval Architecture and Marine Engineering

---

## Background and Motivation

Marine biofouling is the accumulation of organisms such as barnacles, algae, and microbial slime on ship hulls. This is one of the most persistent and costly challenges in maritime operations. As fouling organisms colonize the hull surface, they increase roughness and frictional resistance, forcing vessels to consume more fuel to maintain speed. Even a thin slime layer covering half the underwater hull can intensify greenhouse gas emissions by approximately 25%
(GloFouling, 2022), and heavy fouling growth can raise fuel consumption by as much as 40% ([Wu et al., 2022](https://www.sciencedirect.com/science/article/abs/pii/S0029801825036704)). Steel corrosion compounds the problem further. If corrosion is left untreated, it degrades structural integrity independently of fouling, accelerating the need for costly drydocking.

The economic scale of this problem is significant. The overall cost associated with hull fouling for a single naval vessel class has been estimated at USD 56 million per year or USD 1 billion over 15 years (Schultz et al.,[*Biofouling*, 2011](https://doi.org/10.1080/08927014.2010.542809)). For commercial
shipping, a fouled hull can cost a company several million dollars per drydocking period, primarily through increased fuel expenditure and hull maintenance
(Wu et al., 2022). Periodic hull cleaning has been shown to reduce daily fuel consumption by approximately 9–17%, with drydock cleaning yielding greater
reductions than underwater cleaning ([Adland et al., 2018](https://doi.org/10.1016/j.jclepro.2017.12.247)).

Early detection of fouling and corrosion is therefore desirable, before hull performance is significantly affected. The conventional response is periodic
drydock inspection. This requires trained personnel to visually assess hull condition and prescribe cleaning or repainting. But this approach is labor-intensive, time-consuming, subjective, and expensive. Traditional survey methods also rely on diver inspections in underwater settings, which are hazardous and inherently limited in scalability. By the time a scheduled inspection occurs, fuel penalties from accumulated fouling have already been paid for weeks or months.

Automated computer vision offers a path toward condition based maintenance, rather than schedule based maintenance. If a model can reliably detect and localize fouling and corrosion from hull images, inspection frequency can increase, cleaning decisions become quantifiable, and the subjectivity of human visual assessment is reduced. However, two practical obstacles made this non-trivial at the time of this study.

First, no publicly available deep learning model existed for the simultaneous detection of both biofouling and corrosion. Prior work addressed either fouling
severity classification on large expert labeled datasets or corrosion detection in industrial settings. But not both classes together in a unified segmentation
framework applicable to drydock inspection.

Second, no suitable training dataset existed for Bangladesh's dockyard environment. Fouling community composition, hull paint types, lighting conditions, and inspection geometry in Bangladeshi dockyards differ meaningfully from datasets collected in European or controlled laboratory settings. Training on mismatched data would produce a model that generalizes poorly to the actual deployment environment.

This thesis addresses both gaps. Building a domain-specific dataset through direct field collection at Narayanganj Dockyard, Bangladesh, then training and evaluating YOLOv8 instance segmentation variants to detect both fouling and corrosion under real drydock conditions.

---

## Objectives

1. Build a domain-specific image dataset suitable for training deep learning models to identify both biofouling and corrosion simultaneously
2. Modify and fine-tune existing instance segmentation architectures for this multi-class detection task
3. Determine the best-performing model variant for measuring the extent and severity of fouling and corrosion damage in a given area
4. Enable real-time processing of video images to immediately locate affected hull areas

---

## Dataset

Two data sources were combined:

**Fouling images — self-collected:**
- A total of 36 images were collected from Dockyard & Engineering Works Ltd., Narayanganj, Bangladesh
- 35 images contained a clear presence of fouling; 1 contained an aerial view of the site and was excluded from training
- Images captured at various angles, lighting conditions, times of day, and backgrounds to maximize diversity
- Split: 27 images (77%) training, 8 images (23%) test

**Corrosion images — public dataset:**
- Downloaded from the GitHub repository [beric7/corrosion_cs_classification](https://github.com/beric7/corrosion_cs_classification) (bridge inspection corrosion dataset)
- Total: 440 images across three corrosion severity classes: Fair Steel Corrosion, Poor Steel Corrosion, Severe Steel Corrosion
- Split: 396 images (90%) training, 44 images (10%) test

**Annotation pipeline for fouling images:**
- Manual polygon annotation using [CVAT (Computer Vision Annotation Tool)](https://cvat.ai) a free, open-source, web-based annotation platform
- CVAT does not natively export to YOLOv8 segmentation format; annotated images were downloaded in segmentation mask 1.1 format, then converted to YOLOv8 `.txt` format using the `masks_to_polygons.py` script from the [computervisioneng/image-segmentation-yolov8](https://github.com/computervisioneng/image-segmentation-yolov8) repository

**Annotation pipeline for corrosion images:**
- The public corrosion dataset was pre-annotated using Labelme software
- Converted from Labelme JSON format to YOLO format using the [natepolizogo/labelme2yolo](https://github.com/natepolizogo/labelme2yolo) conversion tool
  
**dataset.yaml classes (nc: 4):**  
`Fair_Steel_Corrosion`, `Poor_Steel_Corrosion`, `Severe_Steel_Corrosion`, `Fouling`

---

## Model: YOLOv8

**YOLOv8** ([Ultralytics, 2023](https://docs.ultralytics.com/models/yolov8/)) was selected as the primary architecture for its anchor-free detection head (reducing box predictions and improving performance on irregular fouling shapes), CSPDarknet53 backbone with C2f modules for multi-scale feature integration, FPN neck with SPPF for contextual feature capture across scales, and COCO-pretrained weights enabling transfer learning to reduce underfitting risk on the small dataset. All five segmentation variants were evaluated: YOLOv8n-seg through YOLOv8x-seg.

Training was conducted on [Google Colab](https://colab.research.google.com) and [Kaggle](https://www.kaggle.com) (GPU-accelerated). Google Colab's free tier could not accommodate combined fouling+corrosion training due to session time limits; Kaggle (limited to 30 GPU-hours/week) enabled joint training from 500 to 1000 epochs.

---

## Experimental Cases

Seven configurations were tested progressively, varying dataset composition, model size, and epoch count:

| Case | Dataset | Model | Epochs | All-Class F1 (peak) |
|---|---|---|---|---|
| 01 | 43 corrosion | YOLOv8s | 50 | 0.16 at conf. 0.110 |
| 02 | 43 corrosion + 27 fouling | YOLOv8s | 50 | 0.25 at conf. 0.702 |
| 03 | 43 corrosion + 27 fouling | YOLOv8l | 100 | 0.29 at conf. 0.047 |
| 04 | 43 corrosion + 27 fouling | YOLOv8l | 500 | 0.31 at conf. 0.597 |
| 05 | 395 corrosion only | YOLOv8l | 50 | 0.34 at conf. 0.168 |
| **06** | **395 corrosion + 27 fouling** | **YOLOv8l** | **500** | **0.39 at conf. 0.154** |
| 07 | 395 corrosion + 27 fouling | YOLOv8l | 1000 | 0.37 at conf. 0.115 |

**Case 06** produced the best overall multi-class performance and was used for all qualitative result illustrations.

---

## Key Results

### Fouling-only training (all five YOLOv8 variants, 500 epochs, 27 images)

| Model | Peak F1 | Confidence |
|---|---|---|
| YOLOv8n-seg | 0.85 | 0.686 |
| YOLOv8s-seg | 0.88 | 0.706 |
| YOLOv8m-seg | 0.82 | 0.753 |
| **YOLOv8l-seg** | **0.90** | **0.680** |
| YOLOv8x-seg | 0.85 | 0.724 |

**YOLOv8l-seg achieved the best fouling detection F1 of 0.90 at confidence 0.68.** The larger YOLOv8x-seg (71.8M parameters) does not surpass it, likely due to mild overfitting on only 27 training images, a direct illustration of the small-data problem that motivated the subsequent RA research.

### Findings across all seven cases

**More training data improves performance.** Adding fouling images to the corrosion-only dataset (Case 01 → Case 02) raises the all-class peak F1 from 0.16 to 0.25. Adding the full 395-image corrosion dataset (Case 04 → Case 06) raises it further to 0.39.

**More epochs improve stability up to a point.** Optimal multi-class performance occurs at 500 epochs (Case 06). Training to 1000 epochs (Case 07) shows diminishing returns — the all-class F1 drops slightly to 0.37, though performance stabilizes further for fouling and severe steel corrosion individually.

**Fouling class consistently outperforms all corrosion classes.** The fouling images were self-collected and self-annotated by the authors — label quality is controlled. Corrosion images from the public dataset carry pre-existing labeling inconsistencies, particularly for Severe Steel Corrosion, which peaks at low confidence and drops rapidly in all cases.

**Corrosion detection remains an open challenge.** This thesis establishes that YOLOv8 can detect corrosion in principle, but achieving reliable multi-class corrosion classification requires higher-quality annotated data than what was publicly available at the time.

---

## Qualitative Results

The trained model (Case 06) produces reliable fouling segmentation masks under varied hull conditions. For corrosion, Fair and Poor Steel Corrosion are detected reasonably well; Severe Steel Corrosion presents the greatest challenge, muddy surfaces and low-contrast textures, are sources of false positives.

<video width="100%" controls>
  <source src="/assets/media1.mp4" type="video/mp4">
</video>

> *Upload Figures 3.10 and 3.11 from your thesis to `images/` and replace this block:*  
> `![Fouling - raw, ground truth, prediction](../images/thesis-fig310-fouling-results.jpg)`  
> `*Raw images, ground truth masks, and model predictions for fouling detection (Case 06, YOLOv8l)*`  
>  
> `![Corrosion - raw, ground truth, prediction](../images/thesis-fig311-corrosion-results.jpg)`  
> `*Raw images, ground truth masks, and model predictions for Fair, Poor, and Severe Steel Corrosion (Case 06, YOLOv8l)*`

---

## Limitations

- Corrosion detection accuracy is low, particularly for Severe Steel Corrosion — labeling errors in the public corrosion dataset are a likely cause; future work should audit and correct these labels
- Study is limited to YOLOv8 variants; newer YOLO versions and other architectures were not evaluated
- Computational constraints (Colab and Kaggle free-tier limits) restricted hyperparameter search and training scale
- No systematic data augmentation analysis — which augmentations help, in what order, and with what interaction effects was entirely unknown at this stage
- Only one fouling type was collected; diverse fouling communities and other corrosion types (galvanic, pitting, crevice) were not separately represented
- Real-world industrial deployment and validation with industry partners remain pending

---

## What This Led To

Two gaps identified here directly shaped the subsequent RA research:

**Data scarcity** — with only 27 usable fouling training images, the core constraint was not the model but the data. If more field images are not obtainable, can augmentation be made as principled and effective as possible?

**No augmentation framework** — augmentation was applied informally; there was no method to determine which augmentations to use, in what order, or how pairs of augmentations interact. The follow-on RA work addressed this directly through the Interaction-Effect metric and k–n Fold ACV protocol.

The 35 fouling images collected for this thesis became the core of the NDBD dataset used in the RA-level research. See the [Dataset Collection entry](/portfolio/00-dataset-collection-narayanganj/) and the [RA Research entry](/portfolio/01-research-assistant-biofouling-IE-metric/) for those contributions.
