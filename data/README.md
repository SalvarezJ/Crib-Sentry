# Data

There are a total of three sources. Two are used for training; one is held out for testing. No data is stored in this repo. Everything is downloaded from the links below when the notebook runs.

| Source | Link | Size | Labels | License |
|---|---|---|---|---|
| Baby in Crib (Roboflow) | https://universe.roboflow.com/test-1caua/baby-in-crib | 242 images, split 169 train / 49 validation / 24 test | `Child`, `Crib` | CC BY 4.0 |
| CribHD-T and CribHD-B (Northeastern) | https://github.com/ostadabbas/CribNet | 1,369 training images (920 toys, 449 blankets) | `hard-toy`, `soft-toy`, `blanket` | Non-commercial |
| CribHD-C (Northeastern) | same as above | 120 images | None, held out for testing | Non-commercial |

## Baby in Crib

These are studio stock photos with bright lighting and clean backgrounds, so they don't look like real nursery camera footage. Around 100 of the 242 images have a child in them. The rest are empty cribs, which I can also use as examples for the "not visible" condition.

## CribHD

CribHD's labels come as outlines of each object (segmentation polygons). I converted them to bounding boxes so they match the child and crib detector. T and B both number their classes starting at 0, so I changed blanket to class 2 when merging them. That way blankets don't get mixed up with hard-toys.

CribHD-C is the only set that looks like a real overhead crib camera. It shows an infant CPR manikin on a mattress with blankets and toys around it. I'm not training on it. I'm using it to test how the system does on realistic images.

**Known gap:** None of these sources have a child outside a crib. I'll have to find a small supplementary dataset of that on Roboflow Universe.

**Ethics:** CribHD uses a manikin instead of real infants, and Baby in Crib is public stock photography. I'm not collecting any footage of real children for this project.
