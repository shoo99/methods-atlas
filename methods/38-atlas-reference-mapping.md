# Reference Atlas Mapping (Single-cell)

**Category**: Single-cell
**Tier**: 2
**Status**: 📝 1-pager

## Overview

작은 scRNA-seq query 데이터를 **대규모 reference atlas** (HCA, Tabula Sapiens, organ-specific 등) 에 mapping → 자동 cell type annotation + label transfer. 직접 clustering 없이 빠른 분석 + cross-study 일관성.

**누가 의뢰**: 임상 sample 빠른 cell-type annotation, 신규 datasets vs published atlas 비교, drug effect 평가.

## Input

- **Query**: scRNA-seq AnnData / Seurat
- **Reference atlas**:
  - Human Cell Atlas (HCA), Tabula Sapiens, Tabula Muris
  - Organ-specific: Allen brain, HLCA (lung), HCL (gut), CellRef (kidney)
- **Optional**: pretrained model (Azimuth, CellTypist)

## Pipeline

```
Query QC'd AnnData
  ↓ select reference
Mapping methods:
  ├─ Azimuth (Seurat) — anchor-based, web
  ├─ scArches / scVI surgery — transfer learning
  ├─ Symphony — fast embedding, scalable
  ├─ CellTypist — multinomial logistic regression, pretrained models
  ├─ scANVI — semi-supervised
  ↓
Label transfer + confidence score → UMAP visualization
Compositional change vs reference (LM-CL, scCODA)
DE per cell type vs reference (out-of-distribution)
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **Azimuth** | 5.0 | Seurat-based web/local |
| **scArches** | 0.6 | scVI/totalVI surgery |
| **Symphony** | R 0.1 | fast, scalable |
| **CellTypist** | 1.6 | LR classifier, 30+ models |
| **scPoli** | 0.6 | conditional, reference-aware |

## Output

- Predicted cell type per cell + confidence, projected UMAP, composition table, novel population flag

## Demo / Time

- 10x PBMC 10k → CellTypist Immune_All_Low model
- ~10-30 min on 10k cells

## Limitations

- ❌ Out-of-distribution (OOD) detection — reference에 없는 cell type은 closest로 잘못 분류
- ⚠ Reference quality dependent — outdated atlas는 잘못된 label
- ⚠ Batch correction 한계 — extreme batch는 mapping 실패
- ⚠ Rare population missing — large dataset preferred

## References

- Hao Y, et al. Integrated analysis of multimodal single-cell data (Azimuth). *Cell* 2021.
- Lotfollahi M, et al. Mapping single-cell data to reference atlases by transfer learning (scArches). *Nat Biotechnol* 2022.
- Domínguez Conde C, et al. Cross-tissue immune cell analysis reveals tissue-specific features in humans (CellTypist). *Science* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
