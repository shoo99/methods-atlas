# Gene Fusion Detection

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

암 driver fusion (BCR-ABL, EML4-ALK, TMPRSS2-ERG 등) 및 신규 chimeric transcript 을 RNA-seq에서 검출. 임상 종양학에서 actionable target 발굴의 핵심 분석.

**누가 의뢰**: 임상 종양 패널 (NCC, 병리), 약물 (TKI, IMM-targeted) 동반진단 R&D, leukemia/sarcoma 진단.

## Input

- **Data type**: Bulk RNA-seq paired-end (이상적: 100bp+ PE)
- **Read depth**: 30-50M PE reads
- **Tissue**: tumor (matched normal 권장 — false fusion 필터)
- **Long-read (옵션)**: Iso-Seq for full-length fusion structure

## Pipeline

```
FASTQ → multi-caller (STAR-Fusion + Arriba + FusionCatcher) → SURVIVOR-style merge
  → high-confidence filtering (recurrence + read support)
  → known fusion DB matching (COSMIC, ChimerKB, Mitelman)
  → in-frame check + ORF/domain → druggability (OncoKB)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **STAR-Fusion** | 1.13 | STAR-aligned, fast, sensitive |
| **Arriba** | 2.4 | high precision, clinical-grade |
| **FusionCatcher** | 1.33 | many filters, high recall |
| (alt) **JAFFA** | 2.3 | assembly-based |
| **CICERO** | 1.9 | structural variant aware |
| Merge | custom + AGFusion | – | annotation |
| Druggability | OncoKB | API | actionable |

## Output

- `results/fusions_consensus.tsv` — multi-caller support
- `results/fusions_annotated.tsv` — gene, breakpoint, in-frame, COSMIC, OncoKB
- `figures/`:
  - `fusion_circos.png`
  - `fusion_structure.png` per top fusion
  - `recurrence_heatmap.png` (cohort)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/fusion:v1.0.0
docker run --rm -v $PWD:/work replisci/fusion:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **K562 cell line** — BCR-ABL1 t(9;22) positive (well-characterized)
- TCGA LAML subset
- Validation: known fusion detection rate

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| Single sample multi-caller | ~3 h | 16 cores | 64 GB |
| Cohort (n=20) | ~12 h | 32 cores | 128 GB |

## Limitations

- ❌ **High false positive rate** — single-caller은 50%+ FP. multi-caller consensus 필수
- ⚠ **Read-through** — adjacent gene 의 read-through은 fusion 아님. 필터 필요
- ⚠ **Low expression fusion** — read support 적으면 검출 어려움
- ⚠ **Reference annotation 의존** — 새로운 fusion partner 발견 한계
- ⚠ **DNA SV 와 일치 확인 권장** — RNA fusion ≠ genomic SV 항상

## Quality Checks

- [x] Multi-caller agreement (≥2 callers)
- [x] Read support: split + spanning reads ≥ 3
- [x] In-frame fusion 비율
- [x] Known driver fusion 검출 (control)
- [x] Recurrent fusion (cohort) cross-validation

## References

- Haas BJ, et al. STAR-Fusion: Fast and Accurate Fusion Transcript Detection from RNA-Seq. *Genome Biology* 2019.
- Uhrig S, et al. Accurate and efficient detection of gene fusions from RNA sequencing data (Arriba). *Genome Res* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
