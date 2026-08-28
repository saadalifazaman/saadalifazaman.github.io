---
title: "Interaction-Effect Metric and k-n Fold ACV for Data-Scarce Industrial Vision"
excerpt: "A novel framework for principled augmentation pipeline design under extreme data scarcity (n < 50), validated on ship hull biofouling detection — achieving up to +47.6% mAP improvement over baseline through augmentation ordering alone.<br/><img src='/images/ie-metric-fig9-qualitative.png'>"
collection: portfolio
---

**Role:** Research Assistant (August 2025 – August 2026)  
**Supervisors:**  
Dr. Kazi Naimul Hoque, Associate Professor, Dept. of Naval Architecture and Marine Engineering, BUET  
Samiul Based Shuvo, Assistant Professor, Dept. of Biomedical Engineering, BUET  
**Status:** Manuscript under review  
**Code and Dataset:** [GitHub — NDBD](https://github.com/saadalifazaman/NDBD)

---

## The Problem

Marine biofouling costs the global shipping industry up to **USD 30 billion annually**[(Martinez, 2025)](https://www.worldports.org/the-global-biofouling-challenge-calls-for-new-technology/) in increased fuel consumption and maintenance. A single ship with 10% barnacle coverage requires 36% more shaft power [AB (2025)](https://selektope.com/wp-content/uploads/2025/04/How-much-could-barnacle-biofouling-limit-shippings-decarbonisation.pdf) and over 110 million tons of excess CO₂ [AB (2025)](https://selektope.com/wp-content/uploads/2025/04/How-much-could-barnacle-biofouling-limit-shippings-decarbonisation.pdf) is emitted annually from ships that have hard biofouling on their hulls. Automated hull inspection is therefore a critical component of intelligent maritime maintenance.

The core bottleneck is **data scarcity**. Real industrial inspection environments cannot produce thousands of labeled images. Because collecting and annotating hull imagery is expensive, logistically constrained, and access-restricted. Existing augmentation methods [AutoAugment](https://arxiv.org/abs/1805.09501) (Cubuk et al., 2019), [RandAugment](https://arxiv.org/abs/1909.13719) (Cubuk et al., 2020), [AugMix](https://arxiv.org/abs/1912.02781) (Hendrycks et al., 2019), and [TrivialAugment](https://arxiv.org/abs/2103.10158) (Müller & Hutter, 2021) optimize transformation selection but share a fundamental assumption: that the joint effect of two augmentations is adequately approximated by their independent contributions, and that applying augmentation A before B is equivalent to applying B before A. **This assumption has never been empirically tested in small-data instance segmentation regimes.**

Without a metric to quantify whether two augmentations together outperform or underperform their individual contributions, practitioners working with fewer than 50 labeled images have no principled basis for pipeline construction. Thus, they rely entirely on trial and error.

---

## What We Built

![Study workflow](/images/ie-metric-fig1-workflow.png)
*Complete experimental pipeline from dataset collection through k–n Fold ACV and IE analysis to cross-architecture validation.*

This study presents a data-scarce industrial vision framework built around three contributions:

### 1. Interaction-Effect (IE) Metric

A novel metric that explicitly quantifies **order-sensitive, pairwise augmentation interactions**:

$$IE_{\alpha \to \beta} = CE_{\alpha \to \beta} - \frac{SE_\alpha + SE_\beta}{2}$$

Where:
- **CE** = Combined Effect of the ordered pair augmentation (α then β)
- **SE** = Single Effect of each individual augmentation

A **positive IE** indicates the ordered pair produces performance exceeding the arithmetic mean of its individual effects (synergistic). A **negative IE** indicates the pair performs below the arithmetic mean of its individual effects, though it may still exceed the performance of either augmentation applied alone. This additive null hypothesis reflects the fact that both augmentations are applied to the same training image — their combined effect **replaces** rather than accumulates their individual contributions.

![IE heatmap](/images/ie-metric-fig6-heatmap.png)
*Interaction Effect heatmap across all 110 ordered augmentation pairs. Warmer colors (red) indicate synergistic pairs; cooler colors (blue) indicate antagonistic combinations.*

### 2. k–n Fold Augmentation Cross-Validation (ACV) Protocol

Standard k-fold cross-validation introduces a specific evaluation bias when used for augmentation studies. Augmented derivatives of a raw validation image may appear in the training set. Under extreme data scarcity (n < 50), this is not negligible. Because a single raw image and its augmented versions can constitute a substantial fraction of the entire dataset, introducing optimistic bias into performance estimation.

The **k–n Fold ACV protocol** eliminates this bias by enforcing strict separation between augmented training data and raw validation data:
- Raw images are divided into **k balanced folds** held in a validation folder
- A parallel training folder is constructed by applying augmentation to each fold independently
- For each of the **kCn (k choose n)** validation fold selections, the model trains on augmented images from remaining k-n folds and validates on raw images only
- Averaged across **3 random seeds (0, 43, 131)** for stochastic stability
- In this study: **k=5, n=1** → 5 training-validation iterations per configuration, 15 independent runs per augmentation configuration

### 3. Two-Stage Screening Framework

Exhaustively evaluating all 110 ordered pairs from 11 augmentations with full k-n fold ACV would require 1,650 training runs, computationally infeasible under the available GPU budget. The framework addresses this through a two-stage approach:

**Stage 1: IE-driven fixed-partition screening:** All 110 ordered pairs evaluated on a single representative partition to compute IE values. The IE heatmap reveals order-dependent dynamics across photometric, geometric, and noise & blur based augmentation categories.

**Stage 2: Full 5–1 Fold ACV on selected candidates:** The 5 most synergistic (IE ≥ 0.06) and 2 most antagonistic (IE ≤ −0.049) couples along with their reverse order (14 ordered pairs total) were advanced to full cross-validated evaluation. This **reduces computation by 87%** while preserving representative pairwise interaction trends.

---

## Dataset

Validated on the **Narayanganj Dockyard Biofouling Dataset (NDBD)**, a newly curated dataset of 35 high-resolution ship-hull images with 92 annotated biofouling instances. The dataset was collected from Dockyard & Engineering Works Ltd., Narayanganj, Bangladesh. This is the **first publicly available annotated dataset of dockyard hull surfaces under real operational conditions**. It included human occlusion, oblique angles, and variable illumination. See the [Dataset Collection portfolio entry](/portfolio/00-dataset-collection-narayanganj/) for full details.

---

## Models

Three instance-segmentation architectures were evaluated:

| Model | Role | Paradigm |
|---|---|---|
| **YOLOv8m-seg** | Primary model | Anchor-free, single-stage |
| **YOLO11m-seg** | Cross-family generalization | Anchor-free, single-stage |
| **Mask R-CNN** | Classical baseline | Anchor-based, two-stage |

Medium-scale variants were chosen to balance model capacity against overfitting risk at n=35. All experiments used a Tesla T4 GPU on Google Colab, with COCO-pretrained weights for transfer learning.

---

## Key Results

### Single Augmentations

All 11 augmentations produced statistically significant improvements (p < 0.05) over the baseline (Mask mAP50–95 = 0.351 ± 0.012), with large effect sizes (Cohen's d > 0.8):

| Top Augmentation | Mask mAP50–95 | Gain (MRC%) |
|---|---|---|
| Rotation (±15°) | 0.469 ± 0.011 | +33.94% |
| Noise (≤1.99%) | 0.455 ± 0.017 | +30.03% |
| Hue (±25°) | 0.454 ± 0.007 | +29.50% |
| Crop (0–30%) | 0.450 ± 0.016 | +28.32% |
| Baseline | 0.351 ± 0.012 | — |

Geometric augmentations (Rotation ±15°, Crop, Shear) consistently achieved the highest scores by forcing the model to learn features from partial views, critical for occluded fouling organisms. Notably, **Exposure (±15%) was the only augmentation achieving statistically significant recall improvement** (+8.91%), making it the preferred choice for safety-critical detection where missed fouling patches are costly.

### Pairwise Augmentation Interactions

**Category-level findings (from IE heatmap, 110 pairs):**

| Category Pair | Strong Synergy (IE ≥ 0.04) | Moderate Synergy (IE ≥ 0.02) | Antagonism (IE < 0) |
|---|---|---|---|
| Photometric → Photometric | 41.7% | 75% | 8.3% |
| Photometric → Geometric | 5% | 55% | 15% |
| Geometric → Geometric | 15% | 25% | 50% |
| Blur/Noise → Blur/Noise | 0% | 0% | 100% |

**Top validated ordered pairs (full 5–1 Fold ACV):**

| 1st Augmentation | 2nd Augmentation | Mask mAP50–95 | Gain (MRC%) |
|---|---|---|---|
| Saturation (±34%) | Rotation (±15°) | 0.517 ± 0.022 | **+47.60%** |
| Rotation (±15°) | Saturation (±34%) | 0.515 ± 0.018 | +46.80% |
| Crop (0–30%) | Rotation (±15°) | 0.509 ± 0.007 | +45.30% |
| Hue (±25°) | Rotation (±15°) | 0.503 ± 0.019 | +43.60% |
| Baseline | — | 0.351 ± 0.012 | — |

The **best ordered pair (Saturation → Rotation ±15°) outperforms the best single augmentation by +13.7%** absolute, achieved through sequencing decisions alone, no architectural modification, no additional inference-time cost.

![Performance comparison](/images/ie-metric-fig7-results.png)
*Mask mAP50–95 across baseline, top single augmentations, and top ordered pairs (mean ± SD, 3 seeds, YOLOv8m-seg).*

### Order Sensitivity

Augmentation order demonstrably matters. For example:
- **Crop → Rotation ±15°** yields Mask mAP50–95 = 0.509 ± 0.007 (+45.3%), whereas reversing to **Rotation → Crop** drops it to 0.499 ± 0.010 (+42.3%)
- **Saturation → Exposure** achieves 0.479 ± 0.003 (+36.9%), while the reverse **Exposure → Saturation** degrades to 0.471 ± 0.014 (+34.6%)
- Rotation-first sequences increase **precision** (stricter boundary consistency); photometric-first sequences improve **recall** (contextual completeness) — enabling intentional precision-recall trade-offs through pipeline design without changing the model

![Qualitative results](/images/ie-metric-fig9-qualitative.png)
*Instance segmentation on validation hull images under three conditions — no augmentation (baseline), best single augmentation (Rotation ±15°), and best ordered pair (Saturation → Rotation ±15°).*

### Cross-Architecture Validation

Order sensitivity and synergistic gains persist across all three architectures, confirming the framework is not model-specific:

| Pair | YOLOv8m-seg | YOLO11m-seg | Mask R-CNN |
|---|---|---|---|
| Saturation → Rotation | 0.517 ± 0.022 | 0.484 ± 0.013 | 0.386 ± 0.010 |
| Baseline | 0.351 ± 0.012 | 0.281 ± 0.041 | 0.318 ± 0.008 |

YOLO variants consistently outperform Mask R-CNN (max 0.517 vs. 0.386), though **all architectures show statistically significant gains (p < 0.05, Cohen's d > 3) from ordered augmentation** — confirming that strategic augmentation ordering can rival architectural complexity in efficiency-sensitive deployments.

### Hold-Out Test Set

A 41-image hold-out test set (collected from publicly available videos, case reports, and news portals — not used during training) was evaluated using the top configuration (Saturation → Rotation ±15°, YOLOv8m-seg with Mosaic, seed=0). **70.7% of samples had confidence scores above 0.81**, supporting a human-in-the-loop deployment strategy where high-confidence predictions are processed automatically and lower-confidence cases are routed for human review.

---

## Practical Implications

- **Resource efficiency:** The two-stage screening framework reduces the 110-pair evaluation space by 87% using IE-guided candidate selection, making systematic augmentation evaluation feasible without large GPU budgets.
- **Deployment model:** The augmented model is positioned as a **human-in-the-loop pre-screening tool** — identifying likely fouling regions automatically while routing ambiguous cases for inspector review. This is the realistic deployment posture for Mask mAP50–95 = 0.517, which does not yet establish human-level boundary accuracy.
- **Sustainability:** Timely fouling detection may reduce fuel consumption by an estimated 5–15% [(Bakka et al., 2023)](https://doi.org/10.1080/09377255.2023.2166849), supporting the IMO's 2050 decarbonization [(IMO, 2023)](https://wwwcdn.imo.org/localresources/en/KnowledgeCentre/IndexofIMOResolutions/MEPCDocuments/MEPC.378(80).pdf) objectives.
- **Transferability:** The IE metric and k–n Fold ACV protocol are domain-agnostic. The framework is applicable to any computer vision task involving extreme data scarcity (n < 50) with high intra-class variability — precision agriculture, structural inspection, environmental monitoring — though IE thresholds require domain-specific recalibration.

---

## Limitations

- Dataset comprises 35 images from a single Bangladeshi dockyard — generalizability across diverse fouling types, vessel materials, and geographic regions (e.g., tropical vs. temperate) requires validation.
- IE thresholds were derived empirically from this dataset and are not universal constants — practitioners applying the IE metric to new domains should recalibrate from their own screening distribution.
- Scope is limited to single-class instance segmentation; multi-class tasks may require framework extension.
- Real-time inference optimization, FLOPs, and edge-device deployment were not assessed.
- Higher-order augmentation chains (3+ sequential transforms) remain unexplored.

---

## Future Directions

- Evaluation on biofouling datasets from 3–5 geographically distinct dockyards (tropical and temperate)
- Systematic variation of training-set size (n = 35 to n = 200) to determine where standard augmentation-search methods become preferable to IE-guided screening
- Edge deployment on NVIDIA Jetson Orin Nano with target < 100 ms inference per frame
- Extension to three-stage augmentation chains beyond pairwise combinations
- Benchmarking against fine-tuned SAM, DETR, and vision-transformer models under comparable data-scarcity conditions

---

[Code and Dataset (GitHub — NDBD)](https://github.com/saadalifazaman/NDBD)

*Note: The SSRN preprint reflects an earlier version of this manuscript. The submitted version contains updated methodology, results, and analysis.*
