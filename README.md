# MRI–CT Training Ratio Analysis for Cross-Dataset Brain Tumor Classification

## 1. Overview

In this project, I investigated how different MRI:CT training ratios affect brain tumor classification and cross-dataset generalization using **EfficientNetB0**.

I trained seven models using different modality ratios on Dataset-1 and evaluated their performance separately on MRI and CT images from Dataset-2 to investigate possible modality distribution bias.

## 2. Objectives

- Compare seven MRI:CT training ratios.
- Keep the training sample count fixed at 2,000 images per experiment.
- Evaluate MRI and CT performance separately on Dataset-2.
- Investigate modality-dependent performance differences.

## 3. Datasets

Both datasets contain MRI and CT images classified into `healthy` and `tumor` categories.

| Dataset | Purpose |
|---|---|
| Dataset-1 | Training and source-dataset validation |
| Dataset-2 | Independent cross-dataset evaluation |

**Google Drive links:**
- [Dataset-1](https://drive.google.com/file/d/1JuWrSAKTsX8tvPrpJUOQvpYUXUiM9E1K/view?usp=drive_link)
- [Dataset-2](https://drive.google.com/file/d/1pBWU65hgU-seXJupjdpDAdkWHZU9XJCp/view?usp=drive_link)
- [Trained `.keras` Models](https://drive.google.com/drive/folders/1l73zESfVstyneyjTnCHbVB0Ck4oNmA0W?usp=sharing)

## 4. Model and Configuration

I used ImageNet-pretrained EfficientNetB0 with a frozen backbone and a binary classification head.

| Parameter | Configuration |
|---|---|
| Input size | 224 × 224 RGB |
| Classes | Healthy, Tumor |
| Batch size | 32 |
| Maximum epochs | 10 |
| Learning-rate scheduler | ReduceLROnPlateau |
| Early stopping | Enabled |

## 5. Training Pipeline

I followed the same training workflow for all seven MRI:CT ratios.

```text
Dataset-1
    │
    ▼
Separate MRI and CT Images
    │
    ▼
Select Images According to MRI:CT Ratio
    │
    ▼
Balance Healthy and Tumor Classes
    │
    ▼
Preprocessing (224 × 224 RGB)
    │
    ▼
EfficientNetB0 (ImageNet Pretrained)
    │
    ▼
Frozen Backbone + Binary Classification Head
    │
    ▼
Train Model for Each Ratio
    │
    ▼
Save Trained Model (.keras)
    │
    ▼
Evaluate on Dataset-2
    │
    ├── MRI Evaluation
    │
    └── CT Evaluation
          │
          ▼
Compare Accuracy and Classification Metrics
```

### Training Procedure

1. I selected 2,000 images from Dataset-1 for each MRI:CT ratio.
2. I maintained balanced healthy and tumor classes within the selected training samples.
3. I resized the images to 224 × 224 pixels and prepared them as RGB inputs.
4. I used ImageNet-pretrained EfficientNetB0 with a frozen backbone and a binary classification head.
5. I trained a separate model for each of the seven ratios using the same configuration, with early stopping and learning-rate scheduling.
6. I saved the trained models in `.keras` format.
7. I evaluated each model separately on MRI and CT images from Dataset-2 and compared the results.

**Experiment control:** The total training sample count and model configuration were kept consistent, while the MRI:CT ratio was varied.
---

## 6. Training Experiments

Each experiment used 2,000 training images, with healthy and tumor classes balanced within the selected samples.

| MRI:CT Ratio | MRI Images | CT Images |
|---|---:|---:|
| 50:50 | 1,000 | 1,000 |
| 60:40 | 1,200 | 800 |
| 40:60 | 800 | 1,200 |
| 30:70 | 600 | 1,400 |
| 70:30 | 1,400 | 600 |
| 80:20 | 1,600 | 400 |
| 20:80 | 400 | 1,600 |

## 7. Results

I evaluated each trained model on MRI and CT data from Dataset-2.

| MRI:CT Ratio | MRI Accuracy | CT Accuracy |
|---|---:|---:|
| 20:80 | 91.72% | 93.59% |
| 30:70 | 92.02% | 92.25% |
| 40:60 | 92.28% | 92.75% |
| 50:50 | 93.54% | 92.01% |
| 60:40 | 93.98% | 90.73% |
| 70:30 | 94.50% | 90.04% |
| 80:20 | 94.42% | 89.00% |

**Key observations:**
- Highest MRI accuracy: **94.50%** at 70:30.
- Highest CT accuracy: **93.59%** at 20:80.
- The MRI–CT accuracy gap reached **3.93 percentage points** at 80:20.
- MRI accuracy remained comparatively stable, while CT accuracy decreased in MRI-dominant experiments.

These results suggest possible modality-dependent performance differences. Repeated experiments are needed to assess reproducibility and determine the causes.

## 8. Repository Structure

```text
mri-ct-training-ratio-analysis/
├── MRI_CT_Training_Ratio_Analysis.ipynb
└── README.md
```

The datasets and trained `.keras` models are stored in Google Drive because of their large file sizes.

## 9. Technologies Used

Python, TensorFlow, Keras, EfficientNetB0, NumPy, Pandas, Matplotlib, Scikit-learn, Google Colab, and GitHub.

## 10. How to Run

1. Clone this repository.
2. Open `MRI_CT_Training_Ratio_Analysis.ipynb` in Google Colab.
3. Download or access both datasets through the Google Drive links.
4. Mount Google Drive and update the dataset paths.
5. Run the notebook cells in order to train and evaluate the models.
6. Alternatively, load the saved `.keras` models to avoid retraining.

## 11. Conclusion

I compared seven MRI:CT training ratios using EfficientNetB0 and independent cross-dataset evaluation. The results suggest that training-data composition may affect MRI and CT classification performance differently, providing an initial investigation into modality distribution bias.

## Disclaimer

This project is for academic research only and is not intended for clinical diagnosis or as a replacement for professional medical assessment.
