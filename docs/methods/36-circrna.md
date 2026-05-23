# circRNA Detection

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Back-splicing 으로 형성되는 covalently closed circular RNA — miRNA sponge, translation, biomarker. Total RNA-seq (rRNA-depleted) 에서 back-splice junction 검출.

**누가 의뢰**: 혈장 biomarker (안정성 ↑), neuro disease, 종양 circRNA panel.

## Input

- **Total RNA-seq** (rRNA depleted, NOT polyA-only)
- **Paired-end** 150 bp 권장
- **Read depth**: 80-100M PE / sample (circRNA는 abundance 낮음)
- **RNase R treated sample** (선택) — circRNA enrichment

## Pipeline

```
FASTQ → STAR (chimeric output) or BWA
  → CIRCexplorer3 / CIRI2 / find_circ — back-splice junction
  → consensus (≥2 tools) → annotation (circBase, circAtlas)
  → quantification (CIRIquant, DCC) → DE (DESeq2)
  → predict miRNA binding (TargetScan, RBPmap)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **CIRCexplorer3** | 3.0 | flexible, multi-aligner |
| **CIRI2** | 2.0.6 | sensitive |
| **find_circ** | 1.2 | classic |
| **DCC** | 0.5 | from STAR chimeric |
| **CIRIquant** | 1.2 | quantification |
| circBase / circAtlas | – | annotation DB |

## Output

- circRNA × sample BSJ counts, host gene info, DE results, miRNA target predictions

## Demo / Time

- ENCODE rRNA-depleted RNA-seq
- ~4 h per sample

## Limitations

- ❌ BSJ read 수 적음 — false positive 비율 ↑. multi-caller + RNase R 검증 권장
- ❌ Linear RNA contamination — abundance 측정 까다로움
- ⚠ Polyadenylation 차이로 polyA-only library에서는 검출 X (total RNA 필수)
- ⚠ Annotation incomplete — known DB 외 다수

## References

- Zhang XO, et al. Diverse alternative back-splicing and alternative splicing landscape of circular RNAs (CIRCexplorer3). *Genome Res* 2016.
- Gao Y, et al. Comprehensive identification of internal structure and alternative splicing events in circular RNAs (CIRI2). *Brief Bioinform* 2018.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
