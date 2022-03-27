# DelFTa: Open-source Δ-quantum machine learning for medicinal chemistry
![](docs/delfta_schema.png)

[![delfta](https://github.com/josejimenezluna/delfta/actions/workflows/build.yml/badge.svg)](https://github.com/josejimenezluna/delfta/actions/workflows/build.yml)
![conda](https://anaconda.org/delfta/delfta/badges/installer/conda.svg)
[![Documentation Status](https://readthedocs.org/projects/delfta/badge/?version=latest)](https://delfta.readthedocs.io/en/latest/?badge=latest)
[![codecov](https://codecov.io/gh/josejimenezluna/delfta/branch/master/graph/badge.svg?token=kMkZiUi0DZ)](https://codecov.io/gh/josejimenezluna/delfta)
![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg)

## Overview 
The DelFTa application is an easy-to-use, open-source toolbox for predicting quantum-mechanical properties of drug-like molecules. Using either ∆-learning (with a GFN2-xTB baseline) or direct-learning (without a baseline), the application accurately approximates DFT reference values (*ω*B97X-D/def2-SVP). It employs 3D message-passing neural networks trained on the QMugs dataset of quantum-mechanical properties, and can predict formation and orbital energies, dipoles, Mulliken partial charges and Wiberg bond orders. See the [paper](https://chemrxiv.org/engage/chemrxiv/article-details/612faa02abeb63218cc5f6f1) for more details (version 1.0.0 used in this work).

## Installation

While the Linux (and Windows, through WSL) installations fully support GPU-acceleration via cudatoolkit, only CPU inference is currently available under Mac OS. We currently support Python 3.7 and 3.8 builds.

## Pre installation

On mac install cmake:
```bash
brew install cmake
```

Install eigen:
```bash
git clone https://gitlab.com/libeigen/eigen.git
cd eigen
mkdir build
cd build
cmake ..
make -j4
make install
```

Install SWIG:
```bash
brew install swig
```

Install openbabel with python binding:
```bash
git clone https://github.com/openbabel/openbabel.git
cd openbabel
mkdir build
cd build
cmake -DRUN_SWIG=ON -DPYTHON_BINDINGS=ON ..
make -j4
make test
sudo make install
```
add this line into your ~/.bashrc:
```bash
export PYTHONPATH=$PYTHONPATH:your/location/in/here/openbabel
source ~/.bashrc
```

Install openbabel with conda:
```bash
conda install -c openbabel openbabel
```

### Installation via conda

We recommend and support installation via the [conda](https://docs.conda.io/en/latest/miniconda.html) package manager, and that a fresh environment is created beforehand. Then fetch the package from our channel:

```bash
conda install delfta -c delfta -c pytorch -c rusty1s -c conda-forge
```


### Installation via Docker

A CUDA-enabled container can be pulled from [DockerHub](https://hub.docker.com/r/josejimenezluna/delfta). 

We also provide a Dockerfile for manual builds:

```bash
docker build -t delfta . 
```

Attach to the provided container with:

```bash
docker run -it delfta bash
```

## First run

DelFTa requires some additional files (_e.g._ trained models) before it can be used. Execute the following in order to fetch those:

```bash
python -c "import runpy; _ = runpy.run_module('delfta.download', run_name='__main__')"
```

## Quick start

We interface with Pybel (OpenBabel). Most molecular file formats are supported (_e.g._ .sdf, .xyz).

```python
from openbabel.pybel import readstring
mol = readstring("smi", "CCO")

from delfta.calculator import DelftaCalculator
calc = DelftaCalculator()
preds = calc.predict(mol)

print(preds)
```


Further documentation on how to use the package is available under [ReadTheDocs](https://delfta.readthedocs.io/en/latest/).

## Tutorials

In-depth tutorials can be found in the `tutorials` subfolder. These include: 

- [delta_vs_direct.ipynb](tutorials/delta_vs_direct.ipynb): This showcases the basics of how to run the calculator, and compares results using direct- and Δ-learning models. 
- [calculator_options.ipynb](tutorials/calculator_options.ipynb): This dives into the different options you can initialize the calculator class with. 
- [training.ipynb](tutorials/training.ipynb): A simple example of how networks can be trained. 


## Citation

If you use this software or parts thereof, please consider citing the following BibTex entry:

```
 @article{atz2021delfta, 
  title={Open-source Δ-quantum machine learning for medicinal chemistry}, 
  DOI={10.33774/chemrxiv-2021-fz6v7}, 
  journal={ChemRxiv}, 
  publisher={Cambridge Open Engage}, 
  author={Atz, Kenneth and Isert, Clemens and Böcker, Markus N. A. and Jiménez-Luna, José and Schneider, Gisbert}, 
  year={2021}
} 
```
