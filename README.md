# Treatment Recommendation Model based on Merged Datasets

**Multi-cohort integration of TCGA-BRCA + METABRIC for treatment recommendation AI**

## Project Overview

- **Objective:** Build treatment recommendation model from harmonized TCGA-BRCA + METABRIC data
- **Approach:** PAM50 + pathway activity scores (GSVA) for cross-platform harmonization
- **Timeline:** Started February 2026
- **Status:** Week 1 - Data acquisition and setup

## Datasets

- **TCGA-BRCA:** 1,095 patients, RNA-seq expression, 105 clinical variables
- **METABRIC:** ~2,000 patients, microarray expression, comprehensive clinical data

## Installation

```bash
# Create environment
conda env create -f environment.yml

# Activate
conda activate treatment-ai

# Install R packages for GSVA
R
> install.packages("BiocManager")
> BiocManager::install("GSVA")
> BiocManager::install("GSEABase")
> quit()
```

## Project Structure

```
tcga-metabric-treatment-ai/
├── data/
│   ├── raw/tcga/          # TCGA-BRCA raw data
│   ├── raw/metabric/      # METABRIC raw data
│   ├── processed/         # Harmonized individual cohorts
│   └── merged/            # Final merged dataset
├── notebooks/             # Analysis notebooks
├── scripts/               # Python scripts
├── docs/                  # Documentation
├── results/
│   ├── figures/
│   └── tables/
└── environment.yml
```

## Progress Tracker

### Week 1 (Feb 20-25, 2026)

- [x] Technical strategy documentation
- [x] Project infrastructure setup
- [ ] METABRIC data acquisition
- [ ] TCGA expression download
- [ ] Cross-cohort comparison
- [ ] Pathway scoring

## Key References

- Kloet et al. 2020 - Gene-set transformation for cross-platform concordance
- Craven et al. 2021 - CIBERSORT on TCGA + METABRIC
- Chen et al. 2022 - Deep transfer learning across bulk RNA-seq
- Li et al. 2025 - Integrated prognostic model TCGA+METABRIC

## Contact

<layaan.essam@gmial.com>
