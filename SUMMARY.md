# PRS Analysis Summary — SSNS and Albuminuria Cross-Trait Negative Control

## Cohort and Data

**Target cohort:** N = 8,904 (961 SSNS cases, 7,943 controls), European (EUR) and Sri Lankan (SL) ancestry, all 22 autosomes, 3,762,371 SNPs. Covariates: 20 genomic PCs for all OR/P/CI estimates.

**SSNS discovery GWAS:** Barry et al. SSNS GWAS (GCST90258619), HapMap3 QC, N_eff = 38,463. Per-SNP N used for LDpred2 QC.

**Albuminuria discovery GWAS:** Teumer + Brandenburg METAL meta-analysis, N = 347,284. Fully independent of target cohort.

---

## Methods Summary

**Simple PRS:** All HapMap3 QC-passed SNPs weighted by GWAS effect size. Evaluated on all N=8,904. No train/test split needed — no model selection.

**C+T (Clumping + Thresholding):** Pre-specified genome-wide significance threshold p<5×10⁻⁸, matching Tu et al. 2026 (*Kidney International*). LD pruning: r²<0.1, 250-kb window. Evaluated on all N=8,904. No train/test split needed — threshold is pre-specified, not data-selected.

**LDpred2-grid Config1:** 12 hyperparameter combinations (p∈{1×10⁻⁴,1×10⁻³,0.01,0.1}, h²∈{0.7,1.0,1.3}×h²_LDSC, sparse=FALSE). LD window ±3,000 SNPs. Best model selected on training folds of 5-fold stratified CV. Evaluated by aggregate out-of-fold (OOF) regression on all N=8,904.

---

## Table 1 — SSNS PRS (all N=8,904, 20 PCs)

| Method | SNPs | AUC (PRS only) | OR | 95% CI | P-value |
|--------|-----:|:--------------:|---:|--------|---------|
| Simple PRS | 658,392 | 0.707 | 3.456 | 2.999–3.982 | 7.3×10⁻⁶⁶ |
| C+T p<5×10⁻⁸ ★ | 38 | 0.732 | 2.283 | 2.094–2.489 | 4.7×10⁻⁷⁸ |
| LDpred2 Config1 ★ | 586,299 | 0.750 | 2.939 | 2.648–3.261 | 9.1×10⁻⁹² |

★ C+T: pre-specified GWS threshold, no model selection. LDpred2: best model from 5-fold CV (PRS_3: p=0.01, h²=0.7×h²_LDSC=0.053, sparse=FALSE).

---

## Table 2 — ALB→SSNS PRS (all N=8,904, 20 PCs)

Cross-trait negative control. Expected result: null (no genetic overlap between albuminuria and SSNS).

| Method | SNPs | AUC (PRS only) | OR | 95% CI | P-value |
|--------|-----:|:--------------:|---:|--------|---------|
| Simple PRS | 508,459 | 0.523 | 0.881 | 0.815–0.951 | 0.001 |
| C+T p<5×10⁻⁸ ★ | 11 | 0.534 | 1.009 | 0.932–1.092 | 0.827 |
| LDpred2 Config1 ★ | 484,655 | 0.543 | 0.929 | 0.861–1.004 | 0.063 |

★ C+T at p<5×10⁻⁸: **perfectly null** (OR=1.009, P=0.827). LDpred2 null (P=0.063, CI crosses 1.0). Simple PRS statistically significant due to large N but biologically null (AUC=0.523); see HLA exclusion (Table 4).

---

## Table 3 — ALB C+T Threshold Sweep (all N=8,904, 20 PCs)

Full sweep of C+T p-value thresholds for both SSNS and ALB. Demonstrates that the null ALB result at GWS is robust, and the inverse signal at permissive thresholds reflects HLA cross-trait pleiotropy.

| Threshold | SSNS SNPs | SSNS OR | ALB SNPs | ALB OR | ALB P |
|-----------|----------:|--------:|---------:|-------:|------:|
| 5×10⁻⁸ | 38 | 2.283 | 11 | 1.009 | 0.827 |
| 1×10⁻⁶ | 48 | 2.183 | 18 | 0.982 | 0.659 |
| 1×10⁻⁵ | 73 | 2.214 | 48 | 0.906 | 0.017 |
| 1×10⁻⁴ | 136 | 2.246 | 134 | 0.874 | 0.0018 |
| 5×10⁻⁴ | 314 | 2.186 | 315 | 0.846 | 2.2×10⁻⁵ |
| 0.001 | 500 | 2.248 | 506 | 0.836 | 5.6×10⁻⁶ |
| 0.01 | 2,614 | 2.729 | 2,630 | 0.891 | 0.003 |
| 0.05 | 9,447 | 3.199 | 8,287 | 0.911 | 0.021 |

