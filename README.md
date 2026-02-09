# Object Recognition for Bird Species Classification

This repository contains the implementation of a classification system for bird species based on the Caltech-UCSD Birds-200-2011 (CUB-200-2011) dataset. The project addresses the challenges of fine-grained visual categorization by focusing on a 20-species subset requiring high-resolution feature extraction.

## Methodology

Our approach utilizes a multi-staged pipeline and a heterogeneous ensemble designed to maximize both localized feature detection and global structural stability.

### 1. Training
* **Progressive Resizing:** Models were initially trained at 224x224 pixels and subsequently fine-tuned at 448x448 pixels.
* **Optimization:** We utilized the AdamW optimizer with decoupled weight decay and a Cosine Annealing scheduler to ensure precise convergence during the fine-tuning phase.

### 2. Architecture & Ensemble Selection
The final system is a heterogeneous ensemble of three complementary architectures:
* **ResNet-101:** Selected for deep hierarchical feature extraction through residual connections.
* **EfficientNet-B0:** Utilized for its compound scaling properties, effectively capturing texture details with high parameter efficiency.
* **ResNet-50 (Experimental Addition):** Integrated as a stabilizing component to provide robust structural features, acting as an anchor to smooth out predictions and reduce variance within the ensemble.



### 3. Regularization and Augmentation
* **Label Smoothing:** Hard targets were replaced with softened distributions (factor of 0.1) to prevent overconfident predictions between visually similar species.
* **Mixup Augmentation:** Synthetic training examples were created by linearly combining pairs of images and labels ($\alpha = 0.2$) to encourage smoother decision boundaries.
* **Data Augmentation:** The training pipeline included RandomResizedCrop, RandomHorizontalFlip, RandomRotation (15°), and ColorJitter.

### 4. Inference with 5-Crop TTA
For final inference, we implemented a 5-crop Test-Time Augmentation (TTA) strategy. By evaluating five spatial crops (corners and center) and their horizontal flips, the model averages 10 distinct views per image to determine the final class via soft-voting.

## Results

Performance was evaluated based on validation accuracy at 448x448 resolution. The addition of the three-model ensemble and TTA provided the highest measured accuracy.

| Configuration | Validation Accuracy (%) |
| :--- | :--- |
| ResNet-101 baseline (224px) | 87.38 |
| + Progressive resizing (448px) | 89.32 |
| + Label Smoothing | 90.29 |
| + Mixup | 91.26 |
| + Cosine Annealing | 92.23 |
| **+  Ensemble (3 Models) + 5-Crop TTA** | **94.17** |



## Usage

The repository includes scripts for:
* **Staged Training:** Implementation of the two-stage progressive resizing pipeline.
* **5-Crop Inference:** Script for generating final predictions using the ensemble and TTA strategy.
* **Error Analysis:** Tools for evaluating model performance through confusion matrices and per-class metrics.

## Contributors
* **Joel Anil Jose**
* **Lucia Victoria Fernandez Sanchez**
