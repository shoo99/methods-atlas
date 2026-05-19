# TCR / BCR Immune Repertoire Analysis

**Category**: Genomics / Single-cell
**Tier**: 2
**Status**: 📝 1-pager

## Overview

T cell (TCR) / B cell (BCR/Ig) receptor 의 V(D)J recombination 결과를 sequencing. **Bulk** TCR/BCR-seq 또는 **single-cell** (10x Immune Profiling) — clone tracking, repertoire diversity, antigen specificity 추정.

**누가 의뢰**: 면역치료 (CAR-T, TIL) clone monitoring, 백신 반응 (B cell response), 자가면역, 감염 (HIV/COVID broadly neutralizing antibody), MRD (minimal residual disease).

## Input

- **Bulk**: 5' RACE / multiplex PCR → MiSeq/NovaSeq paired-end
- **Single-cell**: 10x Chromium 5' v2 + V(D)J library (matched scRNA + TCR/BCR)
- **Sample**: PBMC / sorted T or B cell / tumor TIL
- **Spike-in / UMI** 권장 (정확한 clone abundance)

## Pipeline

```
[Bulk]
FASTQ → MiXCR → clonotype table (V-J-CDR3)
  → diversity (Shannon, Simpson, Gini)
  → clone tracking across samples → public clones DB matching (VDJdb, TCRdb)

[Single-cell]
CellRanger vdj → contig → clonotype
  → scirpy / dandelion — integrate with scRNA (Scanpy/Seurat)
  → clone × cell-state — exhausted/memory/effector
  → BCR somatic hypermutation tree (SHazaM, Alakazam)
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MiXCR** | 4.7 | bulk gold standard |
| (alt) **IgBLAST** | 1.22 | NCBI, classic |
| **CellRanger vdj** | 8.0 | 10x official |
| **scirpy** | 0.17 | Python scRNA + TCR/BCR |
| **dandelion** | 0.4 | BCR-specific scRNA integration |
| **Immcantation** | 4.5 | suite: Change-O, SHazaM, Alakazam |
| **GLIPH2** | – | TCR specificity grouping |
| **VDJdb / TCRdb / iEDB** | – | known antigen-specific TCR |

## Output

- `results/`:
  - `clonotypes.tsv` — CDR3, V, J, count, frequency
  - `diversity_metrics.tsv`
  - `public_match.tsv` — known specificity
  - `scRNA_clone_per_cell.tsv` (single-cell)
- `figures/`:
  - `clone_size_distribution.png`
  - `repertoire_overlap.png`
  - `umap_clone_colored.png` (single-cell)
  - `bcr_lineage_tree.png`
  - `cdr3_length_dist.png`

## Demo / Time

- 10x PBMC 10k 5' + V(D)J demo
- Bulk: ~2 h per sample (MiXCR); single-cell: ~30 min after CellRanger

## Limitations

- ❌ Bulk은 paired α/β chain X (full-length single-cell만 가능)
- ⚠ PCR bias — UMI 사용 권장
- ⚠ Public DB coverage 제한 — known antigen-specific TCR는 일부만
- ⚠ BCR somatic mutation tracking은 longitudinal sampling 필요

## References

- Bolotin DA, et al. MiXCR: software for comprehensive adaptive immunity profiling. *Nat Methods* 2015.
- Sturm G, et al. Scirpy: a Scanpy extension for analyzing single-cell T-cell receptor-sequencing data. *Bioinformatics* 2020.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
