# Paired SAR–Optical Dataset with Semantic Masks

A research dataset and reproducible workflow for exploring **SAR image colorization**, **SAR-to-optical image translation**, and **semantic segmentation** in remote sensing.

This repository provides documentation, schema files, and research code associated with a paired Synthetic Aperture Radar (SAR) and optical imagery dataset, together with semantic masks and colorized mask previews.

> **Important:** The semantic masks are model-generated pseudo-labels. They should not be treated as manually verified ground-truth annotations.

## Dataset

- **Dataset title:** Paired SAR–Optical Dataset with Semantic Masks
- **Repository:** [This GitHub repository](https://github.com/Swaraj1657/sar-optical-semantic-masks.git)
- **Dataset landing page:** [Figshare — SAR Optical Dataset with Semantic Masks](https://figshare.com/articles/dataset/SAR_Optical_Dataset_with_Semantic_Masks/34335066)
- **DOI:** [10.6084/m9.figshare.34335066](https://doi.org/10.6084/m9.figshare.34335066)
- **Upstream dataset:** [QXS-SAROPT](https://github.com/yaoxu008/QXS-SAROPT)

Please consult the dataset landing page for the released files, version information, and applicable terms of use.

## Contents

The dataset is organized into four main directories:

```text
data/
├── sar/           # SAR images
├── opt/           # Corresponding optical images
├── mask/          # Semantic mask images
└── colored_mask/  # Colorized visualizations of masks
```

The repository may also contain supporting materials:

```text
.
├── docs/                       # Dataset and methodology documentation
├── schemas/
│   └── dataset-schema.json     # Dataset structure/schema
├── notebooks/                  # Research notebooks
├── scripts/                    # Utility and validation scripts
├── CITATION.cff                # Citation metadata
├── LICENSE                     # Repository code/documentation license
├── LICENSE-DATA.md             # Dataset rights and attribution notes
└── README.md
```

The directory tree above describes the intended organization; check the files actually included in a particular release.

## Intended research uses

- SAR image colorization and visualization
- SAR-to-optical image translation
- Paired multimodal remote-sensing experiments
- Semantic segmentation and pseudo-label evaluation
- Weakly supervised learning
- Semantic consistency evaluation for image translation

This dataset is intended for research and experimentation. Results may depend on the data split, preprocessing, model configuration, and quality of the pseudo-labels.

## Semantic masks

The mask-generation workflow uses **SegEarth-OV** to produce candidate semantic labels from optical imagery. The associated workflow uses CLIP ViT-B/16 features and SimFeatUp feature upsampling, with a threshold of `0.1` and a target mask size of `256 × 256`.

These settings describe the associated workflow and should be checked against the exact code and configuration used for each dataset release.

### Proposed class mapping

The following zero-based class mapping is documented for this dataset. **Verify it against the released mask PNG values and the accompanying schema before training a model.**

| Class ID | Class name |
|---:|---|
| 0 | Background / unassigned |
| 1 | Water |
| 2 | Ship |
| 3 | Building |
| 4 | Road |
| 5 | Vegetation |
| 6 | Bare soil |
| 7 | Parking lot |
| 8 | Dock |

Mask values, class definitions, and any ignore-label convention should be confirmed from the actual released files. Do not assume that a displayed color corresponds to a particular class ID without checking the mapping.

## Data format

The associated workflow describes semantic masks as single-channel, 8-bit PNG images at `256 × 256` resolution. Confirm the dimensions, encoding, and alignment of each file in the released version before using it.

For paired-image tasks, verify that SAR images, optical images, and masks correspond to the same sample and are spatially aligned. Use nearest-neighbor interpolation when resizing categorical masks so that class IDs are not blended.

## Getting the dataset

Download the dataset from the official Figshare landing page:

**[Download / access the dataset on Figshare](https://figshare.com/articles/dataset/SAR_Optical_Dataset_with_Semantic_Masks/34335066)**

After downloading, extract the archive and place the data in the structure described above, adjusting paths as needed for your project.

The full dataset archive is several gigabytes, so it is generally better to host the data on Figshare rather than commit the entire archive to GitHub. Use this repository for documentation, scripts, schemas, and reproducible research code.

## Basic loading example

The following example loads a mask as integer class IDs using Pillow:

```python
from pathlib import Path
import numpy as np
from PIL import Image

mask_path = Path("data/mask/example.png")

mask = np.array(Image.open(mask_path))

print("Shape:", mask.shape)
print("Data type:", mask.dtype)
print("Unique values:", np.unique(mask))
```

Replace `example.png` with a real filename from your downloaded dataset. Check the output to confirm whether masks are single-channel and which class IDs are present.

## Recommended validation

Before training or publishing results, check:

1. **File pairing:** each SAR image matches the intended optical image and mask.
2. **Dimensions:** image and mask sizes meet the requirements of the experiment.
3. **Mask encoding:** masks contain the expected integer IDs and use the documented ignore-label convention, if any.
4. **Data integrity:** files can be opened and are not truncated or corrupted.
5. **Data splits:** training, validation, and test sets avoid unintended leakage between paired samples or related scenes.
6. **Pseudo-label quality:** manually inspect representative masks and report their limitations.
7. **Provenance and rights:** confirm the upstream dataset's license and redistribution terms before redistributing its imagery or derivatives.

## Limitations

- Semantic masks are automatically generated pseudo-labels and may contain incorrect, incomplete, or uncertain predictions.
- Model-generated labels can inherit biases and limitations from the underlying model, prompts, input imagery, and preprocessing.
- Class coverage and mask quality may vary across scenes and imaging conditions.
- A paired SAR–optical dataset does not guarantee perfect spatial or temporal correspondence for every sample.
- The dataset should not be described as ground-truth annotated unless independent validation supports that claim.

## Data provenance and attribution

This dataset is associated with the upstream [QXS-SAROPT repository](https://github.com/yaoxu008/QXS-SAROPT). Segmentation masks are associated with the [SegEarth-OV project](https://github.com/earth-insights/SegEarth-OV).

Before redistributing upstream images, masks, or derived files, review the upstream dataset and software licenses and comply with their attribution and use requirements. A license for this GitHub repository does **not** automatically grant rights to third-party imagery or models.

## Citation

If you use this dataset, cite the Figshare record:

```text
Paired SAR–Optical Dataset with Semantic Masks.
Figshare. https://doi.org/10.6084/m9.figshare.34335066
```

For the definitive citation, author list, publication date, and version, use the metadata shown on the Figshare landing page.

Please also cite the upstream dataset and any methods or software used in your work, including SegEarth-OV where applicable.

## License

- **Repository code and documentation:** see [`LICENSE`](LICENSE).
- **Dataset and third-party imagery:** see [`LICENSE-DATA.md`](LICENSE-DATA.md) and the upstream dataset's terms.

Do not select a license for the data until you have confirmed that you have the rights to distribute the original imagery and derived masks.

## Acknowledgements

We acknowledge the creators and maintainers of the QXS-SAROPT dataset and the developers of SegEarth-OV and related open-source tools used in the mask-generation workflow.

---

For questions, corrections, or reproducibility issues, please open a GitHub issue with the relevant file names, configuration, and steps to reproduce the problem.
