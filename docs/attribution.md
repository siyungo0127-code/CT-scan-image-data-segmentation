# Attribution and contribution statement

## Dataset

The contrast-enhanced CT dataset was created and published by its original research team. This project did not create the dataset. The source publication is cited in `docs/dataset.md`, and no medical data are redistributed.

## Architecture

U-Net was introduced by Ronneberger, Fischer and Brox:

> Ronneberger, O., Fischer, P. and Brox, T. (2015). U-Net: Convolutional Networks for Biomedical Image Segmentation. MICCAI. https://doi.org/10.1007/978-3-319-24574-4_28

This dissertation evaluated a 2D U-Net baseline; it did not propose or originate the architecture.

## Implementation provenance

The implementation used in the MSc project was based on provided or adapted MSc teaching material. Its authorship, ownership and redistribution conditions have not yet been confirmed. No notebook or extracted training code is therefore published in this repository.

**TODO:** Obtain written confirmation of provenance and redistribution permission before adding implementation files or assigning a software licence.

## Student contribution

The student's contribution comprised:

- defining and managing the controlled experiment comparison;
- implementing configuration changes for augmentation and training controls;
- conducting GPU/HPC training runs;
- selecting checkpoints using validation tumour Dice;
- evaluating liver and tumour segmentation;
- organising and verifying experiment artefacts; and
- comparing and interpreting the results within the limitations of single-seed experiments.

These contributions should be distinguished from the creation of the dataset, the original U-Net architecture and the underlying teaching implementation.

