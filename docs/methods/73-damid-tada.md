# DamID / TaDa (Tissue-specific Chromatin)

**Category**: Epigenomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Dam (DNA adenine methyltransferase) 와 fusion된 단백질이 결합하는 chromatin 영역에 GATC adenine methylation 표시 → DpnI digest 로 분리. No antibody, no crosslinking, low cell number 가능.

**누가 의뢰**: 항체 없는 단백질 (rare TF, complex), in vivo Drosophila tissue, low-input lineage chromatin.

## Input

- Dam-fusion 도입 cell/tissue
- Reads: ~10-30M PE

## Pipeline / Tools

- **damidseq_pipeline** — Marshall lab
- **DamMapper**
- Custom enrichment normalization vs Dam-only control

## Output

- Bound chromatin tracks, DamID peak calls, gene-level binding

## Limitations

- ❌ Resolution limited by GATC site distribution
- ⚠ Background correction critical
- ⚠ Mostly used in Drosophila / model organisms

## References

- Marshall OJ, Brand AH. damidseq_pipeline. *Bioinformatics* 2015.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
