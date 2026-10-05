# Data

I use six sources: four for training and two only for testing. The notebooks download each dataset from the links below, and this repository contains no dataset files. The six example images in the results folder come from Baby in Crib and Baby object detection final under CC BY 4.0.

| Source | Link | Size | Labels | License |
|---|---|---|---|---|
| Baby in Crib | https://universe.roboflow.com/test-1caua/baby-in-crib | 242 images, split 169 train / 49 validation / 24 test | `Child`, `Crib` | CC BY 4.0 |
| Crib_detection | https://universe.roboflow.com/internship-cqxlp/crib_detection-xgv9n | 1,494 images | `crib` as outlines | CC BY 4.0 |
| BASE crib only BABY only | https://universe.roboflow.com/first-workspace-9obfx/base-crib-only-baby-only-7ytse | 553 files, 309 original images | `baby` | CC BY 4.0 |
| Baby object detection final | https://universe.roboflow.com/vtar/baby-object-detection-final | 7,853 files | `baby` | CC BY 4.0 |
| CribHD-T and CribHD-B (Northeastern) | https://github.com/ostadabbas/CribNet | 1,369 training images (920 toys, 449 blankets) | `hard-toy`, `soft-toy`, `blanket` | Non-commercial |
| CribHD-C (Northeastern) | same as above | 120 images | None | Non-commercial |

## Baby in Crib

These are studio stock photos with bright light and clean backgrounds. The Crib detector and the Child detector train on the training and validation folders, and the 24 test images stay separate. 18 labels are wrong because they put a crib box on a baby with no crib. I remove those labels before I train the Crib detector.

## Crib_detection

These are photos of cribs, and most show the crib from the side. The labels are outlines, and I change each outline into a box. 101 original images have a copy in the other folder, and I move those copies to training.

## BASE crib only BABY only

These are real crib camera frames and some photos of empty cribs. The 553 files include copies, and I keep one file for each of the 309 original images.

## Baby object detection final

These are frames from videos of children at home, and the boxes are on the full child. My 10 "left the crib" test images come from this dataset. I removed five videos from the training data because they show or can show my test rooms. I keep one file for each original frame, and that gives 3,136 frames for training.

## CribHD

The CribHD labels are outlines, and I change them to boxes. T and B each start their class numbers at 0, and I change blanket to class 2 when I merge them. CribHD-C shows a doll on a mattress with blankets and toys, and the system does not train on it. I tag each of the 120 images before I run the system: 12 are all clear, and 108 are hazard present.

## Ethics

I'm not collecting any footage of real children for this project. Each image comes from a public dataset, and I use it under its license. CribHD uses dolls and not real infants.
