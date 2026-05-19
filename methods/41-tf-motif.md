# TF Motif Analysis

**Category**: Epigenomics / Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

DNA 서열 (ChIP/ATAC peaks, promoter region 등) 에서 **TF binding motif** enrichment 또는 *de novo* 발견. Regulatory hypothesis 형성에 필수.

**누가 의뢰**: 거의 모든 ChIP-seq / ATAC-seq / CUT&Tag 의뢰의 후속 단계. enhancer activity 메커니즘 추정.

## Input

- **Peak BED files** (ChIP/ATAC/CUT&Tag) or **gene promoter sequences**
- **Background**: shuffled sequence 또는 GC-matched genome
- **Motif database**: JASPAR, CIS-BP, HOCOMOCO, TRANSFAC

## Pipeline

```
Peak BED → genome FASTA extraction
  ├─ Known motif enrichment: HOMER findMotifsGenome.pl
  ├─ AME (MEME suite) — enrichment vs background
  ├─ Centered motif: CentriMo
  ├─ De novo: MEME-ChIP / STREME
  ├─ Footprint (ATAC): TOBIAS / HINT-ATAC
  ↓
Top motifs → TF assignment → chromVAR (single-cell)
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **HOMER** | 4.11 | classic, fast, comprehensive |
| **MEME suite** (AME, MEME-ChIP, STREME, CentriMo) | 5.5 | gold standard |
| **chromVAR** | R 1.28 | single-cell motif activity |
| **monaLisa** | R 1.12 | enrichment + dose-response |
| **TOBIAS** | 0.16 | ATAC footprinting |
| **JASPAR 2024** | – | motif DB |

## Output

- Enriched motifs + p-value + TF name, motif logos, centrality plot, footprint plots

## Demo / Time

- ENCODE CTCF ChIP peaks → CTCF motif top enrichment expected
- ~30 min – 2 h

## Limitations

- ❌ Motif ≠ binding — DNA accessibility, cofactor, modification 영향
- ⚠ Family redundancy — paralogous TFs share motifs
- ⚠ Background choice 중요 — GC matched 권장
- ⚠ Long enhancers는 multiple motifs — context 필요

## References

- Heinz S, et al. Simple Combinations of Lineage-Determining Transcription Factors Prime cis-Regulatory Elements (HOMER). *Mol Cell* 2010.
- Bailey TL, et al. The MEME Suite. *NAR* 2015.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
