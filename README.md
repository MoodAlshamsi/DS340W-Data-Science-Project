# Heart Disease Prediction Using LSRO-ML-HDPM

## About This Project

This is our DS 340W research project at Penn State. We worked on improving heart disease prediction using machine learning, with a focus on reducing missed heart disease cases while keeping the evaluation fair and avoiding data leakage.

We started by studying existing heart disease prediction methods, especially ML-HDPM (Al-Alshaikh et al., 2024). We then recreated a leakage-safe version of that approach and tested several improvements. Our main proposed method is **LSRO-ML-HDPM**, which combines cost-sensitive learning, a recall-focused decision threshold, and probability calibration.

Our goal was not just to get a high accuracy number. We wanted to see whether the improvements could identify more patients with heart disease on data that was not used to train the models.

## What We Did

- Investigated duplicate records and data leakage in heart disease prediction experiments.
- Used a patient-group-aware split so identical patient records would not appear across training, test, and validation sets.
- Recreated a corrected ML-HDPM baseline (**Model B**).
- Tested cost-sensitive learning (**Model C**) and other individual changes.
- Combined the improvements in our proposed **LSRO-ML-HDPM (Model F)**.
- Compared accuracy, recall, F2 score, and missed heart disease cases.
- Evaluated the frozen models on a separate final validation set.

The dataset contains **918 unique patient records** after accounting for duplicates. We used **642 training**, **184 test**, and **92 final validation** patients.

## Results

### Test Set (184 Patients)

| Model | Accuracy | Recall | Missed Heart Disease Cases |
|---|---:|---:|---:|
| B — Leakage-safe ML-HDPM baseline | 88.0% | 86.3% | 14 |
| C — Cost-sensitive improvement | **89.7%** | **92.2%** | **8** |
| F — LSRO-ML-HDPM | 88.6% | 90.2% | 10 |

Both C and F improved recall compared with B. Model C had the strongest accuracy and recall on the test set. Our proposed combined method, F, also missed fewer heart disease cases than B. These are comparisons on the same test patients, not evidence of a direct head-to-head win against the parent paper’s published results on a different dataset.

### Final Validation Set (92 Patients)

| Model | Accuracy | Recall | Missed Heart Disease Cases |
|---|---:|---:|---:|
| B — Leakage-safe ML-HDPM baseline | 87.0% | 82.4% | 9 |
| C — Cost-sensitive improvement | 87.0% | 86.3% | 7 |
| F — LSRO-ML-HDPM | 87.0% | 86.3% | 7 |
| Final selected logistic regression | **88.0%** | 84.3% | 8 |

The final selected model was a default logistic regression with a decision threshold of 0.49. The threshold was selected using out-of-fold predictions from the training data, while the model itself was selected from a pre-declared shortlist using the test set. Its test accuracy was 88.0%. Because the model was chosen using the test set, that test result may be optimistic; its held-out validation accuracy of 88.0% provides a more independent estimate.

On final validation, C and F kept a recall advantage over B, although their accuracy was the same. The validation sample was relatively small, so these results should not be taken as proof that one method is always better.

## Files

- **`Heart_Disease_Research_LSRO_ML-HDPM.ipynb`** — main research notebook.
- **`requirements.txt`** — Python packages used by the project.
- **`heart_statlog_cleveland_hungary_final.csv`** — main dataset.
- **`processed.cleveland.data`** — additional Cleveland dataset used in comparisons.
- **`heart_disease_lsro_mlhdpm_outputs.zip`** — exported research results archive.
- **`final_validation_receipt.json`** — record of the one-time final validation evaluation (included in the results archive).
- **`final_validation_results.csv`** — final validation metrics (included in the results archive).
- **`mlhdpm_comparison.csv`**, **`mlhdpm_ablation.csv`**, **`split_sizes.csv`** — comparison, ablation, and split summaries (included in the results archive).

The notebook also writes `hd_components.py` during execution. It contains reusable components used by the machine-learning pipeline.

## How to Run the Project

We developed the project in **Google Colab**. To run it:

1. Download or clone this repository.
2. Open the main `.ipynb` notebook in Google Colab.
3. Make sure the required dataset files are available to the notebook. The notebook can also download its main public datasets when `ALLOW_DOWNLOAD = True`.
4. Run the setup/install cells and follow the notebook sections in order.
5. Use `RUN_MODE = "quick"` to check that the pipeline works with reduced settings, or `RUN_MODE = "full"` to run the full research configuration.
6. Keep `FINAL_VALIDATION_AUTHORIZED = False` for ordinary reruns. This makes Section 30 perform a dry run rather than re-evaluating the sealed validation patients. **Do not set it to `True` again.** The validation set has already been evaluated once; using it again to revise the models would compromise its independence.
7. Review the generated tables and figures in the `outputs/` folder.

**Important:** The full research run can take several hours, depending on the machine. Quick mode is for checking execution and is **not expected to reproduce the full-run research metrics**.

### Reproducing Reported Results

The test-set results above come from the full research configuration with the fixed seed used in the notebook. The final validation figures come from the previously recorded one-time evaluation of the frozen models.

For a fresh-machine demonstration, run the notebook using the full configuration and compare the test results with the tables above. Keep the original final validation receipt and results file as part of the research record. A normal rerun with validation authorization disabled will not regenerate those final-validation metrics.

Exact reproduction can depend on the Python environment and package versions. The repository's dependencies and setup instructions should be checked on a clean machine before the final demonstration.

## What We Learned

One of the biggest findings was that preventing data leakage matters as much as improving the model itself. Very high reported accuracy can be misleading when duplicate patient records appear in both training and testing.

We also found that focusing on recall can reduce the number of missed heart disease cases. The improvements were clearer on the test set and more modest on final validation. That difference is important to report honestly.

## Limitations and Future Work

Our final validation set contained only 92 patients, and the results may change on other hospitals or patient populations. Future work could include testing on larger independent datasets, studying model calibration across patient groups, and evaluating how the models perform in real clinical settings.

This project is for **research and educational purposes only**. It is not a medical diagnosis tool.

## Course

**DS 340W — Penn State**

Research project on heart disease prediction and leakage-safe machine learning evaluation.

**Team members:** Ariana Netterville, James DiPasquale, Mohammed Alshamsi
