# Polygenic Risk Score (PRS)

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

GWAS summary statistics를 활용해 individual genome 으로부터 trait/disease risk score 산출. 임상 risk stratification, 약물 반응 PGx, 운동선수/육종 형질 예측 등.

**누가 의뢰**: 임상 risk stratification (CVD, T2D, 암), PGx (warfarin, clopidogrel), 농수산 GS-PRS.

## Input

- **Target genotype**: PLINK / VCF (이미 imputed, QC'd)
- **GWAS summary statistics**: SNP, A1/A2, beta, SE, p, MAF (PGS Catalog 또는 자체 GWAS)
- **Reference**: 1000G, HapMap3, UK Biobank
- **Phenotype** (validation): training/testing cohort

## Pipeline

```
Target QC → genotype harmonization (strand flip, allele match)
  → PRSice-2 / LDpred2 / PRS-CS / SBayesR (score calculation)
  → covariate adjustment (PC, age, sex) → calibration
  → AUC/R² in independent test cohort
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **PRSice-2** | 2.3.5 | classic, clumping + thresholding |
| **LDpred2** | R bigsnpr 1.12 | Bayesian, LD-aware |
| **PRS-CS** | 1.1 | continuous shrinkage prior |
| **SBayesR** | 2.4 | summary Bayesian |
| **MegaPRS** | – | quick + accurate |
| **PGS Catalog Calculator** | 2.0 | standardized |

## Output

- `results/prs_per_sample.tsv` — PRS scores
- `results/auc_test.tsv` — performance metrics
- `figures/`: distribution by case/control, risk decile plot, ROC

## Demo / Time

- UK Biobank summary stats + PGS Catalog
- ~30 min – 6 h depending on tool/cohort size

## Limitations

- ❌ Cross-ancestry transferability 낮음 — non-EUR cohort에서 R² 30-50% 감소
- ⚠ Common variants only — rare variant 기여 X
- ⚠ Linear assumption — gene×environment interaction 미반영
- ⚠ Calibration 필요 — score를 absolute risk로 변환

## References

- Ge T, et al. Polygenic prediction via Bayesian regression and continuous shrinkage priors (PRS-CS). *Nat Commun* 2019.
- Privé F, et al. LDpred2: better, faster, stronger. *Bioinformatics* 2020.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
