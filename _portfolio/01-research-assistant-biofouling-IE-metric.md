---
title: "Interaction-Effect Metric and k-n Fold ACV for Data-Scarce Industrial Vision"
excerpt: "Developed a novel augmentation cross-validation protocol and Interaction-Effect metric for small industrial datasets, validated on ship hull biofouling imagery."
collection: portfolio
---

**Role:** Research Assistant (August 2025 – Present)  
**Supervisors:** Dr. Kazi Naimul Hoque, Associate Professor, Dept. of Naval Architecture and Marine Engineering, BUET; Samiul Based Shuvo, Assistant Professor, Dept. of Biomedical Engineering, BUET

---

**The problem:** A core bottleneck in applied computer vision is the lack of large labeled datasets in real-world industrial settings. Existing augmentation strategies offer no principled way to assess how the *order* of augmentations affects model performance under data scarcity.

**What we built:** A k–n Fold Augmentation Cross-Validation (k–n Fold ACV) protocol together with a novel Interaction-Effect (IE) metric that quantifies order-sensitivity among data augmentations. The framework was validated on a newly curated dataset (NDBD) of 35 high-resolution ship-hull images containing diverse biofouling patterns. Eleven domain-specific augmentations and 110 pairwise combinations were analyzed using YOLOv8m-seg as the base model, with cross-validation on YOLO11m-seg and Mask R-CNN confirming generalizability.

**Outcome:** A reproducible, model-agnostic framework for augmentation selection under data scarcity, with publicly available code and dataset.

[Code and Dataset (GitHub)](https://github.com/saadalifazaman/NDBD) | [Preprint (SSRN)](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5966947)
