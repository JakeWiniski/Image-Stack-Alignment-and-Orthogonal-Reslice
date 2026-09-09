# Image Stack Alignment and Orthogonal Reslice

A compact, self-contained [SimpleITK](https://simpleitk.org/) workflow for turning a
folder of unaligned 2D slices (serial sections, a scan series, etc.) into a coherent,
aligned 3D stack — the basis for orthogonal reslicing and downstream analysis.

The full walkthrough lives in
[`Slice Stack Alignment, Reslice, and Analysis.ipynb`](Slice%20Stack%20Alignment%2C%20Reslice%2C%20and%20Analysis.ipynb).

## Overview

The notebook covers three steps:

1. **Load** — read an ordered stack of slice images from a folder and convert them to
   single-channel, floating-point images suitable for registration.
2. **Align** — register each slice to the first slice using 2D rigid-body (Euler)
   registration with a multi-resolution, coarse-to-fine strategy, so features stay in
   correspondence from slice to slice.
3. **Reconstruct & visualize** — stack the aligned slices into a volume and render them
   as textured planes in 3D, providing the starting point for orthogonal reslicing
   (XZ / YZ views) and quantitative analysis.

## Requirements

- Python 3.10+
- [SimpleITK](https://simpleitk.org/)
- NumPy
- Matplotlib

```bash
pip install SimpleITK numpy matplotlib
```

## Usage

1. Place your slice images in a single folder, named so they sort in acquisition order
   (zero-padded, sequential names work best — e.g. `slice_001.png`, `slice_002.png`, …).
   Supported formats: PNG, TIFF, and JPEG.
2. Open the notebook:

   ```bash
   jupyter notebook "Slice Stack Alignment, Reslice, and Analysis.ipynb"
   ```

3. Set the `folder` path in the "Run the alignment" cell to point at your images, then
   run the cells top to bottom.
4. Adjust the physical geometry (`fov_x_mm`, `fov_y_mm`, `slice_thickness_mm`) in the 3D
   visualization step to match your acquisition — the defaults are placeholders for the
   example data.

## Notes

- Slices are registered to the **first** image, which serves as the fixed reference and
  is left unchanged; all other slices are resampled onto its grid so they share a common
  coordinate system.
- Registration uses a mean-squares similarity metric, which assumes the slices share the
  same intensity characteristics. For multi-modal or strongly varying intensities, a
  metric such as Mattes mutual information may perform better.
