# Hi-C / Micro-C (3D Genome Organization)

**Category**: Epigenomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

DNA 3D 접촉을 proximity ligation 으로 측정. **TADs, A/B compartments, chromatin loops, sub-TADs**, single-cell Hi-C 까지 분석. Micro-C는 nucleosome resolution, **Pore-C/Concatemer-Hi-C**는 multi-way 접촉.

**누가 의뢰**: enhancer-promoter looping (gene regulation), tumor genome rearrangement (translocation), iPS/development chromatin remodeling.

## Input

- **Hi-C** library: restriction enzyme (DpnII, MboI), 100-500M PE reads / sample
- **Micro-C**: MNase digest, 200-500M PE
- **Pore-C (long-read)**: ONT, multi-contact
- **Coverage**: 1-2 billion contacts for sub-Mb resolution

## Pipeline

```
FASTQ → BWA-MEM (각 read 독립 align)
  → pairtools → valid pairs (Hi-C structure)
  → cooler → multi-resolution contact matrix (.cool, .mcool)
  → normalize (ICE, KR balancing)
  → analysis:
     ├─ TAD: TADbit / HiCExplorer / TopDom
     ├─ compartment: cooltools eigs
     ├─ loops: Mustache / cLoops2 / HiCCUPS
     ├─ differential: HiC-DC+, multiHiCcompare, diffHiC
  → visualize: HiGlass, Juicebox
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **pairtools** | 1.0 | valid pair detection |
| **cooler / cooltools** | 0.10 / 0.7 | matrix manipulation |
| **HiCExplorer** | 3.7 | TAD, compartments, viz |
| **Mustache** | 1.3 | loop calling |
| **HiCCUPS** (Juicer) | 2.0 | loop calling |
| **HiGlass** | 1.13 | interactive viz |
| **Juicer / Juicebox** | 2.0 | classic pipeline |
| **HiC-Pro / 4DN pipeline** | 3.1 / – | end-to-end |

## Output

- Multi-resolution contact matrix (.mcool), TADs (bed), compartments (eigenvector), loops (bedpe), HiGlass tracks

## Demo / Time

- 4DN ENCSR489OCU (HCT116 Hi-C)
- ~24 h per sample (deep Hi-C)

## Limitations

- ❌ Depth 의존 — 500M+ reads 권장. 적으면 loop 검출 어려움
- ⚠ Single-cell Hi-C 매우 sparse — pooled 또는 imputation
- ⚠ Restriction enzyme bias — Hi-C site density 영향
- ⚠ Cell mixture는 평균 — heterogeneous tissue 해석 주의

## References

- Open2C, Abdennur N, et al. Cooltools: enabling high-resolution Hi-C analysis in Python. *bioRxiv* 2022.
- Durand NC, et al. Juicer Provides a One-Click System for Analyzing Loop-Resolution Hi-C Experiments. *Cell Systems* 2016.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
