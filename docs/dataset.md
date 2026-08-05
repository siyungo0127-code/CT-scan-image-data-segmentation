# Dataset

## Source

The MSc project used an arterial-phase hepatocellular carcinoma subset derived from the public dataset described in:

> *Comprehensive multi-phase 3D contrast-enhanced CT imaging for primary liver cancer*. Scientific Data (2025). https://www.nature.com/articles/s41597-025-05125-2

The complete source dataset and the prepared project subset are not the same resource. This repository does not redistribute either one. Prospective users should consult the dataset publication and current access conditions directly.

## Project representation

The dissertation records describe:

- contrast-enhanced abdominal CT;
- arterial-phase hepatocellular carcinoma data;
- 2D axial PNG slices prepared from volumetric imaging;
- original slice dimensions of 512 × 512 pixels;
- training inputs resized to 256 × 256 pixels; and
- three segmentation classes: background, liver and tumour.

## Unresolved cohort-size discrepancy

The available project records are inconsistent: one record describes 92 patients, whereas notebook documentation describes 94 patients. The authoritative experiment summaries do not state a patient count. This repository therefore does not select either value as definitive.

The discrepancy should be resolved against the original split manifest or dissertation methods record before a cohort count is quoted publicly. Likewise, the size of the complete source dataset must not be presented as the number of images used for model training.

## Data governance

No CT image, mask, patient-style filename or per-slice prediction is included. This omission protects participant privacy and avoids making unsupported redistribution claims. The public repository contains aggregate experimental metrics and non-patient plots only.

