# Brainiac

### Counterfactual Guided Stability-Aware MRI Brain Tumor Analysis Framework
Most medical imaging models answer a single question:

**What is the prediction?**

Brainiac extends this by addressing:

**How stable is that prediction under minimal, anatomically valid changes?**

This distinction is critical in high-stakes settings, where visually similar inputs may lead to significantly different model behaviors.


## Overview

Brainiac is an end-to-end framework for brain tumor analysis from MRI, designed to integrate **transformer-based segmentation, feature representation, and stability-aware risk modeling** within a unified pipeline.

The system leverages **Swin-UNETR for 3D volumetric segmentation**, enabling hierarchical representation learning with long-range spatial context. In parallel, it supports **2D slice-based workflows**, providing flexibility across computational environments while maintaining a consistent downstream modeling strategy.

A central design objective of Brainiac is to incorporate **decision stability as a quantifiable signal**, extending beyond conventional accuracy-driven approaches and post-hoc interpretability methods.

---

## Methodological Pipeline

The framework follows a structured processing pipeline:

```id="k6e4fd"
MRI Acquisition → Preprocessing → Segmentation → Feature Extraction → 
Counterfactual Analysis → Stability-Aware Risk Stratification
```

---

## Core Components

### 1. Preprocessing

* Multi-modal MRI inputs (T1, T2, FLAIR)
* Spatial normalization and resampling
* Z-score intensity normalization
* Skull stripping and artifact reduction

---

### 2. Segmentation Module

**3D Pipeline**

* Swin-UNETR-based transformer architecture
* Volumetric tumor segmentation with long-range context modeling

**2D Pipeline**

* Slice-wise modeling for 2D MRI inputs
* Enables reduced computational overhead and faster iteration

---

### 3. Feature Extraction

Feature representation is derived from segmented tumor regions:

* **Radiomic Features**
  Morphological, intensity, and texture-based descriptors

* **Attention Entropy**
  Extracted from transformer attention maps to capture contextual uncertainty

---

### 4. Counterfactual Analysis

Tumor-constrained counterfactual samples are generated to evaluate prediction behavior under minimal perturbations.

The **Counterfactual Sensitivity Score (CSS)** is defined as:

* The minimum perturbation required to alter a model prediction
* A direct estimate of proximity to the decision boundary

---

### 5. Stability-Aware Risk Stratification

A unified feature vector is constructed:

```id="x2k5z9"
Z = [Radiomics, Attention Entropy, CSS]
```

This representation is used in a downstream classifier (e.g., XGBoost / LightGBM) to categorize cases into:

* Low risk
* Medium risk
* High risk

based on decision stability characteristics.

---

## Repository Structure

```id="6x6c0w"
brainiac/
│── code/                 # Notebooks for preprocessing, segmentation, and analysis
│── models/               # Model checkpoints
│── data/                 # Dataset directory (excluded from version control)
│── risk_features.csv     # Extracted feature set
│── .gitignore
│── README.md
```

---

## Dataset

The framework is evaluated on the BraTS (Brain Tumor Segmentation) dataset:

https://www.med.upenn.edu/cbica/brats2020/data.html

---

## Installation

```bash id="6mlb7c"
git clone https://github.com/buildwithusorg-ai/Brainiac.git
cd Brainiac
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

---

## Dependencies

* Python ≥ 3.10
* PyTorch
* MONAI
* NumPy
* Pandas
* Matplotlib
* scikit-learn
* nibabel
* tqdm
* Jupyter Notebook / JupyterLab

---

## Implementation Notes

* Input volumes are standardized to fixed spatial dimensions
* Counterfactual perturbations are constrained to tumor regions
* Feature modeling integrates morphological, contextual, and stability signals
* GPU acceleration is recommended for segmentation and counterfactual modules

---

## Scope and Limitations

* Performance is dependent on segmentation quality
* Counterfactual generation introduces computational overhead
* Evaluation is limited to publicly available datasets



## Authors

- K. Usha Kiran Mayi  
- Khushi T. Mahesh  
- Induja B  
- Vanshika Jain  
- Shoaib Shaik  

**Affiliation:** Jain Deemed-to-be University
