# Scripts for "Best-practices in single-cell RNA-seq: a tutorial"

This repository contains the scripts used to generate the figures in the manuscript M. D. Luecken and F. J. Theis., "Best practices in single-cell RNA-seq analysis: a tutorial", a case study which complements the manuscript (Also in Supplementary Note 4), and a marker gene study using simulated data from the splatter package.

The main part of this repository is a case study where the best-practices established in the manuscript are applied to a mouse intestinal epithelium regions dataset from Haber et al., Nature 551 (2018) available from the GEO under GSE92332.

The scripts in the plotting_scripts/ folder reproduce the figures that are shown in the manuscript and the supplementary materials. These scripts contain comments to explain each step. Each figure that does not have a corresponding script in the plotting_scripts/ folder was taken from the case study or the marker gene study.

In case of questions or issues, please get in touch by posting an issue in this repository.


## Case study

To run the single cell tutorial from start to finish, the data must be downloaded and several requirements must be met.

### Download the data

As mentioned above the data for the case study comes from GSE92332. To run the case study as shown, you must download this data and place it in the correct folder. Unpacking the data requires `tar` and `gunzip`, which should already be available on most systems. From the location where you store the case study ipynb file, this can be done via the following commands:

```
mdkir data/
mkdir data/Haber-et-al_mouse-intestinal-epithelium/
cd data/Haber-et-al_mouse-intestinal-epithelium/
wget ftp://ftp.ncbi.nlm.nih.gov/geo/series/GSE92nnn/GSE92332/suppl/GSE92332_RAW.tar
mkdir GSE92332_RAW
tar -C GSE92332_RAW -xvf GSE92332_RAW.tar
gunzip GSE_92332_RAW/*_Regional_*
```

### Requirements

General:
- Jupyter notebook
- IRKernel
- rpy2
- R >= 3.4.3
- Python >= 3.5

Python:
- scanpy
- numpy
- scipy
- pandas < 0.24.0
- seaborn
- louvain>=0.6
- python-igraph
- ComBat python implementation from Maren Buettner's github (mbuttner/maren_codes/combat.py)
- python-gprofiler from Valentine Svensson's github (vals/python-gprofiler)

R:
- scater
- scran
- MAST
- gam
- slingshot (change DESCRIPTION file for R version 3.4.3)
- monocle 2
- limma
- splines
- ComplexHeatmap
- RColorBrewer
- clusterExperiment
- ggplot2

#### Application notes:

For R 3.4.3:
When using Slingshot in R 3.4.3, you must pull a local copy of slingshot via the github repository and change the `DESCRIPTION` file to say `R>=3.4.3` instead of `R>=3.5.0`.

For R>=3.5 and bioconductor>= 3.7:
The clusterExperiment version that comes for bioconductor 3.7 has slightly changed naming convention. `clusterExperiment()` is now called `ClusterExperiment()`. The script should be changed accordingly when using newer versions of R.

For Pandas>=0.24.0:
Pandas 0.24.0 is currently not compatible with rpy2 < 3.0.0. Once rpy2 3.0.0 is released, the tutorial should work with latest package updates. Please note that Pandas 0.24.0 requires anndata version 0.6.18 and a scanpy version > 1.37.0.

### Adapting the pipeline for other datasets:

The pipeline was designed to be easily adaptable to new datasets. However, there are several limitations to the general applicability of the current workflow. When adapting the pipeline for your own dataset please take into account the following:

1.  sparse data formats are not supported by `rpy2` and therefore do not work with any of the integrated R commands. Datasets can be turned into a dense format using the code: `adata.X = adata.X.toarray()`

2. The case study assumes that the input data is count data obtained from a single-cell protocol with UMIs. If the input data is read data, then the normalization method should be replaced with another method that includes gene length normalization (e.g., TPM, TMM).