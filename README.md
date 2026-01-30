# periapical-severity-classification

This project builds and compares multiple models to classify **periapical lesion severity** from panoramic dental X-rays, including:
- **MobileNetV2 (transfer learning)** for **3-class severity** classification
- **Simple CNN baseline** for **3-class severity** classification
- **XGBoost** using **XML-derived lesion morphology features** for **binary** classification (**Severe vs Non-Severe**)

---

## Dataset

The dataset is provided via Google Drive (images + XML annotations):

- Dataset link: https://drive.google.com/drive/folders/1-x-OPLli1-eQvyrHPvZSxNwNw8H5EH99?usp=drive_link

### Expected folder structure (inside your Drive/project folder)
Your notebook assumes a structure similar to:
RAS_585_Final/
├─ Original JPG Images/
├─ Augmentation JPG Images/
└─ Image Annots/               # Pascal VOC-style XML files
If your folder names differ, update the `BASE_DIR` and directory variables in the notebook.

---

## What this repo contains

- `notebooks/` : Main Colab/Jupyter notebook used for training + evaluation
- `figures/` : Saved plots (loss/accuracy curves, confusion matrices, PCA plots, etc.)
- `results/` : Metrics tables (`metrics_comparison.csv`) and any exported outputs
- `reports/` : Final presentation (PDF/PPTX)

> **Note:** The raw dataset is not included in this repository. Please download it from the Drive link above.

---

## Methods

### 1) 3-Class Classification (Image-based)
Classes represent **severity levels** (3-class setup, e.g., mild / moderate / severe depending on your labeling).

Models:
- **MobileNetV2 (ImageNet pretrained)** with a custom classification head
- **Simple CNN baseline**
- Training uses **augmented images** and Keras `ImageDataGenerator`

Metrics reported:
- Validation accuracy
- Macro precision/recall/F1
- Confusion matrix + sample predictions

### 2) Binary Classification (XML morphology → XGBoost)
- Extracts lesion morphology features from XML bounding boxes (e.g., lesion count, total area, max area, mean width/height)
- Trains an **XGBoost** classifier to predict **Severe (1)** vs **Non-severe (0)**
- Includes confusion matrix, ROC curve, feature importance, PCA visualization of feature overlap

---

## Results (high-level summary)

- **MobileNetV2 (3-class)** achieves around **~0.52–0.53 validation accuracy**
- **Macro F1 is relatively low**, indicating difficulty on minority classes / class imbalance
- Confusion matrix shows **strong confusion between adjacent severity classes**
- **XGBoost on XML morphology features performs poorly for severe detection**
  - Often predicts **non-severe** for severe examples
  - ROC curve close to **AUC ≈ 0.50**, suggesting morphology features alone are not sufficient

---

## How to run (Google Colab recommended)

1. Open the notebook in Colab:
   - Upload the notebook to Colab, or open it directly if hosted.

2. Mount Google Drive and set the dataset path:
   - In the notebook, update:
     - `BASE_DIR = "/content/drive/MyDrive/<YOUR_FOLDER_NAME>"`

3. Run the notebook top-to-bottom.

---

## Local installation (optional)

If you want to run locally:

```bash
pip install -r requirements.txt
jupyter notebook
