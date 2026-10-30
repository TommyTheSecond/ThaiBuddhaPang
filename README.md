# ThaiBuddhaPang

An image dataset for classifying Buddha images by **pang** (ปาง), the posture or
representation of the Buddha.

This is the dataset released with the ICCT 2026 paper *ThaiBuddhaPang: An Image
Database and Benchmark for Classifying Buddha Images by Pang*.

| | |
|---|---|
| Images | 826 photographs from Wikimedia Commons |
| Classes | 5 pangs |
| Statue instances | 77 |
| Annotations | pang label, statue bounding box, statue instance, cross-validation fold |
| Licences | all redistributable: CC BY-SA, CC BY, CC0, public domain, Free Art License. No NonCommercial or NoDerivatives material |

## Classes

| `pang` | Thai | Pose | Images | Statue instances |
|---|---|---|---:|---:|
| `marawichai` | มารวิชัย | Earth-touching: right hand over the knee, fingers pointing down | 382 | 46 |
| `saiyat` | ไสยาสน์ | Reclining: lying on the right side | 146 | 11 |
| `nakprok` | นาคปรก | Naga-sheltered: seated under the hood of the naga | 121 | 11 |
| `samathi` | สมาธิ | Meditation: both hands resting flat in the lap | 98 | 7 |
| `prathanphon` | ประทานพร | Boon-giving: one palm turned outward | 79 | 8 |
| **Total** | | | **826** | **77** |

The per-class instance counts add up to 83, not 77, because five statue instances
appear in more than one pang.

## Statue instances

A **statue instance** is one physical statue, together with its replicas and close
variants, and all of its photographs. Photographs of the same statue look very
similar, so a random train/test split lets a model recognise the statue instead of
the pose. To get an honest score, keep all images of one `instance_id` on the same
side of any split. The `fold` column gives the five folds used in the paper.

## Files

```
images/            826 JPEG images, named by image id (e.g. 00684.jpg)
annotations.csv    one row per image
```

`annotations.csv` columns:

| Column | Meaning |
|---|---|
| `image` | path to the image in this repository |
| `pang` | class label (see table above) |
| `instance_id` | statue instance, 0 to 76 |
| `instance_name` | readable name of the statue instance |
| `fold` | cross-validation fold used in the paper, 0 to 4 |
| `box_x`, `box_y`, `box_w`, `box_h` | statue bounding box in pixels: top-left corner, width, height |
| `width`, `height` | size of the image file in this repository, in pixels |
| `commons_file` | file name on Wikimedia Commons |
| `commons_page` | the file's page on Wikimedia Commons |
| `author` | photographer as recorded on Commons (empty for 11 images) |
| `license` | the image's licence |
| `sha1_original` | SHA-1 of the original file on Commons |

The bounding boxes are in the pixel space of the image files in this repository.
Some files are reduced-size versions of the original Commons upload. No other
changes were made to the images.

Loading in Python:

```python
import pandas as pd
from PIL import Image

df = pd.read_csv("annotations.csv")
row = df.iloc[0]
img = Image.open(row["image"])
crop = img.crop((row.box_x, row.box_y, row.box_x + row.box_w, row.box_y + row.box_h))
```

## Licence

**Images.** Each image keeps the licence it has on Wikimedia Commons, given in the
`license` column with the author and a link to its Commons page. If you reuse an
image, follow its licence: credit the author, link the licence, and for CC BY-SA
images share adaptations under the same licence.

**Annotations** (labels, bounding boxes, statue instances and folds) are released
under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## Citation

> Nattawat Duangsuwong, Jirathana Yosyingthummakul, Waree Kongprawechnon, and Jessada
> Karnjana. "ThaiBuddhaPang: An Image Database and Benchmark for Classifying Buddha
> Images by Pang." ICCT 2026.
