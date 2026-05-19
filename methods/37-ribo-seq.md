# Ribo-seq (Translation Profile)

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Ribosome-protected mRNA fragments (RPF, ~28-30 nt) sequencing 으로 **translation efficiency** 측정. 발현 ≠ 단백질 양 — Ribo-seq + RNA-seq 조합으로 transcript별 translation level 추정. 신호전달, stress response, viral translation 등.

**누가 의뢰**: protein synthesis pathway 연구 (mTOR, eIF), 약물 (translation inhibitor) 효과, viral translation, IRES, uORF discovery.

## Input

- **Library**: cycloheximide treatment + RNase digestion + RPF library
- **Read length**: 26-34 nt
- **Read depth**: 30-50M RPF / sample + matched mRNA-seq
- **Replicate**: ≥3

## Pipeline

```
RPF FASTQ → trim → rRNA filter (Bowtie2 vs rRNA)
  → STAR (RNA-seq style align) → length filter 26-34 nt
  → P-site offset calculation → ORF mapping
  → RiboTaper / Ribo-TISH (uORF/ORF prediction)
  → translation efficiency (RPF / mRNA) → DE in TE (xtail, RiboDiff)
  → codon usage analysis
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **Ribo-TISH** | 0.2 | TIS prediction, translation analysis |
| **RiboTaper** | 1.3 | ORF finding |
| **Plastid** | 0.6 | ribosome profiling Python |
| **xtail** | R 1.1 | translation efficiency DE |
| **RiboDiff** | 0.2 | alternative DE |
| **PRICE** | 1.0 | de novo ORF |
| **RibORF** | 1.0 | ribosome occupancy DE |

## Output

- RPF count, TE (translation efficiency), uORF/sORF predictions, codon usage, p-site occupancy

## Demo / Time

- GSE89704 (Bazzini et al., zebrafish translation)
- ~4 h per sample

## Limitations

- ❌ Cycloheximide artifact — initiation site bias
- ⚠ rRNA contamination 큼 — depletion 필수
- ⚠ Phasing (3-nt periodicity) sample quality 의존
- ⚠ Matched RNA-seq 필수 (TE 계산)

## References

- Ingolia NT, et al. Genome-wide analysis in vivo of translation with nucleotide resolution using ribosome profiling. *Science* 2009.
- Calviello L, et al. Detecting actively translated open reading frames in ribosome profiling data (RiboTaper). *Nat Methods* 2016.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
