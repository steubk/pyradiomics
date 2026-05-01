> ⚠️ **Unofficial community fork — Python 3.12 compatibility stop-gap.**
>
> This is a temporary fork of [AIM-Harvard/pyradiomics](https://github.com/AIM-Harvard/pyradiomics)
> that ships five Python 3.12 / SciPy 1.15+ / NumPy 2 compatibility fixes which are
> **not yet in any upstream release**. **Use the official `pyradiomics` package whenever it
> works for you.** .
>
> Maintained by [steubk](https://github.com/steubk) — issues:
> https://github.com/steubk/pyradiomics/issues — refs upstream issue
> [AIM-Harvard/pyradiomics#949](https://github.com/AIM-Harvard/pyradiomics/issues/949)

## Why this fork exists

The current upstream release of pyradiomics (3.1.0 on PyPI, 2025-04) does not install or run
on Python 3.12 with current SciPy/NumPy. This fork applies five targeted fixes:

1. Adds `ruamel.yaml` as an explicit runtime dependency (used by the CLI but undeclared upstream).
1. Replaces the deprecated `scipy.ndimage.interpolation` import with `scipy.ndimage`.
1. Adds a `sph_harm` / `sph_harm_y` compatibility wrapper for SciPy ≥ 1.15.
1. Raises `requires-python` to `>=3.10` (consistent with NumPy 2.1).
1. Adds an `[lbp3d]` optional extra (`scipy`/`trimesh`) for the LBP-3D filter.

See `CHANGES.rst` (`Next Release`) for the full diff summary.

## How to install

Prebuilt wheels for Linux / Windows / macOS, CPython 3.10–3.13, are published as a GitHub
Release on this fork (`prebuilt-fix-py312`). Pip installs them via `--find-links` while
fetching all runtime dependencies from PyPI as usual. The distribution name remains
`pyradiomics` (same as upstream), so `import radiomics` works unchanged.

### CLI

```bash
pip install \
  --find-links https://github.com/steubk/pyradiomics/releases/expanded_assets/prebuilt-fix-py312 \
  pyradiomics
```

### `requirements.txt`

```
--find-links https://github.com/steubk/pyradiomics/releases/expanded_assets/prebuilt-fix-py312
pyradiomics
```

### Google Colab

```sh
!pip install --find-links https://github.com/steubk/pyradiomics/releases/expanded_assets/prebuilt-fix-py312 pyradiomics
```

The wheels are versioned `3.1.0+py312fix.<sha>` (PEP 440 local segment over latest upstream
stable). Pip prefers them over plain `3.1.0`, so no `--pre` flag is needed; PyPI rejects
uploads with `+local`, so these wheels can never be confused with an official PyPI release.

## When upstream releases the fix

1. The `prebuilt-fix-py312` GitHub Release will be deleted.
1. Branch `prebuilt/py312` will be removed.
1. Update your dependency to `pyradiomics>=A.B.C` (the upstream version with the fixes) and
   drop the `--find-links` line.

______________________________________________________________________

# pyradiomics v3.1.0

\<-- ## Build Status

| Linux / MacOS | Windows |
| ------------- | ------- |
|               |         |
| -->           |         |

## Radiomics feature extraction in Python

This is an open-source python package for the extraction of Radiomics features
from medical imaging.

With this package we aim to establish a reference standard for Radiomic
Analysis, and provide a tested and maintained open-source platform for easy and
reproducible Radiomic Feature extraction. By doing so, we hope to increase
awareness of radiomic capabilities and expand the community.

The platform supports both the feature extraction in 2D and 3D and can be used
to calculate single values per feature for a region of interest
("segment-based") or to generate feature maps ("voxel-based").

**Not intended for clinical use.**

**If you publish any work which uses this package, please cite the following
publication:** _van Griethuysen, J. J. M., Fedorov, A., Parmar, C., Hosny, A.,
Aucoin, N., Narayan, V., Beets-Tan, R. G. H., Fillion-Robin, J. C., Pieper, S.,
Aerts, H. J. W. L. (2017). Computational Radiomics System to Decode the
Radiographic Phenotype. Cancer Research, 77(21), e104–e107.
https://doi.org/10.1158/0008-5472.CAN-17-0339_

### Join the Community!

Please join the
[Radiomics community section of the 3D Slicer Discourse](https://discourse.slicer.org/c/community/radiomics/23).

### Feature Classes

Currently supports the following feature classes:

- First Order Statistics
- Shape-based (2D and 3D)
- Gray Level Co-occurrence Matrix (GLCM)
- Gray Level Run Length Matrix (GLRLM)
- Gray Level Size Zone Matrix (GLSZM)
- Gray Level Dependence Matrix (GLDM)
- Neighboring Gray Tone Difference Matrix (NGTDM)

### Filter Classes

Aside from the feature classes, there are also some built-in optional filters:

- Laplacian of Gaussian (LoG, based on SimpleITK functionality)
- Wavelet (using the PyWavelets package)
- Square
- Square Root
- Logarithm
- Exponential
- Gradient (Magnitude)
- Local Binary Pattern (LBP) 2D / 3D

### Supporting reproducible extraction

Aside from calculating features, the pyradiomics package includes provenance
information in the output. This information contains information on used image
and mask, as well as applied settings and filters, thereby enabling fully
reproducible feature extraction.

### Documentation

For more information, see the sphinx generated documentation available
[here](http://pyradiomics.readthedocs.io/).

Alternatively, you can generate the documentation by checking out the master
branch and running from the root directory:

```
sphinx-build docs docs/_build/
```

The documentation can then be viewed in a browser by opening
`PACKAGE_ROOT\build\sphinx\html\index.html`.

Furthermore, an instruction video is available
[here](http://radiomics.io/pyradiomics.html).

### Installation

PyRadiomics is OS independent and compatible with Python >= 3.5. Pre-built
binaries are available on PyPi and Conda. To install PyRadiomics, ensure you
have python installed and run:

```
`python -m pip install pyradiomics`
```

Detailed installation instructions, as well as instructions for building
PyRadiomics from source, are available in the
[documentation](http://pyradiomics.readthedocs.io/en/latest/installation.html).

### Docker

PyRadiomics also supports [Dockers](https://www.docker.com/). Currently, 2
dockers are available:

The first one is a [Jupyter notebook](http://jupyter.org/) with PyRadiomics
pre-installed with example Notebooks.

To get the Docker:

```
docker pull radiomics/pyradiomics:latest
```

The `radiomics/notebook` Docker has an exposed volume (`/data`) that can be
mapped to the host system directory. For example, to mount the current
directory:

```
docker run --rm -it --publish 8888:8888 -v `pwd`:/data radiomics/notebook
```

or for a less secure notebook, skip the randomly generated token

```
docker run --rm -it --publish 8888:8888 -v `pwd`:/data radiomics/notebook start-notebook.sh --NotebookApp.token=''
```

and open the local webpage at http://localhost:8888/ with the current directory
at http://localhost:8888/tree/data.

The second is a docker which exposes the PyRadiomics CLI interface. To get the
CLI-Docker:

```
docker pull radiomics/pyradiomics:CLI
```

You can then use the PyRadiomics CLI as follows:

```
docker run radiomics/pyradiomics:CLI --help
```

For more information on using docker, see
[here](https://pyradiomics.readthedocs.io/en/latest/installation.html#use-pyradiomics-docker)

### Usage

PyRadiomics can be easily used in a Python script through the `featureextractor`
module. Furthermore, PyRadiomics provides a commandline script, `pyradiomics`,
for both single image extraction and batchprocessing. Finally, a convenient
front-end interface is provided as the 'Radiomics' extension for 3D Slicer,
available [here](https://github.com/AIM-Harvard/SlicerRadiomics).

### 3rd-party packages used in pyradiomics:

- SimpleITK (Image loading and preprocessing)
- numpy (Feature calculation)
- PyWavelets (Wavelet filter)
- pykwalify (Enabling yaml parameters file checking)
- scipy (Only for LBP filter, install separately to enable this filter)
- scikit-image (Only for LBP filter, install separately to enable this filter)
- trimesh (Only for LBP filter, install separately to enable this filter)

See also the requirements section of the [pyproject file](pyproject.toml).

### 3D Slicer

PyRadiomics is also available as an
[extension](https://github.com/AIM-Harvard/SlicerRadiomics) to
[3D Slicer](slicer.org). Download and install the 3D slicer
[nightly build](http://download.slicer.org/), the extension is then available in
the extension manager under "SlicerRadiomics".

### License

This package is covered by the open source [3-clause BSD License](LICENSE.txt).

### Developers

- [Joost van Griethuysen](https://github.com/JoostJM)<sup>1,3,4</sup>
- [Andriy Fedorov](https://github.com/fedorov)<sup>2</sup>
- [Nicole Aucoin](https://github.com/naucoin)<sup>2</sup>
- [Jean-Christophe Fillion-Robin](https://github.com/jcfr)<sup>5</sup>
- [Ahmed Hosny](https://github.com/ahmedhosny)<sup>1</sup>
- [Steve Pieper](https://github.com/pieper)<sup>6</sup>
- [Hugo Aerts (PI)](https://github.com/hugoaerts)<sup>1,2</sup>

<sup>1</sup>Department of Radiation Oncology, Dana-Farber Cancer Institute,
Brigham and Women's Hospital, Harvard Medical School, Boston, MA,
<sup>2</sup>Department of Radiology, Brigham and Women's Hospital, Harvard
Medical School, Boston, MA, <sup>3</sup>Department of Radiology, Netherlands
Cancer Institute, Amsterdam, The Netherlands, <sup>4</sup>GROW-School for
Oncology and Developmental Biology, Maastricht University Medical Center,
Maastricht, The Netherlands, <sup>5</sup>Kitware, <sup>6</sup>Isomics

### Contact

We are happy to help you with any questions. Please contact us on the
[Radiomics community section of the 3D Slicer Discourse](https://discourse.slicer.org/c/community/radiomics/23).

We welcome contributions to PyRadiomics. Please read the
[contributing guidelines](CONTRIBUTING.rst) on how to contribute to PyRadiomics.

**This work was supported in part by the US National Cancer Institute grants:
U24CA194354 - QUANTITATIVE RADIOMICS SYSTEM DECODING THE TUMOR PHENOTYPE and
U01CA190234 - TUMOR GENOTYPE AND RADIOMIC PHENOTYPE IN LUNG CANCER**
