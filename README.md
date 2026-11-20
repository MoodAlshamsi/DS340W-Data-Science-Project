# Heart Disease Prediction Using LSRO-ML-HDPM

## About This Project

This is our DS 340W research project at Penn State. We investigated heart disease prediction using machine learning, focusing on reducing missed heart disease cases while using a fair evaluation process and avoiding data leakage.

Our main starting point was ML-HDPM (Al-Alshaikh et al., 2024). We recreated a leakage-safe baseline and evaluated several changes. Our proposed **LSRO-ML-HDPM** approach combines cost-sensitive learning, a recall-focused decision threshold, and probability calibration.

Our goal was not simply to maximize accuracy. We also examined recall and false negatives, including performance on data held out from model training and selection.

## What We Did

- Investigated duplicate records and data leakage in heart disease prediction experiments.
- Used a group-aware splitting approach to prevent identical records from appearing across training, test, and validation partitions.
- Recreated a leakage-safe ML-HDPM baseline (**Model B**).
- Tested cost-sensitive learning (**Model C**) and other individual improvements.
- Combined improvements into **LSRO-ML-HDPM (Model F)**.
- Compared accuracy, recall, F2 score, and missed heart disease cases.
- Evaluated frozen models on a separate final validation set.

The dataset contained **1,190 records before duplicate removal**. After removing **272 duplicate records**, **918 unique records** remained: **642 training**, **184 test**, and **92 final validation** records. These counts refer to dataset records; they do not establish that every row corresponds to a distinct individual patient.

## Results

### Test Set (184 Records)

| Model | Accuracy | Recall | Missed Heart Disease Cases |
|---|---:|---:|---:|
| B — Leakage-safe ML-HDPM baseline | 88.0% | 86.3% | 14 |
| C — Cost-sensitive improvement | **89.7%** | **92.2%** | **8** |
| F — LSRO-ML-HDPM | 88.6% | 90.2% | 10 |

Both C and F improved recall compared with B. Model C had the strongest accuracy and recall among these three models on the test set. These are comparisons on the same test records, not direct evidence of superiority over the parent paper's published results on a different evaluation setup.

### Final Validation Set (92 Records)

| Model | Accuracy | Recall | Missed Heart Disease Cases |
|---|---:|---:|---:|
| B — Leakage-safe ML-HDPM baseline | 87.0% | 82.4% | 9 |
| C — Cost-sensitive improvement | 87.0% | 86.3% | 7 |
| F — LSRO-ML-HDPM | 87.0% | 86.3% | 7 |
| Final selected logistic regression | **88.0%** | 84.3% | 8 |

The final selected model was a default logistic regression with a decision threshold of **0.49**. The threshold was selected using out-of-fold predictions from the training data. The model was selected from a pre-declared shortlist using the test set, where it achieved 88.0% accuracy. Because the test set informed model selection, this test accuracy may be optimistic. The held-out final validation accuracy was also 88.0%.

On final validation, C and F retained a recall advantage over B, although all three had the same accuracy. The validation set was small, so these findings do not establish that any method will always perform better on other datasets.

## Repository Files

| File | Purpose |
|---|---|
| `Heart_Disease_Research_LSRO_ML-HDPM.ipynb` | Main research notebook |
| `requirements.txt` | Python package dependencies |
| `heart_statlog_cleveland_hungary_final.csv` | Main dataset |
| `processed.cleveland.data` | Additional Cleveland dataset used in comparisons |
| `heart_disease_lsro_mlhdpm_outputs.zip` | Archived research outputs |

The results archive includes the following research records:

- `final_validation_receipt.json` — record of the original one-time final validation evaluation.
- `final_validation_results.csv` — saved final validation metrics.
- `mlhdpm_comparison.csv`, `mlhdpm_ablation.csv`, and `split_sizes.csv` — comparison, ablation, and split summaries.

The notebook also generates `hd_components.py` during execution, containing reusable machine-learning pipeline components. Run the notebook sections in their intended order so this module is created before it is used.

## How to Run the Project

### Environment

The project was developed in **Google Colab**. The notebook's setup cells handle its environment-specific installation steps. The complete research configuration may take **several hours**, depending on the runtime.

**Before running:** Verify that the repository contains the notebook, required datasets, and `requirements.txt`. The exact package versions and runtime compatibility must be verified against the notebook on a clean environment.

### Steps for a Fresh-Machine Run

