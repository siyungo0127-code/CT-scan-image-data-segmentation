# Reproducibility

## What is available

Seven final configurations were evaluated in the dissertation. For each of the six final configurations with complete records in this Git checkout, the repository provides:

- the original experiment summary;
- the per-epoch training history;
- the test-result table;
- a training-loss plot; and
- a validation-Dice plot.

The consolidated table in `results/experiment_comparison.csv` was transcribed from those records using the precedence: experiment summary, training history, then test results. The preliminary 10-epoch HV-flip run is excluded. Cross-checking found no numerical disagreement among the included source files.

## Evidenced software dependencies

The project materials directly evidence the following Python packages:

- PyTorch
- NumPy
- pandas
- Matplotlib
- Pillow
- tqdm

Exact versions were not recorded consistently, so `requirements.txt` intentionally contains no version pins.

## Withheld components

The repository does not include:

- medical images or masks;
- patient-level split manifests;
- checkpoints;
- prediction directories;
- the original HPC environment; or
- notebooks and training source code whose redistribution status remains under review.

Consequently, the published artefacts support result auditing but do not currently permit an exact end-to-end rerun. This repository should not be described as fully reproducible until the required data access, split manifest, executable implementation and environment specification can be supplied lawfully.

## Verification procedure

For each experiment:

1. Count the data rows in `training_history.csv` to establish epochs completed.
2. Find the maximum `val_dice_tumor` and its epoch.
3. Compare those values with `experiment_summary.txt`.
4. Compare all reported test values with `test_results.csv`.

The current public records pass these checks. The final 200-epoch HV-flip records are not yet present in this Git checkout.
