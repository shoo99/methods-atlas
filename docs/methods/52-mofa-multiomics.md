# MOFA+ Multi-omics Factor Analysis

**Category**: Multi-omics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

여러 omics layer (RNA + ATAC + protein + metabolomics 등) 를 **공통 latent factor** 로 분해하는 unsupervised factor analysis. 각 factor의 sample/cell weighting 과 feature loading 을 통해 cross-omics 변동 축 발견.

**누가 의뢰**: cohort multi-omics (CPTAC 등), 임상 multi-modality, single-cell multi-omics (CITE/multiome) downstream.

## Input

- **각 modality의 matrix**: gene × sample (RNA), peak × sample (ATAC), protein × sample
- **Sample 매칭** 필수 (paired)
- **Cell-level (MEFISTO)**: spatial/temporal axis 포함

## Pipeline

```
Multiple normalized matrices (paired samples)
  → MOFA+ training → K latent factors
  → factor variance per modality → assess shared vs unique
  → top features per factor (loadings)
  → factor × phenotype correlation → biological interpretation
  → enrichment per factor → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MOFA+** | R/Python 1.10 | factor analysis |
| **MEFISTO** | – | spatial/temporal MOFA |
| **muon** | 0.1 | MuData multi-modal in Python |
| **mixOmics** | R 6.30 | block PLS alternative |

## Output

- Factor weights × sample, loadings per feature, variance decomposition, factor-phenotype correlations

## Demo / Time

- CLL data (Argelaguet et al. MOFA paper) — public
- ~30 min for moderate cohort

## Limitations

- ❌ Linear model — non-linear relationships missed (deep learning 대안)
- ⚠ Factor 해석은 사람 — 자동 X
- ⚠ Modality 별 sample 결측 처리 정책 명시
- ⚠ Single-cell scale은 muon/MEFISTO 권장

## References

- Argelaguet R, et al. MOFA+: a statistical framework for comprehensive integration of multi-modal single-cell data. *Genome Biology* 2020.
- Velten B, et al. Identifying temporal and spatial patterns of variation from multimodal data using MEFISTO. *Nat Methods* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