1. On a computer that has **not previously been used to run this project**, open the GitHub repository: https://github.com/MoodAlshamsi/DS340W-Data-Science-Project
2. Download the repository using **Code → Download ZIP**, then extract it, or clone it using Git if Git is installed. For the course recording, **start Zoom recording and screen sharing before this download step**.
3. Open `Heart_Disease_Research_LSRO_ML-HDPM.ipynb` in Google Colab. If using Colab, ensure the notebook and required input files are accessible in the runtime; files stored only on the computer are not automatically available in Colab.
4. Follow the notebook's setup and installation cells. Make sure the datasets are available at the paths expected by the notebook. The notebook can also download its main public datasets when `ALLOW_DOWNLOAD = True`, if its download sources are accessible.
5. Set `RUN_MODE = "full"` to reproduce the full research experiments. Use `RUN_MODE = "quick"` only for a reduced execution check.
6. Keep `FINAL_VALIDATION_AUTHORIZED = False` during normal reproduction runs. Run notebook sections in their documented order, including any setup that generates `hd_components.py`.
7. Allow the full run to finish. Inspect the generated tables and figures in `outputs/` and compare the test-set results with the values in this README and the research paper.
8. Open the saved final-validation records from `heart_disease_lsro_mlhdpm_outputs.zip` and compare them with the final-validation table in this README and the paper.

**Important:** `RUN_MODE = "quick"` uses reduced settings and is **not expected to reproduce the full research metrics**. The official reproducibility demonstration should use the full configuration when the goal is to reproduce the published test results.

### Final Validation and Reproducibility

The final validation set was evaluated once as part of the research procedure, after model selection. The original validation metrics and receipt are preserved in the outputs archive.

For normal reruns, keep:

```python
FINAL_VALIDATION_AUTHORIZED = False
```

With this setting, Section 30 performs a dry run instead of generating a new final-validation evaluation. **Do not turn validation authorization on to retune models, change thresholds, or select a different model based on validation results.**

A fresh full run should reproduce the **test-set** experiments under the notebook's fixed configuration. The **final-validation** numbers in this README are the recorded results of the original evaluation; a normal run with authorization disabled will not independently regenerate them. Review the saved `final_validation_receipt.json` and `final_validation_results.csv` to verify what was originally reported.

**Course reproducibility requirement:** The instructor and TAs require that they be able to reproduce the paper's results on a new machine. Merely displaying archived validation results may not meet that requirement. Before submitting the recording, confirm with the instructor whether the saved one-time validation record is sufficient or whether a separate, read-only verification run of the frozen models is required. Such a verification workflow must not retrain, retune, or select models using validation outcomes.

### Reproducibility Notes

- The reported test results come from the notebook's **full** research configuration and its fixed random seed.
- Exact reproduction may depend on Python and package versions, data availability, and the execution environment.
- Do not change the dataset, seed, split assignments, model settings, or decision thresholds to make the results match.
- If a fresh-machine run produces different metrics, investigate and document the difference rather than editing the reported results.
- Confirm that every table and figure in the final paper agrees with the outputs from the version of the notebook submitted on GitHub.

## Recording Checklist for DS 340W

For the required code demonstration:

1. Use a computer that has **never been used to run this project**.
2. Log in to PSU Zoom and ensure **all team members are present on webcam**.
3. Start recording and share the screen **before downloading the repository**.
4. Show the repository being downloaded from GitHub during the recording.
5. Follow the repository instructions and execute the code without errors.
6. Have **every team member explain portions of the actual code**.
7. Show the resulting tables and figures, compare them with the research paper, and explain which final-validation artifacts are archived rather than regenerated.
8. Make sure the recorded procedure is repeatable by the instructor and TAs.

## What We Learned

One of our main findings was that the evaluation process matters alongside model performance. Duplicate records across data partitions can make reported performance misleading, and evaluation protocols should be compared carefully.

We also found that recall-focused methods reduced missed heart disease cases compared with the leakage-safe baseline. Improvements were more pronounced on the test set and more modest on final validation.

## Limitations and Future Work

The final validation set contained only 92 records, and the results may change on data from other hospitals or patient populations. Model selection using test-set performance may also make test-set estimates optimistic. Future work could include larger independent datasets, studying model calibration across patient groups, and evaluating consistency across external datasets.

This project is for **research and educational purposes only**. It is not a medical diagnosis tool.

## Course

**DS 340W — Penn State**

Research project on heart disease prediction and leakage-safe machine learning evaluation.

**Team members:** Ariana Netterville, James DiPasquale, Mohammed Alshamsi
