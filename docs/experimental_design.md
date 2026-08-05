# Experimental design

## Objective

The controlled comparison evaluated how simple augmentation and training controls affected a consistent PyTorch 2D U-Net baseline. Tumour segmentation was the primary focus.

## Recorded common configuration

- Random seed: 42
- Training image size: 256 × 256 pixels
- Batch size: 8
- Initial learning rate: 0.001
- Output classes: background, liver and tumour
- Model selection: highest validation tumour Dice
- Final evaluation: best selected checkpoint on the held-out test split
- Metrics: Dice Similarity Coefficient and Intersection over Union

The optimiser and loss function are not stated in the authoritative experiment summaries or CSV files and are therefore not asserted in the verified public comparison.

## Verified experiments

| Identifier | Configuration |
|---|---|
| `epoch10_HVflip` | 10 epochs; independent 50% horizontal and 50% vertical flips |
| `epoch200_Hflip` | 200 epochs; 50% horizontal flip |
| `epoch200_Vflip` | 200 epochs; 50% vertical flip |
| `epoch200_noaug` | 200 epochs; no augmentation |
| `epoch200_Vflip_earlystop` | Vertical flip; maximum 200 epochs; early stopping on validation tumour Dice, maximised with patience 20, minimum improvement 0.0 and strict improvement rule |
| `epoch200_Vflip_scheduler` | Vertical flip; 200 epochs; ReduceLROnPlateau on validation tumour Dice, mode max, factor 0.5, patience 10, absolute threshold 0.001, cooldown 0 and minimum learning rate 0.000001 |

The early-stopping run stopped at epoch 32; its best checkpoint was selected at epoch 12. The scheduler reduced the learning rate ten times and reached 0.000001.

## Scope and limitations

All verified experiments used one recorded seed. The comparison is descriptive and does not quantify variability across repeated runs. It must not be described as statistically significant or as demonstrating general superiority.

No authoritative result directory was available for a 200-epoch combined horizontal-and-vertical-flip run or a Dice-only-loss run. Neither is included in the verified table.

