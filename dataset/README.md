# GenRF scene dataset

GenRF provides 121,363 measured urban radio-field scenes for cross-scene learning and evaluation. The main corpus contains China Mobile 5G NR n41 RSRP measurements from Shenzhen, China, organized into scenes at three spatial scales.

| Scene size | Scenes | Array shape | Sliding step |
| --- | ---: | --- | --- |
| 100 m × 100 m | 91,902 | 100 × 100 | 50 m |
| 300 m × 300 m | 20,755 | 300 × 300 | 150 m |
| 500 m × 500 m | 8,706 | 500 × 500 | 250 m |
| Total | 121,363 | | |

GenRF organizes vehicular radio measurements into a multi-scale scene corpus and prepares sparse observation–target pairs for learning across scenes.

## Download

Download the three scene archives from the [dataset release](https://github.com/genrf-project/genrf-project.github.io/releases/tag/dataset-v1.0). Each archive contains the original PNG files for one spatial scale and a `manifest.csv`. The release also includes `metadata.json`, `SHA256SUMS`, this guide, and the dataset license.

## File format

Scenes are stored as 8-bit single-channel grayscale PNGs, at approximately one meter per pixel. These are numerical grids, not colorized figures. No resizing, colorization, or re-encoding is applied in this release.

A pixel intensity of zero is treated as empty. Positive intensities encode RSRP with

```text
RSRP_dBm = -144 + intensity / 255 × 104
```

The original export averages measurements in each occupied cell, clips RSRP to [−144, −40] dBm, normalizes it, and converts it to `uint8`. The quantization step is approximately 0.408 dB. PNG files do not store a separate validity mask; zero is also the encoding's lower endpoint. This release contains the processed, quantized representation; it does not contain the raw measurement logs.

Filenames retain the original form `grid_n41_300m_stephalf_x242_y79.png`. For side length `L`, a scene's local footprint is `[gx × L/2, gx × L/2 + L)` by `[gy × L/2, gy × L/2 + L)`. Columns run from west to east and rows from north to south. The indices locate scenes relative to the preprocessing origin; the archives contain no absolute-coordinate lookup table.

The scene-construction script retains scenes with more than 0.1% measured cells before intensity quantization. The manifests report the number of positive pixels in the released PNGs.

## Evaluation splits

The scene corpus does not include the paper's fixed train/test or evaluation-mask manifests. Neighboring scenes overlap because the sliding step is half the scene size; scenes at different scales also cover shared areas. Cross-scene evaluation should use geographically separated regions with non-overlapping footprints between splits.

## Manifest fields

Each `manifest.csv` records `scene_id`, `path`, `scale_m`, `gx`, `gy`, local footprint bounds in meters (`x_min_m`, `y_min_m`, `x_max_m`, `y_max_m`), `height`, `width`, `nonzero_cells`, and `sha256`. Paths are relative to the extracted archive root. The checksums verify the exact original PNG bytes.

## Attribution and terms

The released GenRF scene corpus is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). See `DATASET_LICENSE.txt` in the release for the full terms. The website template's license applies to website code only.

When using the dataset, credit the GenRF dataset contributors, link to the [dataset release](https://github.com/genrf-project/genrf-project.github.io/releases/tag/dataset-v1.0) and the CC BY 4.0 license, and indicate any modifications. Please acknowledge GenRF, *Reconstruct Any Radio Field in a Feed-Forward Process*.

## Citation

```bibtex
@inproceedings{genrf2027,
  title = {{GenRF}: Reconstruct Any Radio Field in a Feed-Forward Process},
  author = {{Anonymous Authors}},
  booktitle = {Proceedings of ACM SenSys},
  year = {2027}
}
```
