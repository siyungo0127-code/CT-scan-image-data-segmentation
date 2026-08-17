# Liver and tumour segmentation from CT using U-Net

This repository documents an MSc dissertation project on semantic segmentation of the liver and liver tumours in contrast-enhanced abdominal CT images. The work was completed as part of the MSc Cancer Genomics and Data Science programme at Queen Mary University of London and Barts Cancer Institute.

The project evaluated a PyTorch-based 2D U-Net across seven final experimental configurations. It investigated augmentation, training controls and loss-function composition, with tumour segmentation as the primary task. It does not propose a novel neural-network architecture and is not intended for clinical deployment.

> **Data and code availability:** Medical images, masks, model checkpoints, prediction directories and dissertation documents are not redistributed. The training implementation was based on provided or adapted MSc teaching material; redistribution and licensing of that code remain under review. This repository therefore publishes the experimental methodology, verified aggregate results and non-patient figures only.

## Motivation

Accurate delineation of the liver and liver tumours is an important component of quantitative analysis in liver cancer imaging. Manual segmentation is time-consuming and subject to observer variability. This project investigates how simple augmentation and training-control choices affect a consistent 2D U-Net baseline, with tumour segmentation as the primary evaluation focus.

## Dataset

The project used an arterial-phase hepatocellular carcinoma subset derived from the public dataset described in *Comprehensive multi-phase 3D contrast-enhanced CT imaging for primary liver cancer* ([Scientific Data, 2025](https://www.nature.com/articles/s41597-025-05125-2)). The project records describe 2D axial PNG slices prepared from contrast-enhanced CT volumes, with pixel labels for background, liver and tumour. Original slices were 512 × 512 pixels and were resized to 256 × 256 for training.

The working subset comprised **94 HCC patients** and **10,808 axial CT slices**. A patient-level split was used to prevent slices from the same patient appearing in more than one partition:

| Partition | Patients |
|---|---:|
| Training | 65 |
| Validation | 10 |
| Test | 19 |
| **Total** | **94** |

The source dataset and the prepared project subset are distinct. This repository does not redistribute CT images, masks or patient-level split files. See [docs/dataset.md](docs/dataset.md) for the source and data-governance notes.

## Preprocessing

The volumetric data were represented as 2D axial grayscale PNG slices with corresponding semantic masks. Images and masks were originally 512 × 512 pixels and were resized to 256 × 256 for model input. Masks encoded three classes: background, liver and tumour. Augmentation, where enabled, was applied to matched image-mask pairs. Further preprocessing details are not asserted here because the executable implementation is not present in this checkout.

## Model

The model was a 2D U-Net with an encoder-decoder structure, skip connections and a three-channel output for background, liver and tumour. Each axial slice was processed independently at 256 × 256 pixels. U-Net was originally introduced by Ronneberger, Fischer and Brox; this project did not originate the architecture.

Models were selected using the highest validation tumour Dice score. Final test evaluation used the corresponding best checkpoint and reported Dice Similarity Coefficient and Intersection over Union (IoU) for liver and tumour.

## Training setup

The common training setup was:

- framework: PyTorch;
- optimiser: Adam;
- initial learning rate: `1e-3`;
- batch size: 8;
- input size: 256 × 256;
- output classes: 3; and
- model selection: highest validation tumour Dice.

The selected tumour-segmentation configuration combined cross-entropy and Dice loss. Exact environment versions were not consistently preserved, so the dependency list is intentionally unpinned.

## Experimental design

Seven final configurations were evaluated across the dissertation work:

1. 200 epochs with independent horizontal and vertical flips and combined cross-entropy + Dice loss
2. 200 epochs with horizontal flip and combined cross-entropy + Dice loss
3. 200 epochs with vertical flip and combined cross-entropy + Dice loss
4. 200 epochs without augmentation and with combined cross-entropy + Dice loss
5. Vertical flip with early stopping and combined cross-entropy + Dice loss
6. Vertical flip with a `ReduceLROnPlateau` scheduler and combined cross-entropy + Dice loss
7. Vertical flip with Dice-only loss

This Git checkout contains complete result archives for six of the seven final configurations. The final 200-epoch combined-flip archive is not currently available. A separate preliminary 10-epoch combined-flip run was intentionally excluded from the public final-results set. All recorded runs use random seed 42. Differences between runs must not be interpreted as statistical superiority across repeated training runs. Full configuration notes are in [docs/experimental_design.md](docs/experimental_design.md).

## Archived final results

| Experiment | Max epochs | Completed | Augmentation | Training control | Best epoch | Best val tumour Dice | Test liver Dice | Test tumour Dice | Test liver IoU | Test tumour IoU | Time (min) |
|---|---:|---:|---|---|---:|---:|---:|---:|---:|---:|---:|
| 200-epoch H flip | 200 | 200 | 50% H | None recorded | 13 | 0.679825 | 0.757707 | 0.577675 | 0.675573 | 0.525239 | 330.904799 |
| 200-epoch V flip | 200 | 200 | 50% V | None recorded | 11 | **0.715553** | 0.747455 | **0.621882** | 0.672045 | **0.569305** | 481.476459 |
| 200-epoch no augmentation | 200 | 200 | None | None recorded | 53 | 0.689015 | **0.787348** | 0.593380 | **0.706556** | 0.539853 | 364.202436 |
| V flip + early stopping | 200 | 32 | 50% V | Patience 20 | 12 | 0.693623 | 0.768020 | 0.587174 | 0.688106 | 0.534645 | 52.930939 |
| V flip + LR scheduler | 200 | 200 | 50% V | ReduceLROnPlateau | 24 | 0.700385 | 0.779598 | 0.612370 | 0.700647 | 0.555392 | 339.383337 |
| V flip + Dice-only loss | 200 | 200 | 50% V | Dice-only loss | 27 | **0.734028** | 0.777805 | 0.595854 | 0.701951 | 0.542144 | 289.132888 |

![Test tumour Dice and IoU across the six archived final experiments](figures/test_tumour_performance.png)

### Main findings

- The selected vertical-flip configuration using combined cross-entropy and Dice loss achieved the strongest archived tumour performance: test tumour Dice **0.6219** and tumour IoU **0.5693**. Its liver Dice was **0.7475** and liver IoU was **0.6720**.
- No augmentation achieved the strongest liver performance: liver Dice 0.787348 and liver IoU 0.706556.
- The Dice-only configuration achieved validation tumour Dice 0.7340 and test Dice/IoU of 0.7778/0.7020 for liver and 0.5959/0.5421 for tumour.
- The scheduler produced the lowest test loss (0.557456), but did not outperform standard vertical flip on tumour Dice.
- Early stopping ended at epoch 32 and substantially reduced training time, with lower tumour performance than the full vertical-flip run.
- Horizontal flip was less effective than vertical flip for tumour segmentation in this single-seed comparison.

These observations apply only to the recorded runs and do not establish causal or general superiority.

## Repository structure

```text
.
├── README.md
├── LICENSE-NOTICE.md
├── requirements.txt
├── docs/       # Dataset, experiment, reproducibility and attribution notes
├── figures/    # Aggregate training and validation plots only
└── results/    # Verified comparison table and raw text/CSV records
```

Notebooks and source code are not present in this checkout while redistribution rights for the provided or adapted teaching implementation remain under review.

## Running the project

The published dependencies can be installed with:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

The current repository does not contain an executable training or evaluation entry point, dataset split manifest, checkpoints or medical images. It therefore cannot currently be run end-to-end. Once redistribution permission is confirmed, the training/evaluation source and exact command-line instructions should be added here; users must obtain the dataset separately and preserve the documented patient-level split.

## Reproducibility

The repository provides the recorded configuration, per-epoch histories and test metrics needed to audit six final runs. It does not yet include the final 200-epoch combined-flip archive, dataset, complete execution environment, checkpoints or underlying training implementation; exact rerunning is therefore not currently possible from this repository alone.

See [docs/reproducibility.md](docs/reproducibility.md) for the evidenced dependencies and limitations.

## Citation

If referring to the architecture, cite:

> Ronneberger, O., Fischer, P. and Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. *Medical Image Computing and Computer-Assisted Intervention (MICCAI)*. https://doi.org/10.1007/978-3-319-24574-4_28

Dataset users should cite the dataset publication linked above and follow its current access and licensing conditions. A project-specific citation file has not been added because publication details for this dissertation repository have not yet been finalised.

## Acknowledgements and contribution statement

The U-Net architecture and CT dataset were created by their respective original authors. The implementation used in the MSc project was based on provided or adapted teaching material. My contribution comprised the controlled experiment design, configuration changes, GPU/HPC execution, evaluation, experiment management, comparison and interpretation of results.

See [docs/attribution.md](docs/attribution.md) and [LICENSE-NOTICE.md](LICENSE-NOTICE.md) for the current code-redistribution position.
