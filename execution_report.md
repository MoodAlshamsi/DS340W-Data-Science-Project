# Execution report: full-mode run of Heart_Disease_Research_LSRO_MLHDPM.ipynb

* **Date:** 2026-11-15, 02:42 to 04:18 (US Eastern).
* **Mode and switch:** `RUN_MODE = "full"`, seed 42, `FINAL_VALIDATION_AUTHORIZED = False`.
* **Machine:** 2 CPU cores, 7 GB RAM, no GPU. Peak memory used by the whole system: 3.2 GB.
* **Software:** Python 3.13.18, numpy 2.5.3, pandas 3.0.5, scikit-learn 1.9.1, imbalanced-learn 0.14.2, xgboost 3.4.1, lightgbm 4.7.0, catboost 1.2.10, optuna 5.0.0, shap 0.54.1, tensorflow 2.21.0.
* **Runtime:** 95.6 minutes (driver log: 95.7 minutes).
* **Cells:** 83 of 83 code cells executed, with 0 error outputs.

## Checks performed
| Check | Result |
|---|---|
| Every code cell executed without an error output | PASS (83/83) |
| Regenerated 70/20/10 split identical to the delivered `train_642.csv`, `test_184.csv`, `validation_92.csv` | PASS |
| No record, duplicate group or feature vector shared between any two datasets | PASS |
| Leakage and isolation audit, Section 29.4 (8 checks: vault unused; LSRO trained only on the 642 training patients; outer folds used training rows only; no training row in test or validation; Platt fitted on 642 out-of-fold scores; test set only scored; LSRO code uses no Section 28 object; Section 21 model unchanged) | PASS (8/8) |
| Validation set evaluated | NO (0 vault accesses; only the dry run on training rows) |
| Test-set accesses (all scoring only; purposes logged) | 107 |
| Fresh Colab-style start: empty folder, data downloaded from the pinned URLs, Python 3.12.3 / scikit-learn 1.6.1 / TF 2.19 | PASS for Sections 1 to 10 (download, MD5 check, split identical) |
| New Sections 28 and 29 on Python 3.12 / scikit-learn 1.6.1 / TF 2.19 (quick settings) | PASS (0 errors) |
| Authorised validation path, tested on a decoy vault built from training rows (earlier quick run) | PASS; both "evaluate once" guards fired |

## Not verified
* The full notebook was not executed in full mode inside Google Colab itself, or end to end in the Python 3.12 environment; only the sections listed above were run there.
* Not tested on a GPU.

## Time per notebook section (full mode)
| Section | Code cells | Minutes | Errors |
|---|---|---|---|
| 4. Package installation | 1 | 0.0 | 0 |
| 5. Imports, configuration and random seeds | 1 | 0.1 | 0 |
| 6. Dataset loading and validation | 2 | 0.0 | 0 |
| 7. Exploratory data analysis (EDA) | 4 | 0.1 | 0 |
| 8. Building blocks | 9 | 0.0 | 0 |
| 9. HISTORICAL reproduction of the original project (LEAKAGE-PRONE, original protocol, kept | 4 | 0.7 | 0 |
| 10. Three-way split (Training / Test / Validation), leakage checks, and leakage ablation | 4 | 0.4 | 0 |
| 11. Corrected baseline: original models and configurations, leakage-free | 2 | 8.0 | 0 |
| 12. Class-balancing comparison (training CV only) | 1 | 0.9 | 0 |
| 13. Feature-selection comparison (training CV only) | 3 | 2.4 | 0 |
| 14. Feature engineering (training CV only) | 1 | 0.3 | 0 |
| 15. Parent-paper protocols | 9 | 31.7 | 0 |
| 16. Deep learning: original networks, the P1 convolutional interpretation, and the P1 netw | 1 | 7.1 | 0 |
| 17. Combinations of methods (chosen from training CV only) | 1 | 0.5 | 0 |
| 18. Hyperparameter optimisation (tuning on training data only; comparison on the test set) | 5 | 6.5 | 0 |
| 19. Ensemble learning (built and compared by training CV; development comparison on the te | 2 | 3.3 | 0 |
| 20. Primary evidence: nested cross-validation of the whole optimisation procedure | 3 | 15.2 | 0 |
| 21. Final model: pre-declared shortlist, chosen on the 20% test set, then frozen | 6 | 2.5 | 0 |
| 22. Subgroup analysis: age groups, sex and hospital | 1 | 0.0 | 0 |
| 23. Cross-hospital generalisation | 1 | 0.1 | 0 |
| 24. Why has accuracy plateaued? Diagnostic experiments | 4 | 0.2 | 0 |
| 25. Error analysis and calibration | 2 | 0.0 | 0 |
| 26. Feature importance and SHAP explanations | 2 | 0.2 | 0 |
| 27. Could more data help? Investigation of public heart-disease datasets | 2 | 0.0 | 0 |
| 28. Model A: HISTORICAL reproduction of ML-HDPM (Al-Alshaikh et al., 2024) on UCI Clevelan | 1 | 2.6 | 0 |
| 29. LSRO-ML-HDPM: Leakage-Safe, Sensitivity-Optimised ML-HDPM (Models B to F) | 7 | 11.3 | 0 |
| 30. FINAL VALIDATION on the untouched 10% (one evaluation, only when authorised) | 1 | 1.7 | 0 |
| 31. Comprehensive comparison | 1 | 0.0 | 0 |
| 32. Export of results | 2 | 0.0 | 0 |
