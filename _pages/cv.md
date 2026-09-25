---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

[**Download CV (PDF)**](/files/Saad_Alif_Zaman_CV.pdf)

Education
======
* B.Sc. in Naval Architecture and Marine Engineering, Bangladesh University of Engineering and Technology (BUET), 2024
  * Cumulative GPA: 3.37/4.00
<h3>Relevant Coursework</h3>
<p>Selected coursework directly relevant to my research, verified against 
<a href="https://name.buet.ac.bd/undergraduate-courses" target="_blank">BUET's official undergraduate course catalog</a>:</p> 
Marine Maintenance and Repair, Welding Technology, Finite Element Methods, Numerical Computations, Computer Programming (C/C++, FORTRAN), Applied Statistics
<details>
  <summary style="cursor:pointer; color:#0645AD;">(click for course descriptions)</summary>
  <p><strong>Marine Maintenance and Repair (NAME 415):</strong> Maintenance requirements: 
  corrosion, fatigue, marine fouling. Prevention and removal of marine growth; classification 
  requirements of hull survey and defect identification; welding inspection.</p>
  <p><strong>Welding Technology (NAME 345):</strong> Common defects in ship welding, 
  non-destructive testing, inspection and testing of welded specimens.</p>
  <p><strong>Finite Element Methods (NAME 371):</strong> Application of FEM to ship structure; 
  isoparametric elements; linear static analysis.</p>
  <p><strong>Numerical Computations (NAME 416):</strong> Interpolation, numerical 
  differentiation/integration, regression analysis and curve fitting.</p>
  <p><strong>Computer Programming (NAME 336, 436):</strong> FORTRAN 77/90 and C/C++, applied to 
  hydrostatic, stability, and structural strength computations.</p>
  <p><strong>Applied Statistics (Math 283):</strong> Probability, distributions, estimation, 
  hypothesis testing, regression analysis.</p>
</details>

Research Experience
======
* **Research Assistant**, Dept. of Naval Architecture and Marine Engineering, BUET (August 2025–Present)
  * "Interaction-Effect Metric for Data-Scarce Industrial Vision: Application to Ship Hull Biofouling Inspection"
  * Supervisors: Dr. Kazi Naimul Hoque, Associate Professor, Dept. of Naval Architecture and Marine Engineering, BUET  
	           Samiul Based Shuvo, Assistant Professor, Dept. of Biomedical Engineering, BUET
  * Addressing a core bottleneck in applied computer vision, the lack of large labeled datasets in real world industrial settings, we proposed a k–n Fold Augmentation Cross Validation (k–n Fold ACV) protocol, together with a novel Interaction Effect (IE) Metric that quantifies order sensitivity among data augmentations. The framework was validated on a newly curated [NDBD](https://github.com/saadalifazaman/NDBD) dataset of 35 high resolution ship hull images containing diverse biofouling patterns with 92 annotated instances. Eleven domain specific augmentations and 110 pairwise combinations were analyzed using YOLOv8m-seg as the base model, with cross validation across YOLO11m-seg, and Mask R-CNN. The [code](https://github.com/saadalifazaman/NDBD/blob/main/5-1%20Fold%20ACV%20for%20YOLOv8m-seg.ipynb) and dataset are publicly available.
  
* **Undergraduate Thesis**, BUET Final Grade: A+ (4.00/4.00) (June 2023–July 2024)
  * "Developing a biofouling & corrosion identification and distribution system using video images based on deep learning"
  * Supervisor: Dr. Kazi Naimul Hoque, Associate Professor, Dept. of Naval Architecture and Marine Engineering, BUET
  * Collected and curated the Narayanganj Dockyard Biofouling Dataset ([NDBD](https://github.com/saadalifazaman/NDBD)) form Dockyard & Engineering Works LTD, Narayanganj; 35 high resolution hull images, 92 annotated instances, CC BY-NC 4.0 license, publicly available on GitHub. Then trained YOLOv8 segmentation variants (fouling F1 up to 0.90), producing a real-time detection prototype for automated hull inspection. Funded by [RISE](https://rise.buet.ac.bd/#/) Student Research Grant.

* **Junior Year Project**, BUET Final Grade: A+ (4.00/4.00) (May 2022–May 2023)
  * "Designing an Inland 100 Passenger Ship for Inland Waterways of Bangladesh"
  * Supervisor: Dr. Md. Mashiur Rahaman
  * Designed a conceptual 100-passenger inland vessel with electric propulsion to reduce CO₂ emissions and improve energy efficiency compared to conventional diesel systems.

Publications
======
  <ul>{% for post in site.publications %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

### Standardized Test Scores

**International English Language Testing System (IELTS) Academic** (15th December, 2025)  
Overall Band Score: 7.5 (Listening: 8.5, Reading: 8.5, Writing: 6.5, Speaking: 7.0)

Work Experience & Teaching
======
* **Materials Development Instructor**, Udvash Academic & Admission Care, Dhaka (April–August 2025)
  * Developed and reviewed assessment materials for SSC, HSC, and university admission examinations; evaluated question quality and curriculum alignment; contributed to model-test preparation.

* **Academic Mentor — Mathematics & Physics** (2019–Present)
  * Provided one-to-one and small-batch instruction to secondary and higher-secondary students (Bangla- and English-medium curricula), from foundational concepts through advanced problem-solving. Multiple students subsequently admitted to BUET, KUET, DU, and IUT.

* **Industrial Internship — Shipyard Practice**, Khulna Shipyard Limited, Khulna (October–November 2022)

Skills
======
* **Programming:** Python, C, C++
* **Deep Learning:** Ultralytics YOLO, Mask R-CNN, PyTorch, TensorFlow
* **Data Annotation & Processing:** CVAT, Roboflow, LabelImg
* **Research Computing:** Google Colab, Kaggle
* **CAD / FEA:** AutoCAD, Rhino, Abaqus
* **Document Preparation:** LaTeX, Prism, Microsoft Office

Grants & Scholarships
======
* **RISE Student Research Grant, 2023:** Awarded a competitive Student Research Grant (BDT 65,790) by the Research and Innovation center for Science and Engineering ([RISE](https://rise.buet.ac.bd/#/)) for "Developing a biofouling & corrosion identification and distribution system using video images based on deep learning."

<figure style="max-width: 420px; margin: 0.5em 0 1.5em 0;">
  <img src="/images/certificates/rise-grant-certificate.jpg" alt="RISE Student Research Grant Certificate, 2023" style="width: 100%; border: 1px solid #ddd; border-radius: 4px;">
  <figcaption style="font-size: 0.85em; color: #666; margin-top: 0.4em;">RISE Student Research Grant certificate, 2023</figcaption>
</figure>

* **SPARRSO Research Fellowship Program, 2025–26:** Shortlisted (final round) among national applicants for a proposal stage research fellowship with the Space Research and Remote Sensing Organization ([SPARRSO](https://sparrso.gov.bd/)), Bangladesh; invited to present the written proposal "Development of a Fixed-Wing VTOL Quad Plane for Flood Monitoring & Disaster Response" (design not yet implemented).
* **University Stipend Scholarship**, BUET (2019, 2020, 2021)
* **Rajshahi Board Scholarship**, HSC 2018
