# Evaluating Heart Disease Prediction Through Data Cleaning, Machine Learning Model Comparison, and Comprehensive Performance Evaluation

**DS 340W, Penn State.** James DiPasquale, Ariana Netterville, Mohammed Alshamsi.

This repository contains one self-contained, Colab-compatible notebook and its outputs. The notebook:
* reproduces the original GitHub project (Liedel, 2022) and corrects its data leakage;
* implements the three parent papers;
* reproduces ML-HDPM (Al-Alshaikh et al., 2024) under its own protocol;
* tests our extension of it: **LSRO-ML-HDPM** (Leakage-Safe, Sensitivity-Optimized ML-HDPM).

> **Status:**
> * The notebook in this repository was executed in **FULL mode** (95.7 minutes on 2 CPU cores, 0 errors).
> * The 92-patient final validation set is **sealed and has not been evaluated**.

## Repository layout
```
Heart_Disease_Research_LSRO_MLHDPM.ipynb   notebook with full-mode outputs
requirements.txt                           Python packages
data/
  train_642.csv  test_184.csv  validation_92.csv  split_summary.csv
results/
  mlhdpm_comparison_full.csv  mlhdpm_ablation_full.csv   main LSRO tables
  tables/   every CSV table the notebook writes
  figures/  every figure the notebook writes
  frozen_final_model_manifest.json         fingerprint of all frozen models
docs/
  Heart_Disease_Research_Paper_Updated.docx
  Team_Recording_Script_Updated.docx
  execution_report.md                      runtimes per section and checks of the full run
  paper_figures/                           the three figures used in the paper
archive/
  Heart_Disease_Research_Final_Optimized_2026-11-08.ipynb       previous 80/20 notebook (historical, not current results)
  Heart_Disease_Research_LSRO_MLHDPM_quickmode_2026-11-15.ipynb quick-mode preview (superseded by the full run)
```
The input dataset `heart_statlog_cleveland_hungary_final.csv` is not stored here. The notebook downloads it from a pinned public commit (see "Data").

## Quick mode vs full mode
| | `RUN_MODE = "quick"` | `RUN_MODE = "full"` |
|---|---|---|
| Purpose | check that every cell runs | the research results reported in the paper |
| Training CV | 3 folds, 1 repeat | 5 folds, 2 repeats |
| GA feature selection | 4 generations, population 20 | 18 generations, population 80 (P1 settings) |
| Neural networks | 60 epochs | up to 500 epochs (early stopping) |
| Optuna | 6 trials per model | 60 trials per model |
| Nested CV | 3 outer folds, 4 models, 4 trials | 5 x 2 outer folds, 10 models, 25 trials |
| LSRO-ML-HDPM | outer 3 / inner 3 folds, class weights {1, 2} | outer 5 / inner 5 folds, class weights {1, 1.5, 2, 3} |
| Runtime (measured, 2 CPU cores) | about 20 minutes | 95.7 minutes |

The split, the seed (42) and every evaluation rule are identical in both modes. Only the computing budget differs.

## Run in Google Colab
1. Open https://colab.research.google.com and choose `File > Open notebook > GitHub`. Paste this repository's URL and open `Heart_Disease_Research_LSRO_MLHDPM.ipynb`.
2. A CPU runtime is enough. A GPU only speeds up the small Keras networks.
3. Leave `ALLOW_DOWNLOAD = True`. The notebook downloads the dataset, UCI Cleveland-303, Parent Paper 2's published files and a Kaggle file used for an overlap check, all from pinned GitHub commits. We tested this download path from an empty folder with Python 3.12 and scikit-learn 1.6.1.
4. Optional: to have the notebook check that the split matches the published files, create a `data/` folder in the Colab file panel and upload the three split CSVs from this repository. The notebook then asserts that its regenerated split is identical.
5. In the configuration cell:
   * set `RUN_MODE = "quick"` for a test or `"full"` for the research run;
   * keep `FINAL_VALIDATION_AUTHORIZED = False`.
