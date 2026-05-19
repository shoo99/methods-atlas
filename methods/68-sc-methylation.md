# Single-cell DNA Methylation (sc-WGBS / sciMET)

**Category**: Single-cell / Epigenomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Single-cell bisulfite sequencing — 세포 type별 methylome heterogeneity. Bulk WGBS의 평균 신호 한계를 단일세포 해상도로 해부.

**누가 의뢰**: 종양 epigenetic clonal evolution, 발달 epigenetic dynamics, immune cell DNA methylation diversity.

## Input

- Single-cell BS library (sciMET, snmC-seq, sc-snMC2T-seq)
- Sparse coverage per cell (5-10% CpG)

## Pipeline / Tools

- **Bismark single-cell mode**
- **scbs / sciMETv2 pipeline**
- **ASCAT-met** — clone-level
- **scmet** — Bayesian single-cell methylation

## Output

- Per-cell CpG methylation matrix (sparse), cell type clustering by methylation, DMR per cluster

## Limitations

- ❌ Extreme sparsity (>90% missing)
- ⚠ Limited coverage per cell → imputation 필요
- ⚠ Throughput limited vs scRNA

## References

- Mulqueen RM, et al. Highly scalable generation of DNA methylation profiles in single cells. *Nat Biotechnol* 2018.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
