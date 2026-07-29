# Land Cover Segmentation Project

Báo cáo chi tiết được lưu tại thư mục `report/`.

📄 **[Đọc/Tải về Báo cáo Nghiên cứu đầy đủ (PDF)](.report\Land_Cover_Segmentation_Report.pdf)**

---

# LAND COVER SEGMENTATION OF VIETNAM: ANALYZING SATELLITE IMAGERY FOR DIVERSE LAND TYPES

**Course:** Artificial Intelligence  
**Code:** INS308002  
**Lecturer:** Dr. Hung-Manh Ha  
**Institution:** International School – Vietnam National University, Hanoi (VNU-IS)  
**Date:** January 2025  

---

## 📌 Abstract

This study focuses on land cover segmentation using satellite imagery to analyze the dynamic changes in land use and land cover in Vietnam over the past decades. The rapid urbanization and industrialization since the Doi Moi reforms in 1986 have led to significant land conversion, with ancient forests and vital carbon sinks being replaced by urban centers, industrial zones, and monoculture agricultural systems. These transformations have caused profound ecological impacts, including biodiversity loss, increased greenhouse gas emissions, and heightened vulnerability to climate change. 

By developing and comparing advanced deep learning models, specifically **U-Net++** and **DeepLabV3+**, this study aims to accurately classify diverse land cover types and monitor temporal changes. The selected model will be applied to segment satellite imagery of key regions in Vietnam at two distinct time points, facilitating the detection and analysis of urban expansion and land use transitions. This approach harnesses the power of satellite imagery and remote sensing data, offering valuable insights for sustainable urban planning, environmental conservation, and climate change adaptation in Vietnam.

---

