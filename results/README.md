# Verified experiment results

This directory contains the authoritative aggregate records for six completed experiments.

## Files

- `experiment_comparison.csv`: consolidated comparison, with metrics formatted to six decimal places.
- `raw/<experiment>/experiment_summary.txt`: original run summary.
- `raw/<experiment>/training_history.csv`: per-epoch metrics.
- `raw/<experiment>/test_results.csv`: final test metrics from the best validation-tumour-Dice checkpoint.

The raw summaries retain their original relative checkpoint paths as historical run records. Checkpoints themselves are not included.

## Verification precedence

Where sources could disagree, the intended precedence is:

1. `experiment_summary.txt`
2. `training_history.csv`
3. `test_results.csv`

For the six included experiments, the best epoch, best validation tumour Dice and test metrics agree across the applicable files. Training-history row counts also agree with the recorded completed epochs.

## Interpretation limits

- Results are from a single recorded random seed (42).
- Comparisons are descriptive rather than tests of statistical significance.
- Per-slice predictions are not included and should not be treated as independent patient-level observations.
- Missing authoritative result directories prevent inclusion of the planned 200-epoch combined-flip and Dice-only experiments.

