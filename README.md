# GlassBox XAI – Lightweight Medical Imaging Prototype

A solo-built, end-to-end system for interpretable, infrastructure-light medical imaging. Designed to explore segmentation, explainability, and auditability under hardware and data constraints.

---

## Model Architecture, Training, and System Information

- **Custom architecture**: U-Net with attention bottleneck  
- **Dataset**: ISIC 2018 – Task 1: Lesion Segmentation  
- **Custom Loss functions**: Dice, Tversky, Hybrid
- **Training Pipeline** includes robust augmentation, fine tuning, and callbacks for early stopping, iterative checkpoints, and LR scheduling 
- **Model-agnostic** utilities for evaluation, batch metrics, confusion matrices, comparison, and explainability
- **Fully modular** XAI + evaluation pipeline supports any model, batch, or layer combination
- **Modular** image processing stack supports image/mask alignment, augmentation, and pre-inference transformations
- **Plug-and-play** functions for metric reporting, variant comparison, and visual overlays
- **LLM Integration** for accurate and efficient batch metric retrieval using lightweight tinyllama 1.1B.

---

## Visuals: Auditability & HITL Design

- Full Layerwise Grad-CAM, Integrated Gradients, Saliency, Pixel Confidence
- Variant model comparative segmentation
- Outputs and heatmap overlays designed for **review, regulatory compliance, and clinical workflow integration**  
- Transparency layers directly support **human-in-the-loop (HITL)** processes
- Full training, segmentation, and XAI pipeline runs on **modest hardware** with no external dependencies (cloud, expanded data, pretrained models)

---

## Generalizability

System structure, transparency methods, and audit frameworks generalize to:

- Healthcare diagnostics  
- Agriculture 
- Quality control 

---

## Metrics 

| Model              | Dice     | IoU      | Precision | Recall   | F1 Score | Pixel Accuracy |
|-------------------|----------|----------|-----------|----------|----------------|----------|
| **Precision-Optimized**   | **0.8751** | **0.8000** | **0.8870**  | 0.9052   | **0.8751**     | **0.9272**   |
| **Balance-Optimized**| 0.8735   | 0.7925   | 0.8624    | 0.9228   | 0.8735         | 0.9267 |
| **Recall-Optimized** | 0.8573   | 0.7669   | 0.8149    | **0.9444** | 0.8573         | 0.9182   |

<details>
<summary>View Test Set Evaluation Metrics</summary>

![Test Set Evaluation - Model 1](output/eval_model_1.png)

![Test Set Evaluation - Model 2](output/eval_model_2.png)

![Test Set Evaluation - Model 3](output/eval_model_3.png)
*Test Set Evaluation comparison across variant models.*

</details>

---

## Pixel-Level Error

**False positive (FP) and false negative (FN) rates are derived from confusion matrices and represent the percentage of total test set pixels. These values align with reported pixel accuracy.**

| Model                     | False Positives (%) | False Negatives (%) |
|---------------------------|---------------------|----------------------|
| **Precision-Optimized**            | ~2.5%               | ~4.8%                |
| **Balance-Optimized**         | ~3.3%               | ~4.0%                |
| **Recall-Optimized**          | ~5.2%               | ~3.0%                |

<details>
<summary>View Confusion Matrices</summary>

![Confusion Matrix - Model 1](output/cm_model_1.png)

![Confusion Matrix - Model 2](output/cm_model_2.png)

![Confusion Matrix - Model 3](output/cm_model_3.png)
*Confusion Matrix comparison across variant models.*

</details>

---

## Basic Mask Output vs Ground Truth Mask

**Note**: A single batch featuring mean performance, batch 78, is displayed across all visuals for transparency. This batch's performance metrics closely match Model 1's performance on the full 1000 image test set. All functions are fully modular and can be passed any model, batch, layer, or XAI technique. All outputs generated at time of inference.

Basic mask output for batch 78 using Model 1 (Precision-Optimized) to illustrate how overlays are constructed, step by step, from model output.

<details>
<summary>Basic Mask Output</summary>

![Multi-Model - Variant Comparison Visual](output/mask_model_1.png)
*Side-by-side comparison of mask output vs. ground truth masks.*

</details>

---

## Segmentation Overlay vs Ground Truth Overlay

Segmentation Overlay for Model 1 (Precision-Optimized). 

<details>
<summary>Basic Mask Output</summary>

