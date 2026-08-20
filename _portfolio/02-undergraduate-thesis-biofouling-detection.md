---
title: "Biofouling & Corrosion Identification System Using Deep Learning"
excerpt: "Built a real-time hull inspection prototype using YOLOv8 segmentation variants on a self-collected dataset from Narayanganj Dockyard, achieving fouling detection F1 up to 0.90."
collection: portfolio
---

**Role:** Undergraduate Thesis (June 2023 – July 2024)  
**Supervisor:** Dr. Kazi Naimul Hoque, Associate Professor, Dept. of Naval Architecture and Marine Engineering, BUET

---

**The problem:** Manual ship hull inspection for biofouling and corrosion is labor-intensive, inconsistent, and environmentally costly — fouling buildup increases fuel consumption and emissions directly. Automated vision-based inspection offers a scalable alternative, but requires domain-specific datasets that do not yet exist publicly.

**What I built:** Collected and curated a biofouling and corrosion dataset from scratch at Narayanganj Dockyard, Bangladesh. Trained YOLOv8 segmentation variants using transfer learning to detect and localize biofouling and steel-corrosion defects from video and image input. Implemented the full pipeline: field data collection → annotation using CVAT in YOLO format → cloud training on Colab and Kaggle → real-time detection prototype for automated hull inspection.

**Result:** Fouling detection F1 score up to 0.90. The prototype reduces reliance on manual inspection labor and enables condition-based maintenance scheduling, directly reducing fuel consumption and emissions through timely fouling management.
