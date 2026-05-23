# Perturb-seq / CRISPR Screen scRNA-seq

**Category**: Single-cell
**Tier**: 3
**Status**: 📝 1-pager

## Overview

CRISPR sgRNA perturbation + scRNA-seq — 수천 perturbation 의 transcriptomic effect 동시 측정. Gene function 발견, pathway 추론, drug target mechanism.

**누가 의뢰**: drug target validation, signaling pathway mechanism, neurodevelopmental disorder genes.

## Input

- 10x Perturb-seq library (cell-attached guide barcode)
- sgRNA library design (genome-wide or focused)
- Coverage: 100-200 cells per sgRNA

## Pipeline / Tools

- **CellRanger feature barcode mode**
- **mixscape (Seurat)** / **scMageck** — differential expression per perturbation
- **GEMINI / SCEPTRE** — high-MOI mixed perturbation
- **scGen / CPA** — perturbation prediction

## Output

- Per-perturbation DE, perturbation module, predicted functional cluster

## Limitations

- ❌ MOI 변동 (single vs multi-guide) — assignment 문제
- ⚠ KO efficiency variable per guide
- ⚠ Off-target effect — guide redundancy

## References

- Dixit A, et al. Perturb-Seq. *Cell* 2016.
- Replogle JM, et al. Mapping information-rich genotype-phenotype landscapes with genome-scale Perturb-seq. *Cell* 2022.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