![Multi-Model - Variant Comparison Visual](output/overlay_model_1.png)
*Side-by-side comparison of model-produced segmentation overlay vs. ground truth overlay.*

</details>

---

## Segmentation Overlay Comparison of Variant Models vs Ground Truth Overlay with Batch Metrics

Three model variants with same architecture but different loss functions for specialized error profiles. Outputs compared over shared test batch vs ground truth overlays.

<details>
<summary>View Comparative Model Outputs</summary>

![Multi-Model - Variant Comparison Metrics](output/batch_metrics.png)

![Multi-Model - Variant Comparison Visual](output/overlay_multi_model.png)
*Side-by-side comparison of segmentation output vs. ground truth across three model variants.*

</details>

---

## LLM Integration for Batch Metric Retrieval

Accurate and efficient batch metric retrieval using JSON structured data and Tinyllama 1.1B. Retrieved metrics match calculated and saved metrics for each batch.

<details>
<summary>View LLM Integration for Batch Metric Retrieval</summary>

### Model 1 Batch Metric Data for Comparison
![Model 1 - LLM Integration for Batch Metric Retrieval](output/batch_metric_data.png)

### Model 1 Batch Metric LLM Retrieval
![Model 1 - LLM Integration for Batch Metric Retrieval](output/gb_llm_retrieval_model_1.png)

</details>

---

## Superpixel Confidence Mapping for XAI

Pixel-level confidence for model's segmentation decisions. 200 segments applied for visualization.

<details>
<summary>View Pixel Confidence Mapping</summary>

![Model 1 - Confidence Map Output](output/sp_model_1_78.png)
*Heatmap displaying class confidence for each pixel, grouped by like pixels, for transparency.*

</details>

---

## Layer-wise Grad-CAM for Decoder Layers for XAI

Visualization of decoder layers only. Any layer or grouping can be visualized with this technique, but encoder, bottleneck, and output layer not shown here. Optional smoothing applied for visualization.

<details>
<summary>View Layer-wise Grad-CAM for Decoder Layers</summary>

![Model 1 - Layer-wise Grad-CAM for Decoder Layers](output/lwgc_A_dec_model_1.png)
*Heatmap of contribution to output across layers for transparency.*

</details>

---

## Saliency Mapping for XAI

Basic saliency mapping shows model sensitivity without regard to attribution. Scaling factor of 10 applied for visualization.

<details>
<summary>View Basic Saliency Mapping</summary>

![Model 1 - Basic Saliency Mapping](output/sal_model_1.png)
*Heatmap of sensitivity for transparency.*

</details>

---

## Saliency Mapping with Smooth Grad for XAI

Saliency mapping averaging 50 samples for more robust visualization. Scaling factor of 10 applied for visualization. 

<details>
<summary>View Saliency Mapping with Smooth Grad</summary>

![Model 1 - Saliency Mapping with Smooth Grad](output/sal_sg_model_1.png)
*Heatmap of sensitivity, with smooth grad applied, for transparency.*

</details>

---

## Saliency Mapping with Guided Backpropogation for XAI

Saliency mapping with guided backpropogation. Scaling factor of 10 applied for visualization. 
<details>
<summary>View Saliency Mapping with Guided Backpropogation</summary>

![Model 1 - Saliency Mapping with Guided Backpropogation](output/sal_gb_model_1.png)
*Heatmap of saliency, with guided backpropogation, for transparency.*

</details>

---

## Integrated Gradient Mapping for XAI

Integrated gradient mapping for attribution. Parameters applied for visualization: scaling factor of 1, 50 steps, tf.zeros_like baseline. All parameters are modular.

<details>
<summary>View Integrated Gradient Mapping</summary>

![Model 1 - Integrated Gradient Mapping](output/ig_out_model_1.png)
*Heatmap of gradient attribution for transparency.*

</details>

---

### Connect & Contact

Available for live walkthroughs, Q&A, and technical deep dives on this project.

**Jeffrey Robert Lynch** [LinkedIn](https://www.linkedin.com/in/jeffrey-lynch-350930348)

(Demo access, source code discussions, and use-case exploration available upon request.)

---

## Disclaimer

This project is a research-oriented prototype for educational and exploratory purposes only. It is **not a certified medical device**, and I am **not a licensed medical professional**. No part of this work should be used for clinical decision-making without expert validation and regulatory approval.