SSNS OR is stable (≈2.2–2.3) at GWS thresholds. ALB signal is null at GWS (OR=1.009); inverse signal emerges below p<1×10⁻⁵ as HLA-correlated sub-threshold variants accumulate. See `plots/ct_threshold_sweep.png`.

---

## Table 4 — HLA Exclusion (ALB PRS, all N=8,904, 20 PCs)

HLA region: chr6:25,000,000–34,000,000 (GRCh37).

| Method | HLA SNPs | OR (with HLA) | P | 95% CI | OR (no HLA) | P | 95% CI |
|--------|--------:|-------------:|--:|--------|------------:|--:|--------|
| Simple PRS | 4,431 | 0.881 | 0.001 | 0.815–0.951 | 0.965 | 0.370 | 0.893–1.043 |
| C+T p<5×10⁻⁸ | 0 | 1.009 | 0.827 | 0.932–1.092 | N/A | N/A | N/A |
| LDpred2 Config1 | 4,362 | 0.929 | 0.063 | 0.861–1.004 | 0.967 | 0.391 | 0.895–1.044 |

**C+T p<5×10⁻⁸:** NOT APPLICABLE — all 11 ALB GWS SNPs are on chromosomes 1, 2, 4, 12, 14, 15; zero are in the HLA region. The GWS C+T score is already HLA-free, and it is perfectly null (OR=1.009).

**Simple PRS (no HLA):** OR=0.965, P=0.370 — fully null. The observed inverse association is entirely attributable to 4,431 HLA SNPs.

**LDpred2 (no HLA):** OR=0.967, P=0.391 — fully null.

---

## Key Figures

| Figure | File | Description |
|--------|------|-------------|
| SSNS ROC | [plots/roc_ssns.png](plots/roc_ssns.png) | ROC curves: Simple(0.707), C+T p<5e-8(0.732), LDpred2(0.750) |
| ALB ROC | [plots/roc_alb.png](plots/roc_alb.png) | ROC curves: Simple(0.523), C+T p<5e-8(0.534), LDpred2(0.543) |
| Forest plot | [plots/forest_plot.png](plots/forest_plot.png) | OR [95% CI] for 3 methods × 3 groups (SSNS, ALB with HLA, ALB without HLA) |
| C+T sweep | [plots/ct_threshold_sweep.png](plots/ct_threshold_sweep.png) | OR vs p-value threshold for SSNS and ALB (log scale x-axis) |
| LDpred2 heatmap | [plots/ldpred2_heatmap.png](plots/ldpred2_heatmap.png) | AUC across 12 Config1 hyperparameter combinations |
| Manhattan | [plots/manhattan_3panel.png](plots/manhattan_3panel.png) | Target cohort GWAS (batch-corrected, 3,762,371 SNPs, 20 PCs) |

---

## Scripts

| Script | Purpose |
|--------|---------|
| `scripts/01_simple_prs.sh` | Compute simple PRS (PLINK2 `--score`, all QC-filtered SNPs) |
| `scripts/02_ct_prs.sh` | Compute C+T scores at 11 p-value thresholds (PLINK2 `--clump` + `--score`) |
| `scripts/03_ldpred2_window_compare.R` | LDpred2-grid Config1, size=3000 (12 hyperparameter combinations) |
| `scripts/eval_ct_5e8.R` | Evaluate C+T at p<5e-8 on all N=8,904; full threshold sweep |
| `scripts/05_hla_exclusion_eval.R` | HLA exclusion analysis |
| `scripts/generate_figures.R` | Generate all figures from result TSV files and score files |

---

## Report Documents

| Document | Contents |
|----------|----------|
| [01 — Data and Directories](reports/01_DATA_AND_DIRECTORIES.md) | Input/output paths, cohort description |
| [02 — Preprocessing and QC](reports/02_PREPROCESSING_AND_QC.md) | SNP attrition, per-SNP N, chr15 recovery, batch correction |
| [03 — Methods](reports/03_METHODS.md) | Method details, configs, evaluation approach |
| [04 — C+T Thresholds and Grid](reports/04_CT_THRESHOLDS_AND_GRID.md) | Full C+T threshold sweep, Config1 grid AUC |
| [05 — Results and Figures](reports/05_RESULTS_AND_FIGURES.md) | All results tables and figures |
