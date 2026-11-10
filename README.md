# A New Hybrid Model for Secure Image Steganography Using Fusion

Code accompanying the paper:

> Yasmina Ayman Kamal, Hoda Osama Elkhodary, Sherin M. Youssef.
> **A New Hybrid Model for Secure Image Steganography Using Fusion.**
> *Journal of Advanced Research in Applied Sciences and Engineering Technology*, 62(3), 31–40, 2024.
> DOI: [10.37934/araset.62.3.3140](https://doi.org/10.37934/araset.62.3.3140) ·
> [Article page](https://semarakilmu.com.my/journals/index.php/applied_sciences_eng_tech/article/view/6397)

## Overview

A secret (payload) image is hidden inside a cover image. The pipeline:

1. **Normalization.** Cover and payload are scaled to `[0, 1]`.
2. **Pre-processing.** The cover is weighted by `α`, the payload by `β = 1 − α` (`α = 0.999` by default).
3. **Discrete Wavelet Transform (DWT).** Both images are split into LL, LH, HL and HH sub-bands.
4. **Encryption.** The payload sub-bands are scrambled with a chaotic map (Arnold's Cat Map in the paper; the notebook also has Hénon-map and logistic-map variants).
5. **Fusion.** Each encrypted payload sub-band is added to the matching cover sub-band (alpha blending).
6. **Inverse DWT (IDWT).** The fused sub-bands are recombined to produce the stego image.
7. **Evaluation.** MSE and PSNR are computed between the stego image and the cover image.

Extraction reverses these steps: DWT of the stego image, subtraction of the cover sub-bands, decryption, IDWT, and division by `β`.

## Repository structure

```
.
├── notebooks/
│   └── Steganography.ipynb   # full pipeline: embedding, extraction, experiments
├── images/                   # put the test images here (see below)
├── requirements.txt
├── CITATION.cff
├── LICENSE
└── README.md
```

## Getting started

### Option A: Google Colab
1. Open `notebooks/Steganography.ipynb` in Colab.
2. Upload `cameraman.tif` (payload, 512×512) and `lena_gray_256.tif` (cover, 256×256) to your Google Drive under `Colab Notebooks/`.
3. Run the cells from top to bottom.

### Option B: Locally
```bash
git clone https://github.com/<your-username>/hybrid-image-steganography-fusion.git
cd hybrid-image-steganography-fusion
pip install -r requirements.txt
jupyter notebook notebooks/Steganography.ipynb
```
When running locally, skip the `google.colab` drive-mount cell, put the test images in `images/`, and change the image paths, for example:
```python
PI = cv2.imread("../images/cameraman.tif")
CI = cv2.imread("../images/lena_gray_256.tif")
```

## Test images

The standard grayscale test images used are **Cameraman** (payload) and **Lena** (cover). They are not included in this repository. You can get them from the [USC-SIPI Image Database](https://sipi.usc.edu/database/) or other standard image-processing test sets.

## Results

The paper reports PSNR values of about 70.8 dB and SSIM up to about 0.99 (Daubechies 3). See the paper for full results across wavelet families.

## Notes

The notebook is the original research notebook. Besides the main pipeline, it contains exploratory sections ("Trying Arnold Cat Encryption on a separate image", "Trying New Encryption Technique") that compare other chaotic-map encryption schemes. Some of those cells depend on helper functions defined elsewhere, or are works in progress.

## Citation

If you use this code, please cite the paper:

```bibtex
@article{kamal2024hybrid,
  title   = {A New Hybrid Model for Secure Image Steganography Using Fusion},
  author  = {Kamal, Yasmina Ayman and Elkhodary, Hoda Osama and Youssef, Sherin M.},
  journal = {Journal of Advanced Research in Applied Sciences and Engineering Technology},
  volume  = {62},
  number  = {3},
  pages   = {31--40},
  year    = {2024},
  doi     = {10.37934/araset.62.3.3140}
}
```

## License

Released under the MIT License. See [LICENSE](LICENSE).
