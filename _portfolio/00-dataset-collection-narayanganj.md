---
title: "Field Data Collection: Narayanganj Dockyard Biofouling Dataset (NDBD)"
excerpt: "Independently collected and curated the first publicly available ship hull biofouling dataset from an active industrial dockyard in Bangladesh. 35 high-resolution images, 92 annotated instances, CC BY-NC 4.0 license, foundational to two subsequent research projects."
collection: portfolio
---

**Role:** Primary data collector and dataset curator  
**Location:** Dockyard & Engineering Works Ltd., Narayanganj, Bangladesh  
**Period:** 2023 (supporting Undergraduate Thesis and subsequent RA research)  
**Dataset License:** Creative Commons Attribution-NonCommercial 4.0 (CC BY-NC 4.0)  
**Dataset:** [GitHub — NDBD](https://github.com/saadalifazaman/NDBD)

---

## Why This Dataset Had to Be Built From Scratch

Marine biofouling datasets for computer vision-based inspection remain scarce because of the high cost, logistical complexity, and time-intensive collection 
processes. Dockyard environments are even more constrained. Operational safety requirements, privacy regulations, and restricted access in active facilities 
make data collection especially difficult. Existing public datasets in the literature mainly target marine debris, underwater organisms, or infrastructure fouling.
But few capture real dockyard hull surfaces exhibiting the operational variabilities that define actual inspection conditions: human occlusions, inconsistent 
lighting, and oblique viewing angles. This gap limits the development of robust segmentation models for safety-critical maintenance environments.

No usable public dataset existed for this domain. Building one was a prerequisite for any meaningful applied research.

---

## Dataset Characteristics

| Characteristic | Description |
|---|---|
| **Total Images** | 35 high-resolution marine biofouling images |
| **Annotated Instances** | 92 biofouling regions across all 35 images |
| **Collection Site** | Dockyard & Engineering Works Ltd., Narayanganj, Bangladesh |
| **Annotation Tool** | [CVAT (Computer Vision Annotation Tool)](https://cvat.ai) manual polygon annotations, cross-checked for quality |
| **Annotation Format** | YOLO instance segmentation masks |
| **License** | CC BY-NC 4.0 |

**Key variabilities captured:**
- **Angles:** Varied viewing angles relative to hull surfaces
- **Lighting:** Diverse illumination conditions: different times of day, shadows, highlights
- **Background:** Heterogeneous backgrounds (e,g, water, dock structures, equipment)
- **Instance size:** Biofouling regions ranging from small patches to extensive coverage
- **Human presence:** Human presence in approximately half the images, simulating real dockyard operations and occlusions
- **Coverage scale:** 4 full-coverage images (>80% frame), 20 medium-scale (30–80%), 11 contextual (<30%)

---

## Representative Dataset Samples
*Representative scenes from the NDBD dataset (from associated preprint, Fig. 2)*
The dataset captures six representative scene types:

- **(a)** Contextual daylight view without human presence
- **(b)** Submerged ship-hull biofouling
- **(c)** Heavy (>80%) fouling coverage from varying angles and illumination
- **(d)** Under-hull inspection in low-light conditions with human presence
- **(e)** Side-view cleaning operation with human presence
- **(f)** Dockyard environment with visible infrastructure and human presence

> *Sample annotated images from the paper will be added here.*  
> *(Upload your Figure 2 image to the `images/` folder as `ndbd-sample-annotations.jpg` and replace this line with:*  
> `![NDBD sample annotations](../images/ndbd-sample-annotations.jpg)`*)*

---

## Ethics and Data Privacy

This dataset was collected following the [Datasheets for Datasets (Gebru et al., 2021)](https://dl.acm.org/doi/10.1145/3458723) framework, with explicit 
attention to purpose transparency, privacy, consent, and diversity. Key compliance steps:

- **Institutional consent** obtained from Dockyard & Engineering Works Ltd. in compliance with local regulations and GDPR principles
- **No personally identifiable information (PII):** Faces, vessel numbers, and other identifiers were removed during preprocessing
- **IMO ethical principles** regarding marine worker safety were observed throughout collection
- **Bias audit** conducted to ensure balanced representation across hull coverage types
- **Anonymized dataset** released under CC BY-NC 4.0, prohibiting re-identification

This compliance documentation is included in the associated manuscript and follows standards expected by international venues for dataset publication.

---

## What This Enabled

This dataset directly enabled two subsequent research projects:

1. **Undergraduate Thesis:** Biofouling and corrosion detection prototype using YOLOv8 segmentation variants, achieving fouling detection F1 up to 0.90
2. **RA Research:** k–n Fold Augmentation Cross-Validation framework and Interaction-Effect metric for data-scarce industrial vision, validated across
   YOLOv8m-seg, YOLO11m-seg, and Mask R-CNN

The dataset is publicly available and intended to serve as a benchmark resource for future research in marine inspection, hull maintenance automation, and 
industrial computer vision under data-scarce conditions.

[Dataset (GitHub — NDBD)](https://github.com/saadalifazaman/NDBD) | [Associated Preprint (SSRN)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5966947)

*Note: The SSRN preprint reflects an earlier version of the manuscript. The submitted version contains updated methodology and results.*
