# Spatial Deconvolution (Spot → Cell Type Mapping)

**Category**: Spatial
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Visium 등 spot-level spatial transcriptomics를 **single-cell reference**로 deconvolution → 각 spot의 세포 type 비율 추론. Tissue architecture × cell-type 결합 분석에 필수.

**누가 의뢰**: tumor microenvironment cell composition, brain region cell-type mapping, organ niche 분석.

## Input

- **Spatial**: Visium AnnData (spot × gene)
- **Reference scRNA**: matched tissue scRNA atlas (cell type labeled)
- **Sample**: matched sample 권장 (donor-paired ↑↑ accuracy)

## Pipeline

```
Spatial AnnData + scRNA reference
  → variable gene + cell-type signature
  → deconvolution methods (cell2location/RCTD/SpaCET/SPOTlight)
  → cell-type proportion per spot
  → spatial visualization → cell-type co-localization
  → niche / domain discovery (BayesSpace)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **cell2location** | 0.1.4 | Bayesian, ref-based, gold standard |
| **RCTD (spacexr)** | R 2.2 | classic, fast |
| **SpaCET** | 1.1 | tumor-specific, no ref needed |
| **SPOTlight** | R 1.10 | NMF-based |
| **Stereoscope** | 0.3 | probabilistic |
| **Tangram** | 1.0 | mapping single cells → spots |
| **CARD** | R 1.0 | high resolution |

## Output

- Cell-type proportion per spot, spatial cell-type maps, co-localization heatmap, niche identification

## Demo / Time

- 10x Visium mouse brain + Allen brain atlas
- cell2location: ~2 h with GPU

## Limitations

- ❌ Reference quality 결정적 — non-matched reference는 부정확
- ⚠ Rare cell types 검출 어려움 (proportion < 5%)
- ⚠ Cell-type 정의 다를 시 합쳐서 비교 필요
- ⚠ Visium 55 µm spot은 multi-cell average — single-cell resolution X

## References

- Kleshchevnikov V, et al. Cell2location maps fine-grained cell types in spatial transcriptomics. *Nat Biotechnol* 2022.
- Cable DM, et al. Robust decomposition of cell type mixtures in spatial transcriptomics (RCTD). *Nat Biotechnol* 2021.
- Ru B, et al. Estimation of cell lineages in tumors from spatial transcriptomics data (SpaCET). *Nat Commun* 2023.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