## 📑 Table of Contents
- [I. Introduction](#i-introduction)
- [II. Literature Review](#ii-literature-review)
- [III. Methodology](#iii-methodology)
  - [1. Data Collection and Preprocessing](#1-data-collection-and-preprocessing)
  - [2. Model Selection](#2-model-selection)
- [IV. Result and Comparison](#iv-result-and-comparison)
  - [4.1 Results](#41-results)
    - [4.1.1 U-Net++](#411-unet)
    - [4.1.2 DeepLabV3+](#412-deeplabv3)
- [V. Discussion & Application](#v-discussion--application)
- [VI. References](#vi-references)

---

## I. INTRODUCTION

### 1. Background and Motivation
In our rapidly modernizing world, many areas of our Earth's land cover are being urbanized or turned into adjacent land use types like farms to support our growing population. These types of activities have a large negative impact on the precarious ecological and climate states of our planet. Specifically, many ancient forests and other carbon sinks are being turned into cities and other mono-culture land use types.

Developing a method to monitor and track these activities can be crucial in gathering evidence and enacting policies to safeguard our planet's most crucial natural land cover types. Satellite imagery and other remote sensing data are invaluable in the fight against climate change in areas such as fighting deforestation, land use carbon accounting, icecap monitoring, and many others.

### 2. Research Objectives
For this project, we develop and compare two deep learning models, **U-Net++** and **DeepLabV3+**, trained on satellite imagery to classify different land cover types within the images. This is a classic multiclass semantic segmentation problem on image data. 

Additionally, we apply the best-performing model to segment a specific region at two different time points to detect and analyze urban changes over time. Furthermore, we aim to gain hands-on experience in working with satellite imagery data, image processing techniques, and advanced deep learning models.

---

## II. LITERATURE REVIEW

Traditional and modern methods in land cover segmentation have seen significant advancements. Initially, traditional methods such as **Maximum Likelihood Classification (MLC)** and **K-means Clustering** were widely used. Although computationally efficient, these methods struggle with mixed pixels and fail to effectively capture spatial context.

In recent years, modern methods based on **Deep Learning (DL)** have been extensively applied:
- **Convolutional Neural Networks (CNNs)** like U-Net and SegNet have demonstrated high effectiveness in image segmentation.
- **Graph Convolutional Networks (GCNs)** extend CNN capabilities to work with graph data, capturing relationships in irregular spaces.
- **Attention Mechanisms** improve focus on important image regions.
- **Domain Adaptation** (Feature-level, Latent Space, and GAN-based) minimizes differences between source and target domain distributions.

In this project, we focus on developing and comparing **U-Net++** (with nested skip pathways capturing complex features) and **DeepLabV3+** (utilizing Atrous Spatial Pyramid Pooling - ASPP and encoder-decoder architecture).

---

## III. METHODOLOGY

```text
Data Collection ➔ Data Labelling ➔ Data Preprocessing ➔ Model Selection ➔ Model Training ➔ Model Testing ➔ Model Evaluation ➔ Model Application

```

### 1. Data Collection and Preprocessing

#### 1.1 Data Collection

1. **Kaggle Data (DeepGlobe 2018):** 631 RGB satellite images ($2448 \times 2448$). Resized to $320 \times 320$ to enhance processing efficiency.
2. **Self-Collected Data (Google Earth Pro):** Satellite images from various regions in Vietnam, manually labeled using Roboflow into 7 categories.

**Class Mask Labels:**

* `0`: Background (no color)
* `1`: Agriculture
* `2`: Barren
* `3`: Forest
* `4`: Rangeland
* `5`: Unknown
* `6`: Urban
* `7`: Water

#### 1.2 Data Preprocessing Pipeline

* **Directory Structure:** Organized into `train/`, `valid/`, and `test/` folders with original images and class masks.
* **Data Cleaning & Integrity:** Verification of pair synchronization (1 image : 1 mask) and removal of corrupted files.
* **Data Augmentation:**
* Tiling large Kaggle images to $320 \times 320$.
* Transformations: Horizontal/vertical flipping, rotations ($90^\circ, 180^\circ, 270^\circ$), brightness/contrast adjustment, Gaussian noise addition.


* **Dataset Split:** Total 5,431 images and masks.
* **Training Set:** ~70%
* **Validation Set:** ~20%
* **Test Set:** ~10%



---

### 2. Model Selection

#### 2.1 U-Net++

U-Net++ (Nested U-Net) bridges the semantic gap between encoder and decoder feature maps by introducing **nested skip pathways** and **deep supervision**. This design preserves low-level and high-level features for fine-grained segmentation boundaries.

#### 2.2 DeepLabV3+

DeepLabV3+ extends DeepLabV3 by adding an effective **decoder module** to refine segmentation results along object boundaries.

* **Key Modules:** Atrous Spatial Pyramid Pooling (ASPP), 1x1 Convolutions, Global Average Pooling, and deep backbone networks (such as ResNet or Xception).

---

## IV. RESULT AND COMPARISON

### 4.1 Results

#### 4.1.1 U-Net++ Performance (10 Epochs)

* **Loss & Accuracy:** Training loss decreased from `1.4708` to `1.0097`. Validation loss reached `1.0878`. Both training and validation accuracy reached **85.20%**.
* **IoU & Dice Loss:** IoU improved to `0.4125` (train) and `0.3111` (val).
* **Summary:** U-Net++ demonstrated steady boundary learning and stability across training stages.

#### 4.1.2 DeepLabV3+ Performance (10 Epochs)

* **Test Loss:** `0.1612`
* **Test Accuracy:** **99.99%**
* **IoU (Intersection over Union):** **0.9999**
* **Precision / Recall / F1-Score:** **0.9999** across all metrics.
* **Summary:** DeepLabV3+ achieved near-perfect land cover segmentation performance due to the ASPP module and powerful feature extraction backbone.

---

## V. DISCUSSION & APPLICATION

### Practical Application: Temporal Urban Change Detection

The best-performing model (**DeepLabV3+**) was applied to segment satellite images of a specific region in Vietnam across two distinct time points to monitor land cover transitions.

**Key Observations:**

1. **Land Cover Shift:** The initial time point displayed high density of forested areas (dark green). The second time point revealed a significant reduction in forest cover with a expansion of barren land and agricultural zones.
2. **Environmental Impact:** Highlights deforestation trends likely driven by urban development and agricultural expansion.

**Policy Recommendations:**

* **Forest Protection:** Enforce strict reforestation programs and protection of carbon sinks.
* **Sustainable Land Management:** Mitigate uncontrolled conversion of forests to farmland.
* **Urban Planning:** Promote structured urban expansion balancing economic growth with ecological preservation.

---

## VI. REFERENCES

1. Bousias Alexakis, E., Armenakis, C. (2020). *Evaluation of UNet and UNet++ architectures in high resolution image change detection applications.*
2. Qin, R., & Liu, T. (2022). *A Review of Landcover Classification with Very-High Resolution Remotely Sensed Optical Images Analysis Unit, Model Scalability and Transferability.*
3. Tasar, O., et al. (2020). *ColorMapGAN: Unsupervised Domain Adaptation for Semantic Segmentation Using Color Mapping Generative Adversarial Networks.* IEEE TGRS.
4. Yu, H., et al. (2022). *Development of weed detection method in soybean fields utilizing improved deeplabv3+ platform.*
5. Yuan, H., et al. (2022). *An improved DeepLab v3+ deep learning network applied to the segmentation of grape leaf black rot spots.*
6. Zhou, Z., et al. (2019). *UNet++: Redesigning skip connections to exploit multiscale features in image segmentation.* IEEE TMI.

```