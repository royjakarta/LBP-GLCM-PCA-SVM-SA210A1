# LBP–GLCM–PCA–SVM Model for Microstructural Degradation Stage Classification of SA210 Grade A1 Steel

Machine learning model for microstructural degradation stage classification in SA210 Grade A1 steel.

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

### Hyperparameter Search

The notebook uses `GridSearchCV` with stratified cross-validation to optimize the model. The main grid includes:

- `pca__n_components`: full feature size, 80, 120
- `svc__C`: 0.1, 1, 10, 50, 100
- `svc__gamma`: `scale`, 0.001, 0.01, 0.1

This search strategy was used to determine the most suitable dimensionality reduction level and SVM hyperparameters for the classification task.

---

## Expected Dataset Organization

The notebook expects a folder-based dataset structure in which each degradation stage is stored in a separate subfolder.

### Training Dataset

```text
SA210A1/
├── StageA/
├── StageB/
├── StageC/
├── StageD/
├── StageE/
└── StageF/
```

### Independent Validation Dataset

```text
Validasi_SA210A1/
├── StageA/
├── StageB/
├── StageC/
├── StageD/
├── StageE/
└── StageF/
```

Each subfolder should contain the corresponding micrograph images in supported formats such as:

- `.png`
- `.jpg`
- `.jpeg`

---

## Installation

### Option 1 — Using `requirements.txt`

Create and activate your Python environment, then install the required packages:

```bash
pip install -r requirements.txt
```

### Option 2 — Using Conda

If an `environment.yml` file is provided, create the environment with:

```bash
conda env create -f environment.yml
conda activate lbp-glcm-pca-svm
```

---

## Main Dependencies

The workflow relies primarily on the following Python packages:

- `numpy`
- `pandas`
- `matplotlib`
- `scikit-learn`
- `scikit-image`
- `opencv-python`
- `seaborn`
- `joblib`
- `jupyter`

---

## How to Run

1. Clone or download this repository.
2. Prepare the dataset folders using the expected class-directory structure.
3. Install the required dependencies.
4. Open the notebook:
   - `LBP_GLCM_PCA_SVM_model.ipynb`
5. Run the cells sequentially.
6. Review:
   - class distribution,
   - train/test split,
   - grid search results,
   - confusion matrices,
   - saved model artifacts,
   - independent validation performance.

---

## Main Files

Typical important files in this repository include:

- `LBP_GLCM_PCA_SVM_model.ipynb`  
  Main notebook containing preprocessing, feature extraction, training, tuning, evaluation, and validation.

- `requirements.txt`  
  Python package requirements for pip-based installation.

- `environment.yml`  
  Conda environment specification for reproducible setup.

- `GridSearch_Heatmap_*.png`  
  Visualization of cross-validation performance across selected hyperparameter combinations.

- `*.joblib` or `*.pkl`  
  Saved trained model and, when applicable, label encoder or auxiliary objects.

---

## Outputs

Depending on the executed cells and configured paths, the notebook can generate:

- feature arrays,
- trained PCA–SVM model,
- cross-validation statistics,
- classification report,
- test confusion matrix,
- independent validation confusion matrix,
- hyperparameter tuning heatmap,
- saved model files for later inference.

---

## Reproducibility Notes

This repository is intended to improve reproducibility, but exact numerical results may still depend on several factors, including:

- dataset version and image count,
- random train–test partitioning,
- preprocessing consistency,
- package versions,
- operating system and execution environment.

For this reason, it is recommended to keep:

- fixed random seeds,
- clearly documented dataset structure,
- and explicit dependency files (`requirements.txt` or `environment.yml`).

---

## Notes on Data Availability

The full image dataset may not be publicly distributed in this repository, depending on data ownership, manuscript policy, or file-size limitations. If the dataset is not included, this repository should be interpreted as the computational workflow accompanying the study rather than a complete public benchmark package.

If needed, a short statement may be added here describing the conditions under which the image dataset can be shared.

**Example statement:**

> The image dataset used in this study is not publicly included in this repository. Selected data or derived features may be made available upon reasonable request, subject to research and institutional considerations.

---

## Citation

If you use this workflow, please cite the associated manuscript when available.

**Suggested format:**

```text
On progress:
Rio Pudjidarma Santoso, Winarto Winarto, Jaka Fajar Fatriansyah. Physically Based PCA–SVM Framework for Microstructural Degradation Classification in SA210 Grade A1 Steel. Journal. Year. DOI
```

---

## Suggested Manuscript Linkage Statement

This repository accompanies the manuscript on microstructural degradation stage classification of SA210 Grade A1 ferritic–pearlitic steel using a texture-based machine-learning framework. It is intended to provide the core implementation details required for computational transparency and methodological reproducibility.

---

## Author

**Rio Pudjidarma Santoso**  
Department of Metallurgical and Materials Engineering  
Faculty of Engineering, Universitas Indonesia

---

## License

No license until the repository is finalized.

---

## Acknowledgement

This repository was prepared to support the transparent reporting of a machine-learning-assisted metallographic classification workflow for degraded boiler steel microstructures.
