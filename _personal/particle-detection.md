---
layout: single
title: "Particle Detection with Dense Neural Networks"
classes: wide
header:
  teaser: /assets/visuals/particle_detection_confusion_matrix.png
teaser_alt: "Confusion matrix for the smallest model, S1: 594,695 true negatives and 626,464 true positives against 105,129 false positives and 73,712 false negatives"
card_summary: "Eight dense networks benchmarked on seven million particle-detection events — five scaling depth and width, three varying activation — to separate what architecture buys you from what activation does."
tech: ["PyTorch", "NumPy", "scikit-learn"]
---

**Description**: A controlled comparison of dense neural network designs for binary particle detection, measuring both predictive quality and the training-time cost of larger networks.

**Tech Stack**: PyTorch, NumPy, scikit-learn

**Coursework**: DS 7333 — Quantifying the World, M.S. Data Science, Southern Methodist University.

## Objective

Detect particles from sensor features (`label` = 1 for detection, 0 for non-detection), and understand what actually drives performance: network capacity, or activation choice?

The suite isolates these two variables. **Five models** scale depth and width while holding activation fixed. **Three models** hold architecture fixed at two hidden layers of 128 units and change only the hidden activation.

## Data

7,000,000 rows, split 80/20 with stratified sampling — a 1,400,000-row validation set that is almost perfectly balanced (699,824 non-detection vs. 700,176 detection). That balance is what makes accuracy and macro-averaged metrics trustworthy here; neither class can dominate the score.

The pipeline reads the gzipped CSV in `float32` for memory efficiency and faster GPU transfer, then standardizes features with `StandardScaler` **fitted only on the training split** to prevent leakage into validation.

## Modeling

All eight models are dense multilayer perceptrons emitting a single logit, trained with `BCEWithLogitsLoss` and Adam at `1e-3`, thresholded at 0.5 after sigmoid.

Every run uses **early stopping** (patience 15) and restores the best validation-loss checkpoint before evaluation — so models that converge at different speeds are compared fairly rather than being penalized for a fixed epoch budget.

## Results

| Model | Variant | Params | Epochs | Train time (s) | Val loss | Accuracy | F1 |
|---|---|---|---|---|---|---|---|
| S1 | 1 × 32, ReLU | 961 | 267 | 18,717 | 0.280 | 0.872 | 0.875 |
| S2 | 1 × 64, ReLU | 1,921 | 183 | 12,643 | 0.273 | 0.876 | 0.879 |
| S3 | 2 × 64, ReLU | 6,081 | 251 | 17,267 | 0.261 | 0.883 | 0.885 |
| S4 | 2 × 128, ReLU | 20,353 | 167 | 12,165 | 0.257 | 0.885 | 0.887 |
| **S5** | **3 × 128, ReLU** | **36,865** | **140** | **10,474** | **0.255** | **0.886** | **0.888** |
| A_relu | 2 × 128, ReLU | 20,353 | 167 | 12,133 | 0.257 | 0.885 | 0.887 |
| A_tanh | 2 × 128, tanh | 20,353 | 300 | 21,814 | 0.256 | 0.885 | 0.887 |
| A_elu | 2 × 128, ELU | 20,353 | 242 | 17,345 | 0.258 | 0.884 | 0.887 |

**Architecture dominates activation.** Accuracy climbs steadily from 0.8723 (S1) to 0.8859 (S5) as depth and width increase. Within the fixed-architecture activation comparison, the spread is only ~0.001 — tanh edges out ReLU, ELU trails slightly, but all three land in the same place.

### Best vs. worst model

| | S5 (3 × 128) | S1 (1 × 32) |
|---|---|---|
| Accuracy | 0.8859 | 0.8723 |
| Non-detection F1 | 0.8836 | 0.8693 |
| Detection F1 | 0.8880 | 0.8751 |

Across every model, **detection recall exceeds non-detection recall** — the networks lean toward predicting detection, catching more positives at some cost to specificity. As capacity grows, both recalls improve and the precision/recall gap narrows, so the larger models aren't just trading one error for another.

## Speed vs. accuracy

The most counterintuitive result: **the largest model was also the fastest to train.** S5 has the most parameters but converged in 140 epochs and finished in 10,474 seconds — less wall-clock time than the much smaller S1, which needed 267 epochs.

Early stopping is what inverts the usual relationship. Wall-clock cost tracks *how many epochs a model needs*, not just its per-epoch cost, and higher-capacity networks reached their best validation loss sooner. A_tanh shows the opposite end of the same effect: it ran the full 300 epochs to gain essentially nothing over A_relu, making it the slowest variant for no measurable benefit.

## Recommendation

Use **S5** when validation quality is the priority — it is both the most accurate and, in this run, the cheapest to train. For rapid iteration, the two-layer 128-unit ReLU network gives nearly identical quality with a well-behaved training profile.

The transferable lesson: spend the tuning budget on depth and width first, and treat activation as a secondary optimization once the architecture is settled.
