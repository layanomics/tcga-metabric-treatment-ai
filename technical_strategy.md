# Multi-Cohort Treatment AI - Technical Strategy

**Project:** Treatment Recommendation Model based on Merged Datasets  
**Date:** February 20, 2026  
**Cohorts:** TCGA-BRCA + METABRIC

---

## Selected Harmonization Approach

### ✅ CHOSEN: PAM50 + Pathway Activity Scores (GSVA)

**Rationale:**

- **#1 ranked approach** for cross-platform breast cancer integration
- Gene-set transformation improves concordance between RNA-seq and microarray while preserving biology (Kloet et al., 2020)
- Lower-dimensional, stable features suited for treatment prediction models
- Less sensitive to platform artifacts than gene-level integration
- Directly aligned with treatment biology (ER/PR/HER2 signaling, proliferation, immune)

**Why NOT batch correction on raw genes:**

- RNA-seq + microarray violate distribution assumptions for ComBat
- Risk of over-correcting and removing real biological differences
- Platform and biology are confounded

**Why NOT direct gene-level merge:**

- Poor concordance between platforms at gene level
- Heavily impacted by platform-specific dynamic range

---

## Selected Pathway Databases

### 1. MSigDB Hallmark Gene Sets (~50 pathways)

- **Use:** Compact, non-redundant oncogenic processes
- **Relevance:** E2F targets, estrogen response, apoptosis → endocrine therapy, chemo, targeted agents
- **Citation:** Ge et al., 2018; Zhong et al., 2024

### 2. MSigDB C2: KEGG & Reactome Collections

- **Use:** Drug-target pathways (PI3K-AKT, cell cycle, DNA repair)
- **Relevance:** Directly maps to treatment mechanisms
- **Citation:** Huang et al., 2025; Zhong et al., 2024

### 3. Breast Cancer-Specific Signatures (PAM50)

- **Use:** Intrinsic subtype features
- **Relevance:** Clinical treatment decisions (hormonal vs HER2-targeted vs chemo)
- **Citation:** Kloet et al., 2020

### 4. Immune/TME Gene Sets (CIBERSORT, Immune Hallmarks)

- **Use:** T-cell subsets, immune infiltration
- **Relevance:** Immunotherapy response, chemo-immune interactions
- **Citation:** Craven et al., 2021

**Total features:** ~60-80 pathway scores + PAM50 subtype

---

## Implementation Plan

### Step 1: Compute Pathway Scores (Per Cohort)

- TCGA: GSVA on RNA-seq TPM
- METABRIC: GSVA on microarray normalized
- Z-score normalize **within each cohort**

### Step 2: Optional Residual Batch Correction

- After pathway transformation
- Light ComBat on pathway scores if needed
- Only if PCA shows residual cohort clustering

### Step 3: Merge with Clinical Variables

- Harmonize clinical schema
- Combine pathway scores + clinical + outcomes

---

## Validation Criteria

### ✅ Biology Preserved

- [ ] PAM50 subtypes show expected survival differences in each cohort
- [ ] ER/HER2-pathway patterns align with receptor status
- [ ] Proliferation signatures correlate with grade/Ki67

### ✅ Batch Effects Minimized

- [ ] PCA: Samples cluster by subtype/biology, NOT by cohort
- [ ] Variance explained by "platform" < 5% after harmonization
- [ ] Cross-cohort prediction works (train TCGA, test METABRIC)

---

## Key Pitfalls to Avoid

❌ Don't apply ComBat directly to RNA-seq + microarray  
❌ Don't use overly large/noisy gene sets (reduces concordance)  
❌ Don't over-correct and flatten biological differences  
✅ Do stratify by clinical covariates when checking batch removal  

---

## References

- Kloet et al. 2020 - Gene-set transformation improves cross-platform concordance
- Craven et al. 2021 - CIBERSORT on TCGA + METABRIC TNBC
- Ye et al. 2025 - Multimodal neoadjuvant therapy model (TCGA+METABRIC)
- Li et al. 2025 - Prognostic model combining METABRIC + TCGA
- Wang et al. 2018 - Unifying RNA-seq data from different sources
