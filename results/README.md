# Results

I measure two systems on the same images and the same tags.
- The old system uses one detector for the child and the crib (notebook 02) and the hazard detector (notebook 03).
- The new system uses a Crib detector (notebook 05), a Child detector (notebook 06) and the same hazard detector.

## Alert accuracy

| Test images | Old system | New system | Target |
|---|---|---|---|
| 24 Baby in Crib test images | 18 correct (75.0%) | 17 correct (70.8%) | 85% or more |
| 10 "left the crib" images | 0 correct | 7 correct | no target |
| 120 CribHD-C images | 20 correct (16.7%) | 50 correct (41.7%) | no target |

## Time for each image on the T4 GPU

| Test images | New system | Target |
|---|---|---|
| 24 Baby in Crib test images | 0.034 seconds | less than 1 second |
| 120 CribHD-C images | 0.325 seconds | less than 1 second |

## Detector scores on the validation images

| Detector | Notebook | Images | Precision | Recall | mAP50 | mAP50-95 |
|---|---|---|---|---|---|---|
| Child and Crib (old) | 02 | 49 | 0.898 | 0.910 | 0.969 | 0.671 |
| Hazards | 03 | 90 | 0.765 | 0.715 | 0.794 | 0.589 |
| Crib (new) | 05 | 177 | 0.944 | 0.850 | 0.908 | 0.774 |
| Child (new) | 06 | 336 | 0.948 | 0.912 | 0.976 | 0.717 |

## Errors that the new system still makes

- The Crib detector puts a crib box with low confidence on three photos that show a baby with no crib.
- The Child detector does not find one baby, and it puts a child box on printed words one time.
- The hazard detector reads two fitted sheets as blankets.
- In a side view, the box of a child that climbs out is still on the crib box. This makes my "inside" rule fail three times.
- CribHD-C shows a mattress with no bars. The Crib detector finds a crib in only 76 of 120 images.

Notebooks 04, 07 and 08 show each measurement.
