# Isoform / Alternative Splicing Analysis

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

같은 유전자에서 만들어지는 **다양한 transcript isoform** (SE, A3SS, A5SS, MXE, RI 등 alternative splicing event) 의 발현 차이를 검정. Bulk DE는 gene-level 통합 신호만 보지만, isoform analysis는 splicing 변화를 직접 검출한다.

**누가 의뢰**: 신경/근육 질환 (SMA, ALS — splicing 변이 핵심), 종양 (alternative splicing of MET, AR-V7 등), splicing modulator drug 효과 평가.

## Input

- **Data type**: Bulk RNA-seq paired-end Illumina (또는 long-read for full isoform)
- **Read length**: 100-150 bp paired-end (longer ↑ junction detection)
- **Read depth**: **40-60M reads / sample** (DE 보다 더 깊게)
- **Strand library 권장** — antisense isoform 구별
- **Long-read (옵션)**: Iso-Seq, ONT direct cDNA — full-length

## Pipeline

```
FASTQ → STAR (2-pass with junctions) → BAM
  → rMATS (event-level differential AS)
  → SUPPA2 (transcript usage)
  → DEXSeq (exon-level DE)
  → MAJIQ (local splicing variation, complex events)
  → IsoformSwitchAnalyzeR (functional consequence)
  → Long-read (옵션): Salmon/NanoCount → DTU
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Align | STAR (2-pass) | 2.7.11b | accurate junctions |
| **rMATS-turbo** | 4.3.0 | event-level (SE/A3SS/A5SS/MXE/RI) |
| **SUPPA2** | 2.4 | event PSI from quantification |
| **DEXSeq** | R/Bioc 1.52 | exon-level differential |
| **MAJIQ** | 2.5 | LSV, complex events |
| Salmon | 1.10.3 | transcript quant |
| IsoformSwitchAnalyzeR | R/Bioc 2.6 | isoform switching + ORF/domain |
| (Long-read) NanoCount | 1.1 | ONT transcript quant |
| (Long-read) FLAIR | 2.0 | full-length ID |

## Output

- `results/rmats_summary.tsv` — per event type
- `results/PSI_matrix.tsv` — Percent Spliced In
- `results/isoform_switches.tsv` — gene-level switches
- `figures/`:
  - `psi_distribution.png`
  - `event_volcano.png` per event type
  - `sashimi_plot.png` — splice junction visualization
  - `switch_consequence.png` — domain/ORF loss
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/isoform-splicing:v1.0.0
docker run --rm -v $PWD:/work replisci/isoform-splicing:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- GEO **GSE48278** (knockdown of PTBP1 — known splicing factor, well-characterized splicing changes)
- 또는 ENCODE shRNA splicing factor dataset

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| Demo (6-12 samples) | ~4 h | 16 cores | 64 GB |
| Full project | ~8 h | 32 cores | 128 GB |

## Limitations

- ❌ **Read depth 요구 ↑** — junction-spanning reads 필요. 20M 미만은 검정력 낮음
- ❌ **Short-read isoform 분리 한계** — long-read 통합 권장
- ⚠ **Tool 간 일치율 ~60%** — 다중 tool consensus 권장
- ⚠ **Annotation 의존** — 새로운 isoform 검출은 long-read 또는 *de novo*
- ⚠ **Sample mixing** — cell type 차이가 splicing 차이로 보일 수 있음
- ⚠ **Junction reads 수 미달** — minimum junction read count 필터 필수

## Quality Checks

- [x] STAR 2-pass junction count
- [x] PSI distribution bimodal (mostly 0 or 1)
- [x] Replicate PSI Pearson correlation > 0.95
- [x] Known splicing factor knockdown → expected target detected

## References

- Shen S, et al. rMATS: robust and flexible detection of differential alternative splicing from replicate RNA-Seq data. *PNAS* 2014.
- Trincado JL, et al. SUPPA2: fast, accurate, and uncertainty-aware differential splicing analysis. *Genome Biology* 2018.
- Vaquero-Garcia J, et al. MAJIQ: A new method to quantify and visualize splicing variation. *eLife* 2016.
- Vitting-Seerup K, Sandelin A. IsoformSwitchAnalyzeR: analysis of changes in genome-wide patterns of alternative splicing. *Bioinformatics* 2019.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
