# Evaluative Process for 3D Content Creation from High-Resolution Text

This repository presents research on evaluating 3D content creation from high-resolution text inputs using advanced techniques like CLIP and loss functions tailored for text-to-3D mesh generation.

## Table of Contents
- [Introduction](#introduction)
- [Background](#background)
  - [High-Resolution Text-to-3D Mesh Generation](#high-resolution-text-to-3d-mesh-generation)
  - [How CLIP Supports Text-to-3D Generation](#how-clip-supports-text-to-3d-generation)
  - [Current Loss Models](#current-loss-models)
    - [Diffusion Prior Loss](#diffusion-prior-loss)
    - [Regularization Losses (Geometry)](#regularization-losses-geometry)
    - [Regularization Losses (Texture)](#regularization-losses-texture)
    - [CLIP-Based Loss](#clip-based-loss)
    - [Viewpoint and Augmentation Losses](#viewpoint-and-augmentation-losses)
- [The Unsolved Problem of CLIP-Loss](#the-unsolved-problem-of-clip-loss)
- [Methodology](#methodology)
  - [Baseline Approaches](#baseline-approaches)
  - [Proposed Information-Theoretic Weighting Approach](#proposed-information-theoretic-weighting-approach)
    - [Preprocessing Pipeline](#preprocessing-pipeline)
    - [Enhanced Dataset Schema](#enhanced-dataset-schema)
    - [CLIP R-Precision Calculation](#clip-r-precision-calculation)
- [Dataset](#dataset)
- [Experimental Design](#experimental-design)
  - [Loss Calculation](#loss-calculation)
  - [Linear Probe Training](#linear-probe-training)
  - [Performance Evaluation](#performance-evaluation)
- [References](#references)

---

## Introduction

This project explores advancements in high-resolution text-to-3D mesh generation using OpenAI’s CLIP, differentiable rendering, and various optimization techniques. The focus is on overcoming challenges in current loss functions to improve the quality and fidelity of generated 3D models.

---

## Background

### High-Resolution Text-to-3D Mesh Generation

Generating high-resolution 3D meshes from text involves transforming textual descriptions (e.g., "Evergreen tree") into accurate and detailed 3D shapes. Challenges include the need for computational resources and reliance on 3D-text datasets. Recent advancements use **zero-shot learning** powered by tools like **CLIP**.

### How CLIP Supports Text-to-3D Generation

- **Differentiable Rendering:** Converts 3D models into 2D images for evaluation.
- **CLIP-Based Optimization:** Measures similarity between rendered images and text descriptions, driving iterative improvements.

### Current Loss Models

#### Diffusion Prior Loss
Refines embeddings based on text-image pairs for improved consistency.

#### Regularization Losses (Geometry)
Maintains smooth shapes using techniques like **Laplacian Regularization**.

#### Regularization Losses (Texture)
Optimizes textures to avoid artifacts using maps like **normal maps**.

#### CLIP-Based Loss
Measures cosine similarity between text prompts and rendered images.

#### Viewpoint and Augmentation Losses
Encourages consistency across different angles and viewpoints.

---

## The Unsolved Problem of CLIP-Loss

CLIP's 2D training bias causes challenges in generating consistent 3D shapes. Without sufficient constraints, optimization can lead to tangled or noisy meshes. Techniques like **regularization constraints** and **viewpoint augmentation** help mitigate these issues but increase computational demands.

---

## Methodology

### Baseline Approaches

- **Dream Fields** and **CLIP-Mesh**: Evaluate single views at specific elevations.
- **DreamFusion**: Averages across azimuths to reduce variance.

### Proposed Information-Theoretic Weighting Approach

#### Preprocessing Pipeline
- Generate four distinct views per mesh.
- Compute **Shannon entropy** for each view as a measure of informational complexity.
- Apply exponential weighting based on entropy using a hyperparameter `p`.

#### Enhanced Dataset Schema
Each 3D mesh is enriched with:
- Human-annotated quality scores (MOS).
- View-specific weights and R-Precision scores.

#### CLIP R-Precision Calculation
- Compute individual R-Precision scores for each view.
- Apply entropy-based weights.
- Generate a weighted mean R-Precision score.

---

## Dataset

The project uses the **LIRIS lab's Graphics-LPIPS dataset**:
- Over 343,000 stimuli from 55 source models.
- A subset of 3,000 stimuli with human-annotated quality scores (MOS).

---

## Experimental Design

### Loss Calculation
Compute weighted CLIP R-Precision scores for various values of `p`.

### Linear Probe Training
Use 5-fold cross-validation to train a linear probe mapping R-Precision scores to MOS values.

### Performance Evaluation
Evaluate predicted MOS values against human-annotated ground truth using **Mean Squared Error (MSE)**.

---

## References

- Khalid et al., 2022: [CLIP-Mesh](https://arxiv.org/abs/xxxxxxx)
- Jain et al., 2022: [Dream Fields](https://arxiv.org/abs/xxxxxxx)
- Nehmé et al., 2023: [Graphics-LPIPS Dataset](https://arxiv.org/abs/xxxxxxx)

For more details, refer to the full research paper.
