# HiChIP / ChIA-PET (Protein-specific 3D Genome)

**Category**: Epigenomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Hi-C + specific protein IP — TF (CTCF, YY1) 또는 histone mark (H3K27ac) 매개 chromatin loop 만 선택적 enrich. 전체 Hi-C보다 enhancer-promoter loop 검출에 더 효율적.

**누가 의뢰**: enhancer-promoter looping, cohesin/CTCF biology, transcription factor 3D contacts.

## Input

- HiChIP / ChIA-PET library (Hi-C + IP)
- 100-200M PE reads per sample
- Specific antibody (CTCF, H3K27ac, Pol2)

## Pipeline / Tools

- **HiC-Pro** + protein filter
- **hichipper** — HiChIP-specific
- **FitHiChIP** — loop calling
- **CID** — alternative

## Output

- Loop calls (bedpe), TF-anchored enhancer-promoter pairs

## Limitations

- ❌ Antibody-dependent
- ⚠ Depth requirements similar to Hi-C
- ⚠ Loop annotation context-dependent

## References

- Mumbach MR, et al. HiChIP: efficient and sensitive analysis of protein-directed genome architecture. *Nat Methods* 2016.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
