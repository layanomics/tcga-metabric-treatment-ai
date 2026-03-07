# Cross-Cohort Harmonization Roadmap
**Date:** February 23, 2026  
**Session:** 1.3 - Cross-Cohort Comparison

## Cohort Overview

| Metric | TCGA-BRCA | METABRIC | Combined |
|--------|-----------|----------|----------|
| Patients | 1,095 | 2,509 | 3,604 |
| Clinical variables | 107 | 36 | TBD |
| Expression genes | 60,660 (Ensembl) | 20,385 (symbols) | ~60 pathways |
| Platform | RNA-seq | Microarray | Harmonized |

## Critical Decisions

### 1. PAM50 Subtype Harmonization
**Recommendation:** Use 4 main subtypes (LumA, LumB, Her2, Basal)
- Excludes: Normal (TCGA: 19, METABRIC: 148)
- Excludes: claudin-low (METABRIC: 218)
- Excludes: NC (METABRIC: 6)
- **Rationale:** Cleaner model, larger sample sizes, standard in literature

**Alternative:** Keep all 7 classes if biological diversity matters

### 2. Expression Harmonization Strategy
**Selected Approach:** PAM50 + Pathway Activity Scores (GSVA)

**Why NOT direct gene merge:**
- Different platforms (RNA-seq vs microarray)
- Different gene ID formats (Ensembl vs symbols)
- Poor cross-platform concordance at gene level

**Why pathway scores:**
- Platform-agnostic (gene sets use symbols)
- Biologically interpretable
- Proven in literature (Kloet et al. 2020)
- Reduces dimensions (60,660 genes → 60 pathways)

### 3. Clinical Variable Schema
**Core harmonized variables (n=6-8):**
- Demographics: Age, Sex
- Biomarkers: ER, PR, HER2 (need remapping)
- Tumor: Stage, Size (partial)
- Outcomes: OS time, OS status
- Subtypes: PAM50

**Variables to create:**
- Standardized treatment indicators (chemo, hormone, radiation)
- Unified survival times (convert months→days)
- Derived features (triple-negative, HR-positive)

## Implementation Plan

### Week 2: Pathway Scoring
1. Download gene sets (Hallmark, KEGG, immune)
2. Map to gene symbols
3. Compute GSVA scores for TCGA
4. Compute GSVA scores for METABRIC
5. Z-score normalize within cohort

### Week 3: Clinical Harmonization
1. Create canonical schema
2. Map variables
3. Standardize categories
4. Handle missing data
5. Create merged clinical table

### Week 4: Final Merge & QC
1. Merge pathway scores + clinical
2. Quality checks (PCA, distributions)
3. Train/val/test splits
4. Export final dataset

## Key Strengths by Cohort

**TCGA Advantages:**
- More clinical variables (107 vs 36)
- Newer cohort (better treatment data)
- RNA-seq (full transcriptome)

**METABRIC Advantages:**
- Larger sample size (2.5x)
- Excellent RFS data (95% vs 19%)
- Longer follow-up
- Well-curated subtypes

**Combined Power:**
- 3,604 total patients
- Complementary strengths
- Better statistical power
- External validation possible (train TCGA, test METABRIC)
