# Virome (DNA / RNA Viruses)

**Category**: Microbiome
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Metagenomics / metatranscriptomics 에서 **viral sequence** 검출 + 분류 + 신종 발견. Phage (gut virome), eukaryotic virus (감염병), 환경 virome.

**누가 의뢰**: 임상 진단 (sepsis pan-viral panel), gut phage-bacteria interaction, 식품/수산 viral outbreak, 환경 viral diversity.

## Input

- **Shotgun metagenome** (DNA virus) 또는 **metatranscriptome** (RNA virus 포함)
- Virion-enriched library (filtration + DNase + RNase) 권장 — eukaryotic virus
- **Read depth**: 5-20 GB / sample
- **Negative control 필수** — kitome contamination

## Pipeline

```
FASTQ → host removal → assembly (metaSPAdes / Flye)
  → viral contig prediction:
     ├─ VirSorter2 — multi-classifier
     ├─ geNomad — comprehensive, plasmid+virus
     ├─ viralVerify / CheckV (quality assessment)
  → taxonomy (vConTACT2, BLASTn vs RefSeq/IMG/VR)
  → host prediction (CRISPR spacer, iPHoP)
  → AMG (auxiliary metabolic genes) — DRAMv
  → abundance (read recruitment)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **VirSorter2** | 2.2 | classic viral detection |
| **geNomad** | 1.8 | virus + plasmid + chromosomal |
| **CheckV** | 1.0 | viral genome completeness |
| **vConTACT3** | 0.3 | gene-sharing network taxonomy |
| **iPHoP** | 1.3 | host prediction |
| **DRAMv** | 1.5 | viral functional annotation |
| **viralFlye** | 0.2 | long-read viral assembly |

## Output

- Viral contigs (FASTA + quality), taxonomy, host prediction, abundance, AMG profile

## Demo / Time

- Gut virome dataset (Gregory et al. 2020)
- ~24 h per sample

## Limitations

- ❌ Reference incompleteness — novel virus 비율 ↑, IMG/VR + RefSeq 결합 권장
- ⚠ Phage vs prophage — chromosomal-integrated 구별 필요
- ⚠ Low-abundance virus는 deeper depth 필요
- ⚠ RNA virus는 별도 metatranscriptome workflow

## References

- Guo J, et al. VirSorter2: a multi-classifier, expert-guided approach to detect diverse DNA and RNA viruses. *Microbiome* 2021.
- Camargo AP, et al. Identification of mobile genetic elements with geNomad. *Nat Biotechnol* 2024.
- Nayfach S, et al. CheckV assesses the quality and completeness of metagenome-assembled viral genomes. *Nat Biotechnol* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
