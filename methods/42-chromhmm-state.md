# Chromatin State Segmentation (ChromHMM / Segway)

**Category**: Epigenomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

여러 histone mark + DNase/ATAC 신호를 통합해 게놈을 **chromatin state** (active promoter, enhancer, polycomb-repressed, heterochromatin 등)로 segmentation. ENCODE Roadmap의 핵심 자원.

**누가 의뢰**: 임상/연구 cell type chromatin atlas, comparative epigenomics, disease-associated enhancer profiling.

## Input

- **Histone ChIP-seq tracks**: H3K4me3, H3K4me1, H3K27ac, H3K36me3, H3K27me3, H3K9me3 (최소 5-6 marks)
- **ATAC/DNase** (선택)
- **Format**: BAM or bigWig per cell type
- **Replicate 평균** 또는 pooled

## Pipeline

```
ChIP-seq peaks/coverage → binarize (200 bp bins)
  → ChromHMM LearnModel — N-state HMM
  → state emission + transition probabilities
  → state assignment per bin
  → Segway (alternative) — DBN model
  → annotate states (active TSS, weak enhancer, polycomb 등)
  → comparative across cell types
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **ChromHMM** | 1.25 | HMM-based, ENCODE standard |
| **Segway** | 3.0 | DBN-based alternative |
| **EpiCSeg** | – | conditional segmentation |
| **IDEAS** | – | integrative across cell types |

## Output

- State track (BED), emission matrix, transition matrix, overlap with annotations (genes, enhancers, CpG islands), cross cell-type heatmaps

## Demo / Time

- ENCODE Roadmap 127 cell types (pre-computed available)
- New 6-mark dataset: ~6 h

## Limitations

- ❌ N-state 선택 heuristic — 보통 15-25 states. validation 필요
- ⚠ Mark coverage 부족 시 unreliable
- ⚠ Cell-type 차이 큰 경우 state label 일관성 어려움
- ⚠ Functional validation 별도 필요 (reporter assay, CRISPRi)

## References

- Ernst J, Kellis M. ChromHMM: automating chromatin-state discovery and characterization. *Nat Methods* 2012.
- Hoffman MM, et al. Unsupervised pattern discovery in human chromatin structure through genomic segmentation (Segway). *Nat Methods* 2012.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
