# Nascent Transcription (GRO-seq / PRO-seq / NET-seq)

**Category**: Transcriptomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

활성 전사 (active transcription) 의 직접 측정 — paused RNA Pol II, enhancer RNAs (eRNAs), divergent transcription, nascent RNA dynamics. mRNA-seq의 steady-state 측정과 본질적으로 다름.

**누가 의뢰**: enhancer-promoter pause 메커니즘, 약물 transcription kinetics, eRNA discovery.

## Input

- Specialized library: Nuclear run-on (GRO/PRO-seq), immunoprecipitation (NET-seq)
- High depth: 50-100M

## Pipeline / Tools

- **groHMM / dREG / GROfit** — nascent RNA peak/TFE calling
- **PINTS** — pause site identification
- **HOMER nascent mode**

## Output

- TFE (transcription factor element) coordinates, pause indices, eRNAs, sense/antisense fluxes

## Limitations

- ❌ Specialized wet-lab protocol — most labs not setup
- ⚠ Strand-specific essential
- ⚠ Computational tools less mature

## References

- Core LJ, et al. Defining the status of RNA polymerase at promoters (GRO-seq). *Cell Reports* 2014.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
