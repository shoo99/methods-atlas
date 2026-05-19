# ATAC-seq Footprinting (TF Binding Inference)

**Category**: Epigenomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

ATAC-seq cut profile 의 단백질 결합 "footprint" (protected region) 패턴에서 TF binding 직접 추론 — ChIP-seq 없이 TF activity 변화 측정.

**누가 의뢰**: ChIP-seq 불가능 sample (low input, FFPE) 의 TF inference, drug treatment TF activity 변화.

## Input

- High-coverage ATAC-seq (200M+ reads)
- Standard ATAC pipeline 거친 BAM + peaks

## Pipeline / Tools

- **TOBIAS** — gold standard (BINDetect, ATACorrect)
- **HINT-ATAC**
- **chromVAR** — motif accessibility variability (bulk + single-cell)

## Output

- TF footprint scores, differential TF binding, TF activity per condition

## Limitations

- ❌ Tn5 bias correction critical
- ⚠ Resolution lower than ChIP-seq
- ⚠ Family-shared motifs ambiguous

## References

- Bentsen M, et al. ATAC-seq footprinting unravels kinetics of transcription factor binding during zygotic genome activation (TOBIAS). *Nat Commun* 2020.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
