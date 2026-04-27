# LBP–GLCM–PCA(50)–SVM Framework for Six-Stage Microstructural Degradation Classification of SA210 Grade A1 Boiler Steel

This repository contains the machine learning workflow developed for six-stage microstructural degradation classification in SA210 Grade A1 ferritic–pearlitic boiler steel. The framework was designed to support stage-based metallographic interpretation of degraded boiler tube microstructures using optical micrographs.

The workflow combines standardized image preprocessing, multi-scale Local Binary Pattern (LBP) features, Gray-Level Co-occurrence Matrix (GLCM) texture descriptors, Principal Component Analysis (PCA), and radial-basis-function Support Vector Machine (RBF-SVM) classification. The final model uses a compact LBP–GLCM–PCA(50)–SVM representation, where the original 126-dimensional fused texture feature vector is reduced to 50 principal components before SVM classification.

This repository is provided as a computational companion to the associated manuscript and is intended to improve methodological transparency, reproducibility, and future extension of the proposed metallographic classification framework.

---

## Repository Purpose

The main purposes of this repository are:

- to provide the complete Jupyter Notebook implementation of the proposed classification workflow;
- to document the image preprocessing, feature extraction, feature fusion, dimensionality reduction, training, tuning, testing, validation, and ablation procedures;
- to support reproducibility of the machine-learning results reported in the associated manuscript;
- to provide a reusable framework for related metallographic image-classification tasks;
- to support future integration of image-based degradation classification with hardness degradation and remaining-life assessment.

---

## Study Context

The classification task addressed in this repository is not conventional phase recognition. The six target classes represent ordered microstructural degradation stages in SA210 Grade A1 ferritic–pearlitic steel after controlled subcritical thermal exposure. The stages correspond to the progressive evolution from intact ferrite–pearlite morphology to lamellar fragmentation, carbide spheroidization, carbide coarsening, and advanced carbide redistribution.

The class labels are therefore metallurgically informed. They are derived from controlled thermal exposure histories and metallographic interpretation of the degradation sequence, rather than from visual clustering or image appearance alone.

---

## Main Notebook

The main notebook is:

```text
lbp_glcm_pca_svm_sa210a1_final.ipynb
