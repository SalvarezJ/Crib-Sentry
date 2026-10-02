# Crib Sentry

## Author
Seth Alvarez

## Project Tier
Tier 2: two detectors plus logic that turns what they find into alerts, instead of one model just reporting what it detects.

## Problem Statement
A traditional baby monitor only helps if someone is watching it. Parents can't watch continuously, so a lot of what the camera records goes unnoticed. The goal is to cut down on what gets missed.

## Solution Overview
The system takes one frame from a crib camera and runs two detectors. One finds the child and the crib; the other finds hazards like toys and blankets. Then it compares where the boxes are and returns one of four alerts: all clear, left the crib, not visible, or hazard present.

## Technical Approach
- CV technique: Object detection
- Model architecture: CNN
- Model: YOLO11
- How it will be used: Transfer learning, starting from weights pretrained on COCO
- Framework: PyTorch through Ultralytics
- Augmentation: Darker lighting, blur, and rotation during training so the model does better on real nursery footage
- Why: Every alert depends on where one box is compared to another, so bounding boxes are what I need. YOLO11 can be trained on a few hundred images on a free Colab GPU, and it can run on video later without changing the approach.

## Dataset

| Source | Size | Labels | License |
|---|---|---|---|
| [Baby in Crib](https://universe.roboflow.com/test-1caua/baby-in-crib) (Roboflow) | 242 images, split 169/49/24 | `Child`, `Crib`, already labeled | CC BY 4.0 |
| [CribHD](https://github.com/ostadabbas/CribNet) T and B (Northeastern) | 1,369 training images | `hard-toy`, `soft-toy`, `blanket`, converted from outlines to boxes | Non-commercial |
| CribHD-C | 120 images, unlabeled | Held out for testing | Non-commercial |

The Baby in Crib images are studio stock photos. CribHD-C looks more like real overhead camera footage, so I'm holding it out to see how much worse the system does on it. More details in [data/README.md](data/README.md).

**Known gap:** No dataset I'm using has a child outside a crib, which is half of the alert logic.

## Success Metrics
- Primary: How often the system's alert is correct on the 24 test images. I expect at least 85%.
- Secondary: How long the system takes per image. I expect under 1 second.

I care more about recall than precision. For a safety system, missing a real problem is worse than a false alarm.

## Milestone Plan

| Phase | Goal | Milestone | Week |
|---|---|---|---|
| Blueprint | Plan approved | Midterm submitted | 6 |
| First Working Demo | Pretrained model runs start to finish on a few sample images | Something works, even if rough | 7 |
| Make It Yours | Add data, training, and alert logic | System works on my problem | 7-8 |
| Improve and Measure | Test, fix, and measure against the metrics | Metrics recorded | 8 |
| Package and Present | Demo video, README, final slides | Final submitted | 9 |

## Resources
- Compute: Google Colab free tier with a T4 GPU. My PC's GTX 1660 as a backup.
- Cost: $0. Every model, dataset, and tool I'm using is free or open source.

## Risks and Mitigation

| Risk | Probability | Plan B |
|---|---|---|
| A child lying down or partly under a blanket might not be detected, since the training images only show children sitting or standing and fully visible | High | If the crib is detected but no child is found in it, report "not visible" instead of "left the crib." It's better to say it can't tell than to give a wrong alert. |
| No dataset has a child outside a crib, which is half of the alert logic | High | Find a small supplementary dataset of children outside cribs on Roboflow Universe. |

## Demo Video
Link goes here at the Final.

## AI Usage Log
See [docs/AI_usage_log.md](docs/AI_usage_log.md)

## Current Status
- [x] Repository created
- [x] Proposal submitted
- [x] First working demo
- [ ] System works on my data
- [ ] Metrics measured
- [ ] Final submitted
