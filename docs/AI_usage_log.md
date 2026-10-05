# AI Usage Log

The major times I used AI while planning and building this project, what I learned, and how I used it.

---

### 1. Downloading the datasets into Colab

- **Date and tool:** Sep 22, 2026 · Claude
- **What I asked:** Help downloading CribHD into Colab and checking the files.
- **What the AI suggested:** Cells to download and unzip it. It guessed two folder paths wrong: a `CribHD/CribHD/` folder that doesn't exist, and `yolov8` instead of `YoloV8`.
- **What I learned:** I already knew to check folders with `ls` first, but I'd skip it sometimes and run into problems. Now I make it a habit before writing any paths.
- **How I applied it:** Every path in my notebook uses the real layout, `/content/CribHD/` and `YoloV8`.

---

### 2. Converting polygon labels to bounding boxes, line by line

- **Date and tool:** Sep 22, 2026 · Claude
- **What I asked:** Why CribHD's label lines had dozens of numbers instead of five, and how to use them with my detector.
- **What the AI suggested:** They're segmentation polygons, meaning outlines of each object. It wrote a function to turn each outline into a box using the smallest and largest x and y values.
- **What I learned:** A YOLO box label is always five values: class, center x, center y, width, and height. Two datasets that both number their classes starting at 0 can't be merged as they are.
- **How I applied it:** I had it explain the code line by line, then ran it. I also suggested adding 2 to CribHD-B's class number so blankets wouldn't get mixed up with hard-toys.

---

### 3. Catching lost labels in the merge

- **Date and tool:** Sep 23, 2026 · Claude
- **What I asked:** I was merging the toy and blanket labels into one folder so a single hazard detector could train on all three classes. I asked where the filename 'train0.txt' in its sample output came from.
- **What the AI suggested:** It admitted both subsets had a file with that name, so CribHD-B's files replaced CribHD-T's during the merge. It suggested adding `T_` or `B_` to each filename and counting the files in the folder after merging.
- **What I learned:** A merge can lose data without any error. The code counted the files it read, not the files that ended up in the folder, so the numbers looked right. After merging, check what's actually in the folder.
- **How I applied it:** I added the prefixes and the new check. That recovered 499 labels, and training boxes went from 1,760 to 2,783.

---

### 4. Picking the risks for slide 8

- **Date and tool:** Sep 23, 2026 · Claude
- **What I asked:** Brainstorming a Plan B for the risk that a model trained on studio stock photos wouldn't work as well on real nursery footage.
- **What the AI suggested:** Data augmentation as the Plan B, meaning training on darker, blurred, and rotated copies of the images.
- **What I learned:** A Plan B is only what you do if the risk actually happens. I was already planning to use augmentation, so listing it as a Plan B made it look like I wasn't going to do it otherwise.
- **How I applied it:** I pointed out that contradiction and brought up McManus's feedback about a child partly covered by a blanket. I'd already noticed that risk, so her spotting it too settled it. I made occlusion risk 1, with Plan B being to report "not visible" instead of "left the crib."

---

### 5. The training code, line by line

- **Date and tool:** Oct 2, 2026 · Claude
- **What I asked:** I asked the AI to help me figure out how to tell the `model.train()` line to train on my data, which is the cell in notebook 02 that trains the Child and Crib detector. Then I had it explain it and then quiz me on it.
- **What the AI suggested:** It explained each line in the training cell, which made it easier to connect to the fine-tuning steps back in Module 5. It then asks me questions, and I get the meaning of `nc` wrong. I thought it was the number of objects or boxes in one image; I had the idea that it counts something about objects.
- **What I learned:** `nc` doesn't count how many objects are in an image, but how many **types** of objects the detector knows. `nc` is the number of classes, and it makes the model replace its last layer of 80 COCO classes with a layer for my classes. `best.pt` contains the weights from the epoch with the best validation score, and `last.pt` contains the weights from the last epoch.
- **How I applied it:** I use the same training cell for each of my detectors, and I load `best.pt` in the alert notebooks. The training output confirms the change with the line "Overriding model.yaml nc=80 with nc=1" for the Crib detector and the Child detector.

---

### 6. Two sets of tags for CribHD-C

- **Date and tool:** Oct 4, 2026 · Claude
- **What I asked:** CribHD-C has no labels, so I tag each of the 120 images with the correct alert. I tell the AI to tag the images too so we can compare.
- **What the AI suggested:** It tags all 120 images and shows me its tags only after I finish so that they do not change mine. It names three types of images where it's not sure, and I decide them alone.
- **What I learned:** This is a common practice in data annotation work. Annotators tag the same items with the same guideline and then compare how much they agree, which is close to a majority vote. The guideline is clear when they agree.
- **How I applied it:** The two sets agree on all 120 images: 12 all clear and 108 hazard present. I use my tags as the ground truth in notebook 08. I ask the AI to confirm my rule for three images before I finish, and we find that our agreement appropriately reflects the rules I set in place.

---

### 7. The CribHD-C measurement, line by line

- **Date and tool:** Oct 5, 2026 · Claude
- **What I asked:** I asked the AI to explain notebook 08 line by line because it tests the risk that McManus names in her feedback: a child partly below a blanket.
- **What the AI suggested:** I have it quiz me until I can answer the questions myself. It corrects me on three points: the tags connect to the images by position, `child_found` counts images and not boxes, and 44 images have no crib box.
- **What I learned:** I learned how the code measures the old system and the new system. More importantly, I found the weakest part of my project: three of my four alerts need a crib box. My Blueprint never considered the risk that the system doesn't find the crib. I saw a weak crib score early in the build, but I didn't see its real cost until this notebook; ironically, it became my biggest risk and an even greater lesson.
- **How I applied it:** I keep the rule for the Final, and I report its cost. The system finds the child in 104 of 120 images and still gives "not visible" for 44 of them because it finds no crib. My next step is a system that can judge the safety of the child when it finds no crib.