6. Choose `Runtime > Run all`. A full run takes several hours, and free Colab sessions can disconnect, so use Colab Pro or run locally for full mode.
7. Download `heart_disease_lsro_mlhdpm_outputs.zip` from the file panel at the end.

## Run locally
```bash
git clone <this-repository-url>
cd <repository-folder>
python -m venv .venv
source .venv/bin/activate            # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook Heart_Disease_Research_LSRO_MLHDPM.ipynb
```
Or run it non-interactively:
```bash
jupyter nbconvert --to notebook --execute Heart_Disease_Research_LSRO_MLHDPM.ipynb --output executed.ipynb --ExecutePreprocessor.timeout=-1
```
Requirements: Python 3.13 (full run tested) or 3.12 (data loading, split and the new Sections 28 and 29 tested), about 3.2 GB of RAM (peak system memory measured during the full run), 2 or more CPU cores, and internet access for the first run (or place the data files next to the notebook).

## Data
* The comprehensive heart disease dataset has 1,190 rows and 918 unique patient records (272 exact duplicates). The notebook downloads it from a pinned mirror and checks its MD5 (`89331abbd1786b4942ab8c588b832d39`).
* **The split.** 70% training (642), 20% test (184) and 10% validation (92) unique patients, stratified by class, duplicate-aware, seed 42. The notebook regenerates it identically on every run. `data/split_summary.csv` documents the sizes, the class balance and the zero-overlap checks.
* **Training** is used for every fitted step: preprocessing, feature selection, balancing, tuning, calibration, thresholds and cross-validation. **Test** is used only for development comparisons. **Validation** is sealed: the notebook can evaluate it only when `FINAL_VALIDATION_AUTHORIZED = True`, and only once; it then writes `outputs/final_validation_receipt.json`. `data/validation_92.csv` is provided for transparency. **Do not use its labels during development.**

## Authorising the final validation (once, by the team)
1. Do this only after the team agrees that the models are final.
2. Set `FINAL_VALIDATION_AUTHORIZED = True` and run all.
3. The pre-declared primary comparison is the F2 score of Model F against Model B.
4. Keep the receipt file.

## Citation of the parent papers
See the references in `docs/Heart_Disease_Research_Paper_Updated.docx`.

## Main results (full mode, test set of 184 patients; validation set sealed)
| Model | CV F2 (5 folds) | Test F2 | Test accuracy | Test recall | Test specificity | Missed cases (FN) | False alarms (FP) |
|---|---|---|---|---|---|---|---|
| B: Leakage-safe ML-HDPM | 0.860 | 0.865 | 0.875 | 0.853 | 0.902 | 15 | 8 |
| C: + cost-sensitive loss | 0.912 | 0.924 | 0.902 | 0.931 | 0.866 | 7 | 11 |
| D: + recall-constrained threshold | 0.899 | 0.888 | 0.886 | 0.882 | 0.890 | 12 | 9 |
| E: + Platt calibration | 0.879 | 0.879 | 0.880 | 0.873 | 0.890 | 13 | 9 |
| F: LSRO-ML-HDPM (C + E + D) | 0.904 | 0.908 | 0.891 | 0.912 | 0.866 | 9 | 11 |
| Benchmark: LightGBM | 0.880 | 0.871 | 0.875 | 0.863 | 0.890 | 14 | 9 |
| Benchmark: cost-sensitive LightGBM + Platt + threshold | 0.883 | 0.900 | 0.886 | 0.902 | 0.866 | 10 | 11 |

* **Best model:** Model C, the cost-sensitive loss alone.
* **Model F vs Model B:** LSRO-ML-HDPM improved on the leakage-safe baseline, but the pre-declared primary comparison (F2 of F vs B in cross-validation) was not statistically significant (+0.044, p = 0.16).
* See `docs/Heart_Disease_Research_Paper_Updated.docx` and the notebook's Section 35 for the full interpretation.
