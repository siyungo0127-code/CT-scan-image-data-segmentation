# Experimental design

## Objective

The controlled comparison evaluated how simple augmentation and training controls affected a consistent PyTorch 2D U-Net baseline. Tumour segmentation was the primary focus.

## Recorded common configuration

- Random seed: 42
- Training image size: 256 × 256 pixels
- Batch size: 8
- Initial learning rate: 0.001
- Optimiser: Adam
- Output classes: background, liver and tumour
- Model selection: highest validation tumour Dice
- Final evaluation: best selected checkpoint on the held-out test split
- Metrics: Dice Similarity Coefficient and Intersection over Union

Loss-function composition was one of the investigated factors. The selected vertical-flip configuration used combined cross-entropy and Dice loss. Optimiser and loss fields were not written into the archived experiment summaries, so these details come from the final dissertation record rather than the result CSV files.

## Final experiments

| Identifier | Configuration |
|---|---|
| Final 200-epoch HV flip (archive missing) | 200 epochs; independent 50% horizontal and 50% vertical flips; combined cross-entropy + Dice loss |
| `epoch200_Hflip` | 200 epochs; 50% horizontal flip |
| `epoch200_Vflip` | 200 epochs; 50% vertical flip |
| `epoch200_noaug` | 200 epochs; no augmentation |
| `epoch200_Vflip_earlystop` | Vertical flip; maximum 200 epochs; early stopping on validation tumour Dice, maximised with patience 20, minimum improvement 0.0 and strict improvement rule |
| `epoch200_Vflip_scheduler` | Vertical flip; 200 epochs; ReduceLROnPlateau on validation tumour Dice, mode max, factor 0.5, patience 10, absolute threshold 0.001, cooldown 0 and minimum learning rate 0.000001 |
| `epoch200_Vflip_diceonly` | 200 epochs; 50% vertical flip; Dice-only loss over liver and tumour with background excluded |

The early-stopping run stopped at epoch 32; its best checkpoint was selected at epoch 12. The scheduler reduced the learning rate ten times and reached 0.000001.

Six configurations have complete authoritative result directories in this Git checkout. Only the final 200-epoch HV-flip directory is absent. The preliminary `epoch10_HVflip` records were removed from the proposed public repository state because they are not part of the final experimental design.

## Scope and limitations

All verified experiments used one recorded seed. The comparison is descriptive and does not quantify variability across repeated runs. It must not be described as statistically significant or as demonstrating general superiority.

The preliminary 10-epoch HV-flip archive must not be used as a substitute for the missing final 200-epoch HV-flip result.
