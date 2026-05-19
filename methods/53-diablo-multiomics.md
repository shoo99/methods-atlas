# DIABLO Multi-omics Integration (Supervised)

**Category**: Multi-omics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

**Supervised** multi-omics integration — phenotype/group label 을 활용해 cross-omics discriminative signature 학습. sparse PLS-DA 기반. DIABLO (mixOmics) 가 표준.

**누가 의뢰**: 임상 cohort multi-omics classifier (disease vs normal), drug response signature 다층, multi-omics biomarker panel.

## Input

- Paired multi-omics matrices + group/phenotype label
- ≥3 samples per group, balanced 권장

## Pipeline

```
Multiple matrices + labels
  → DIABLO (sparse PLS-DA) — selected features per block
  → cross-validation (M-fold) → optimal sparsity
  → discriminative latent components → loadings
  → cross-omics network visualization (cim, circos)
  → Predictive performance (BER, AUC)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **DIABLO (mixOmics)** | R 6.30 | block sparse PLS-DA |
| **JIVE** | – | joint and individual variance |
| **iCluster+** | R 2.4 | Bayesian clustering |
| **MOFA+** (unsupervised) | – | – |

## Output

- Per-block feature loadings, latent variates, cross-omics correlation, classifier performance (AUC)

## Demo / Time

- TCGA-BRCA mRNA + miRNA + protein subtype prediction
- ~30 min – 2 h

## Limitations

- ❌ Supervised — label 정확도에 민감
- ⚠ Sparsity hyperparameter 선택 — CV로 안정화
- ⚠ Sample size > features per block needed (over-fit risk)

## References

- Singh A, et al. DIABLO: an integrative approach for identifying key molecular drivers from multi-omics assays. *Bioinformatics* 2019.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
