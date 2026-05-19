# ITS Fungal Amplicon Profiling

**Category**: Microbiome
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Internal Transcribed Spacer (ITS1/ITS2) amplicon sequencing — fungal community 동정. 16S가 박테리아용이라면 ITS는 fungal 표준. 종/속 수준 fungal microbiome.

**누가 의뢰**: 식품/와인/김치 fermentation, 농업 식물병 (Fusarium, Aspergillus 등), 임상 진균증, 환경 fungal diversity.

## Input

- **Primer**: ITS1F/ITS2 (ITS1 region) 또는 ITS3/ITS4 (ITS2)
- **Format**: paired-end Illumina (V2 chemistry로 ITS 길이 가변성 대응)
- **Reads / sample**: 10,000-50,000
- **Mock community**: ZymoBIOMICS fungal mock 권장

## Pipeline

```
FASTQ → ITSx (ITS region extraction — flanking SSU/LSU 제거)
  → DADA2 / VSEARCH (denoise to ASVs)
  → taxonomy vs UNITE database
  → diversity, ordination, differential abundance (QIIME 2)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **QIIME 2** | 2024.10 | – |
| **ITSx** | 1.1.3 | extract ITS from flanking regions |
| **DADA2** | 1.34 | denoise |
| **UNITE database** | 10 (2024) | fungal taxonomy reference |
| **PIPITS** | – | ITS-specific pipeline |
| **FUNGuild** | – | functional/ecological assignment |

## Output

- ASV × sample, taxonomy (Kingdom-Species), alpha/beta diversity, differential, FUNGuild (saprotroph/pathogen/symbiont)

## Demo / Time

- UNITE example dataset
- ~1 h per cohort

## Limitations

- ❌ ITS region length variability — alignment 어려움. Sequence-based methods 권장
- ⚠ Database coverage uneven — Ascomycota > Basidiomycota > others
- ⚠ Species-level 신뢰도 변동
- ⚠ Spike-in mock으로 정량 정확도 검증 권장

## References

- Bengtsson-Palme J, et al. Improved software detection and extraction of ITS1 and ITS2 from ribosomal ITS sequences (ITSx). *Methods Ecol Evol* 2013.
- Abarenkov K, et al. UNITE general FASTA release for Fungi. *UNITE Community* 2024.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
