# Deep Learning for Perception — Assignment 2 (CIFAR-10)

**Student ID:** 23F-0632  
**Notebook:** `DL_ASS01_23F_0632.ipynb`

## Overview
Small-data CIFAR-10 image classification with a from-scratch SmallInceptionCNN (85,538 trainable parameters). The experiment uses 500 sampled images (seed 42), split class-balanced into 400 training and 100 validation images. Models were trained under seven configurations, with three seeds (42, 123, 2026) and a maximum of 60 epochs per run. The official 10,000-example test split was kept out of model selection.

## Method and selection
All 7 configurations were compared by three-seed validation means. The predeclared selection rule was highest mean validation accuracy, then lowest standard deviation. Baseline and geometric tied at 48.67%; baseline was selected on lower validation standard deviation (2.31 vs 3.21). The selected configuration was not changed after evaluating test data.

## Results (mean ± sample standard deviation across three seeds, %)

| Configuration | Validation | Clean test | Specified shifts only | Drop (percentage points) |
|---|---:|---:|---:|---:|
| Baseline (selected) | 48.67 ± 2.31 | 41.27 ± 0.93 | 18.24 ± 1.18 | 23.03 |
| Geometric | 48.67 ± 3.21 | 42.43 ± 1.26 | 24.38 ± 0.95 | 18.05 |
| Photometric | 47.67 ± 2.52 | 40.54 ± 1.33 | 19.99 ± 3.25 | 20.55 |
| Combined | 46.00 ± 1.00 | 38.54 ± 0.45 | 23.92 ± 2.42 | 14.62 |
| Combined + Cutout | 43.00 ± 3.00 | 36.40 ± 2.60 | 25.14 ± 1.31 | 11.26 |
| Combined + Mixup | 42.00 ± 3.61 | 37.57 ± 1.61 | 24.48 ± 0.66 | 13.09 |
| Final candidate | 45.33 ± 3.51 | 41.39 ± 0.47 | 27.43 ± 0.53 | 13.96 |

## Corruption disclosure (important)
The `specified-shifts-only` evaluation uses an index-seeded, reproducible combination of rotation ±25°, brightness and contrast factors from 0.7 to 1.3, and a black 16×16 occlusion. Some implementation choices were made for reproducibility, not supplied verbatim by the instructor. **The additional hidden corruption was not provided.** Thus these shifted scores are *not* results for the official full shifted-test specification; the hidden-corruption-only metric is pending. See `experiment_results/shift_protocol.json`.

## Failure analysis
The locked baseline's seed-42 checkpoint achieved 40.23% on the clean test set; per-class recall ranged from 9.4% (cat) to 64.4% (ship). Confusion matrix and eight misclassified example images are saved in `experiment_results/`.

## Files to include

```text
DL_ASS01_23F_0632.ipynb
experiment_results/
  baseline_seed42.pth ... final_candidate_seed2026.pth
  validation_summary.csv
  all_clean_test_results.csv
  robustness_summary.csv
  model_selection.json
  shift_protocol.json
  ablation_validation.png
  confusion_matrix_seed42.png
  failure_examples_seed42.png
  clean_vs_specified_shifts.png
DL_Assignment2_23F-0632_One_Page_Summary.docx
Section_A_scanned.pdf  # add your genuine handwritten scan
```

## Running / reproducing
Open the executed notebook in Google Colab with PyTorch and torchvision, use CIFAR-10 download, and run its setup sections. Training all 21 models is expensive: evaluation may reuse the checkpoint files. Confirm normalized preprocessing and class-balanced split match the notebook. Google Drive experiment ZIP is a backup; unpack before loading checkpoints from `experiment_results/`. This repository does not currently include an instructor-supplied hidden corruption.

## Source of results
Numbers above were transcribed from recorded Colab console outputs. Add the executed notebook, checkpoint ZIP or accessible results, and actual figures before publishing.
