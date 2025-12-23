---
layout: dataset
title: "LookAlikes Dataset v2"
year: 2025
authors: "Adriano D'Alessandro et al."
task: "Fine-Grained Object Counting"
splash: "/images/lookalikes_hero.png"   # or `image:` if you prefer
download_url: "https://drive.google.com/drive/folders/1Dz3GRStKq-DL_eX1w7Eo0rscLSN7h5sG?usp=drive_link"
image: "/images/lookalikes_share.png"
description: "LookAlikes is a fine-grained object-counting dataset for evaluating the generalization of class-agnostic counting methods. Each image contains visually similar objects, from which only a subcategory must be counted. It ships with our paper FiGO: Fine-Grained Object Counting without Annotations"
download_label: "LOOKALIKES"
download_note: "(3.4 GB)"
---
<div style="background-color: #fff3cd; color: #856404; padding: 10px; border: 1px solid #ffeeba;">
  <strong>NOTE:</strong> The dataset link is currently offline as I upload the new v2 of the LookAlikes dataset. This is the dataset used to evaluate methods in our FiGO paper.
</div>

# Overview
**LOOKALIKES** is a fine-grained object-counting dataset that ships with our paper
[**FiGO: Fine-Grained Object Counting without Annotations**](https://arxiv.org/abs/2504.11705) — D’Alessandro, Mahdavi-Amiri, Hamarneh (2025).

What it’s for: stress-testing **text-specified, class-agnostic counting** models (i.e., zero-shot / prompt-shot / open-world, or whatever we're calling it these days). The dataset is **test-only** and deliberately tricky: most images contain **multiple visually similar sibling sub-categories** (e.g., jalapeño vs. habanero, penne vs. gnocchi), and the task is to **count only the instances of the requested sub-category** given a text prompt.

If you’ve got a pre-trained counter, this is a drop-in benchmark for measuring **generalization under fine-grained confusion**. Run your model with a sub-category prompt, report per-class counts, and let us know how it holds up.

(P.S. don't forget to cite our work!)


---

## 1) Download

The dataset can be directly downloaded from the following link:

{% include download_button.html %}


## 2) What’s inside

```
Data-LookAlikes/
├── <Broad-Category>/                         
│   ├── images/
│   │   ├── <fname>                 # JPG/PNG
│   │   └── ...
│   └── ...
├── annotations.all.testset         # all labels
├── usage_example.ipynb             # a demo for loading images
├── LICENSE.md
└── README.md
```

**Dataset stats**

* **Images:** 1,190
* **Domains:** peppers, apples, pasta, potatoes, tomatoes, waterfowl, wildflowers, etc.
* **Sub-categories:** 37
* **Annotation type:** point labels per instance (+ per-subcategory counts)
* **Avg density:** 36.4 objs/img (max 656)
* **Multi-class:** 83% of images contain ≥2 sibling sub-categories

---

## 3) Annotation schema

`annotations.all.testset.json`:

```json
{
  "<broad-category>": {
    "sub_categories": ["<sub-cat>", ...],
    "data": [
        {"image": {"fname": "<fname>", "width": "W", "height": "H"},
         "annotations": {"<sub-cat>": [{"point": [x, y]}, ...], ...}
        },
        ...
    ]
  }
}
```

---

## 4) Evaluation protocol

**Task:** predict the **count for each sub-category present** in an image.
**Primary metrics:** In our paper, we calculate MAE / RMSE per sub-category and macro-averaged across all.

There are helper functions in `usage_example.ipynb` that can be used for macro-averaging as follows:

```python

annotations = json.load(open("./annotations.all.testset.json"))
mae_across_splits = []
for broad_cat, sub_cat in  get_subcategory_keys(annotations):
    subcat_split = get_subcategory_split(annotations, broad_cat, sub_cat)
    per_example_subcat_mae = []
    for example in sub_cat_split:
        # calculate MAE
        mae = None # add your logic
        per_example_subcat_mae.append(mae)
    # calculate mae for sub_cat
    mae_across_splits(np.mean(per_example_subcat_mae)

print(np.mean(mae_across_splits))

```

---

## 5) License (summary)

* **Images:** original creators’ licenses (Flickr/Unsplash/Pexels/…); see metadata.
* **Annotations:** custom **research & individual use** license.

  * ✅ Academic & personal use
  * ❌ Corporate, commercial, governmental, policing, military, surveillance uses

Full text: see `LICENSE.md`.

---

## 6) Citation

```bibtex
@article{dalessandro2025justsaytheword,
  title   = {FiGO: Fine-Grained Object Counting without Annotations},
  author  = {D'Alessandro, Adrian and Mahdavi-Amiri, Ali and Hamarneh, Ghassan},
  journal = {arXiv preprint arXiv:2504.11705},
  year    = {2025}
}
```

