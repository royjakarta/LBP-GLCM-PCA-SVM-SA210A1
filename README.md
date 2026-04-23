# LBP-GLCM-PCA-SVM-SA210A1
Machine learning model for microstructural degradation stage classification in SA210 Grade A1 steel
# LBP–GLCM–PCA–SVM Model for Microstructural Degradation Stage Classification of SA210 Grade A1 Steel

This repository contains the machine learning workflow developed for the classification of microstructural degradation stages in SA210 Grade A1 ferritic–pearlitic steel. The model was designed to support stage-based interpretation of degraded boiler tube microstructures using optical metallographic images.

The workflow combines standardized image preprocessing, texture-based feature extraction, dimensionality reduction, and supervised classification. In particular, the model uses contrast enhancement and scale-bar removal during preprocessing, followed by multi-scale local binary pattern (LBP) and gray-level co-occurrence matrix (GLCM) feature extraction, principal component analysis (PCA), and support vector machine (SVM) classification.

This repository is provided as a computational companion to the associated manuscript and is intended to improve methodological transparency and reproducibility.

---

## Repository Overview

The main objectives of this repository are:

- to document the full image-based classification workflow used in the study,
- to provide the Jupyter Notebook implementation of the model,
- to support reproducibility of the training, tuning, and evaluation procedures,
- and to enable future extension of the framework for related metallographic classification tasks.

---

## Main Workflow

The classification pipeline consists of the following stages:

1. **Image preprocessing**
   - grayscale conversion,
   - CLAHE-based local contrast enhancement,
   - fixed bottom cropping to remove the embedded scale bar region,
   - resizing to a standardized image size.

2. **Feature extraction**
   - multi-scale LBP descriptors,
   - manually computed GLCM descriptors extracted from quantized grayscale images.

3. **Feature preparation**
   - concatenation of LBP and GLCM features into a single feature vector,
   - label encoding for stage classes,
   - stratified train–test splitting.

4. **Model development**
   - standardization using `StandardScaler`,
   - dimensionality reduction using `PCA`,
   - classification using `SVC` with RBF kernel,
   - hyperparameter optimization using `GridSearchCV`.

5. **Evaluation**
   - cross-validation performance,
   - test-set classification report,
   - confusion matrix visualization,
   - independent validation on a separate dataset.

---

## Implemented Feature Extraction Scheme

### 1. Local Binary Pattern (LBP)
The model uses multi-scale uniform LBP with the following radii:

- `R = 1`
- `R = 2`
- `R = 3`

For each radius, the number of sampling points is defined as:

- `P = 8 × R`

The LBP histograms from all radii are normalized and concatenated.

### 2. Gray-Level Co-occurrence Matrix (GLCM)
GLCM features are extracted from quantized grayscale images using:

- **gray levels**: `64`
- **distances**: `[1, 2, 3]`
- **angles**: `[0°, 45°, 90°, 135°]`

The GLCM is implemented manually in the notebook, which makes the extraction procedure explicit and traceable.

### 3. Combined Feature Vector
The final feature vector is formed by concatenating:

- LBP features
- GLCM features

This combined representation is then passed to PCA and SVM classification.

---

## Model Configuration

The classification model is built using a scikit-learn pipeline:

- `StandardScaler`
- `PCA`
- `SVC(kernel='rbf', class_weight='balanced', probability=True)`

### Hyperparameter search
The notebook uses `GridSearchCV` with stratified cross-validation to optimize the model. The main grid includes:

- `pca__n_components`: full feature size, 80, 120  
- `svc__C`: 0.1, 1, 10, 50, 100  
- `svc__gamma`: `scale`, 0.001, 0.01, 0.1  

This search strategy was used to determine the most suitable dimensionality reduction level and SVM hyperparameters for the classification task.

---

## Expected Dataset Organization

The notebook expects a folder-based dataset structure in which each degradation stage is stored in a separate subfolder.

## Training dataset
```text
SA210A1/
├── StageA/
├── StageB/
├── StageC/
├── StageD/
├── StageE/
└── StageF/

---

## Independent validation dataset
## Training dataset
```text
Validasi_SA210A1/
├── StageA/
├── StageB/
├── StageC/
├── StageD/
├── StageE/
└── StageF/
