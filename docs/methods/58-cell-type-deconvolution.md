# Bulk Cell-type Deconvolution

**Category**: Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Bulk RNA-seq / methylation 에서 **cell type composition** 추정 — scRNA가 없거나 임상 plasma/혈액 등 sample에서 cell type 비율 추론. 종양 면역미세환경, 혈액 cell composition, methylation EpiDISH 등.

**누가 의뢰**: 종양 면역 TME 분석 (bulk RNA-seq), 혈액 cell type 비율 변화, methylation cohort cell composition 보정.

## Input

- **Bulk expression matrix** (gene × sample) — TPM or normalized
- **Reference signature** (scRNA-derived or curated):
  - LM22 (CIBERSORT), CIBERSORTx custom, Tabula Sapiens
  - For methylation: FlowSorted.Blood (450K/EPIC)

## Pipeline

```
Bulk matrix + signature → deconvolution
  ├─ CIBERSORTx (gold standard, ν-SVR)
  ├─ MuSiC — single-cell reference based
  ├─ EpiDISH — methylation
  ├─ xCell — score-based (relative)
  ├─ EPIC, quanTIseq, BayesPrism
  ↓
Cell-type proportion per sample
→ statistical comparison vs condition → integration with DE
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **CIBERSORTx** | web | LM22 + custom signature |
| **MuSiC** | R 1.0 | scRNA-anchored |
| **xCell** | R 1.3 | score-based, 64 cell types |
| **EPIC** | R 1.1 | tumor-aware |
| **quanTIseq** | – | absolute fractions |
| **BayesPrism** | R 2.2 | Bayesian, scRNA reference |
| **EpiDISH** | R/Bioc 2.22 | methylation 450K/EPIC |

## Output

- Cell-type fraction per sample, comparison across conditions, batch effects of cell composition

## Demo / Time

- TCGA bulk RNA + CIBERSORTx LM22
- ~30 min per cohort

## Limitations

- ❌ Reference signature 정확도 결정적
- ⚠ Score-based (xCell) vs fraction (CIBERSORTx, EPIC) 결과 비교 어려움
- ⚠ Rare cell types proportion < 5% 검출 어려움
- ⚠ Tissue-specific cell types — reference 매칭 필수

## References

- Newman AM, et al. Determining cell type abundance and expression from bulk tissues with digital cytometry (CIBERSORTx). *Nat Biotechnol* 2019.
- Wang X, et al. Bulk tissue cell type deconvolution with multi-subject single-cell expression reference (MuSiC). *Nat Commun* 2019.
- Chu T, et al. Cell type and gene expression deconvolution with BayesPrism. *Nat Cancer* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
