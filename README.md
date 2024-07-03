# EcoHAB_AUV_FlashWeave

This repository contains code to extract, transform, and load the AUV 16S, 18Sv9, and metadata to get it ready for [FlashWeave co-occurrence network](https://github.com/meringlab/FlashWeave.jl) building. You can run through the steps to run the ETL in the notebook **01_AUV_FlashWeave_ETL.ipynb**. Once these steps are run, you can either run the final cell to generate the network locally, or move the cleaned data and run_flashweave_v9.jl script to a supercomputer and run it there using `julia run_flashweave_v9.jl 16S_filepath 18S_filepath metadata_filepath outfile_filepath`.

## Data

The data was supplied by Monica, and placed in the data/input folder, with the following structure:

```
data/input
├── 16S_AUV
│   ├── 230314_EH21_AUV_Metadata.txt
│   ├── NO-plastid-dada2-table.csv
│   └── taxonomy.csv
└── 18SV9_AUV
    ├── 230314_EH21_AUV_Metadata_forAUVV9.csv
    ├── 230821_EH21_AUV_meta_with_dab.csv
    ├── dada2-table.csv
    └── taxonomy_AUV18SV9.csv
```

## Prerequisites:

- Python version 3.10.9 (may work with higher versions, but developed and tested with 3.10.9)
- Julia version 1.6+ (https://julialang.org/downloads/) (tested with 1.9.3)
- FlashWeave (see instructions on installing at [FlashWeave](https://github.com/meringlab/FlashWeave.jl))
- Python requirements: install with `pip install -r requirements.txt`
- R requirements:
    - circlize
    - pals
    - install with: `install.packages(c('circlize', 'pals'))`

## 01_AUV_FlashWeave_ETL.ipynb

- This notebook generates the transformed, loaded, and cleaned 16S and 18S V9 OTU data subsetted to the samples included in each of the 16S, 18S, and metadata files, with lower-frequency OTUs filtered out, following the same steps as the [Chaffron paper](https://www.science.org/doi/10.1126/sciadv.abg1921): "For each sample, the upper quartile (Q3) of its nonzero abundance values was computed. An OTU was retained when its observed abundance was higher than Q3 in at least five samples."
- This notebook also has a final cell that can run the FlashWeave co-occurrence network generation locally, if your computer is powerful enough to do so.

## 02_AUV_Circlize_ETL.ipynb

- This notebook genereates the edgelists necessary to build a circos plot using the circlize package in R. The final cells in the notebook generate an edgelist for Pseudo-nitzschia and Bacillariophyta, and runs the R script to generate the circos plot.
