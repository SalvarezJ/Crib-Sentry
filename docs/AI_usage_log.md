# AI Usage Log

The major times I used AI while planning this project, what I learned, and how I used it.

---

### 1. Downloading the datasets into Colab

- **Date and tool:** Sep 22, 2026 · Claude
- **What I asked:** Help downloading CribHD into Colab and checking the files.
- **What the AI suggested:** Cells to download and unzip it. It guessed two folder paths wrong: a `CribHD/CribHD/` folder that doesn't exist, and `yolov8` instead of `YoloV8`.
- **What I learned:** I already knew to check folders with `ls` first, but I'd skip it sometimes and run into problems. Now I make it a habit before writing any paths.
- **How I applied it:** Every path in my notebook uses the real layout, `/content/CribHD/` and `YoloV8`.

---

### 2. Converting polygon labels to bounding boxes

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

