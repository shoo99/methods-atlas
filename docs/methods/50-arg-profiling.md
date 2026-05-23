# Antibiotic Resistance Gene (ARG) Profiling

**Category**: Microbiome
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Shotgun metagenomics 또는 isolate WGS 로부터 **항생제 내성 유전자** 검출. CARD/Resistome 기반 — 임상 (병원감염), 농축산 (가축 사료, 분뇨), 환경 (하수, 토양) 모두 적용.

**누가 의뢰**: 병원 내 다제내성균 surveillance, 식품 안전 (가축 분리주), one-health 모니터링.

## Input

- Shotgun metagenome FASTQ (deeper for sensitivity)
- 또는 isolate 게놈 (assembled or short-read)
- **Reference DB**: CARD, ResFinder, NCBI AMRfinderPlus DB

## Pipeline

```
[Metagenome]
FASTQ → host removal → CARD/RGI bwt mode (read-based)
  → ARG abundance per sample → relative quantification (RPKM)

[Isolate]
Assembly → CARD/RGI / AMRfinderPlus / ResFinder
  → presence/absence calls → MLST / clone tracking

→ contig context: plasmid (mob-suite) vs chromosomal
→ host bacteria assignment (host-tracking)
→ Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **CARD / RGI** | 3.3 | comprehensive, well-curated |
| **AMRfinderPlus** | 4.0 | NCBI, includes virulence |
| **ResFinder** | 4.5 | classical |
| **DeepARG** | 1.0 | deep learning, novel ARG |
| **mob-suite** | 3.1 | plasmid context |
| **Kleborate** | 3.0 | Klebsiella ARG + virulence |

## Output

- ARG × sample (presence/abundance), drug class summary, plasmid context, MGE (transposon, integron) overlap

## Demo / Time

- CHARM/EARN public hospital surveillance
- ~1 h per sample

## Limitations

- ❌ Gene presence ≠ resistance phenotype — silent gene 가능. AST 검증 권장
- ⚠ Novel ARG 검출 한계 — DeepARG 일부 보완
- ⚠ Read-based vs assembly-based — short ORF 손실 가능
- ⚠ Plasmid context는 short-read에서 불완전 — long-read 권장

## References

- Alcock BP, et al. CARD 2023: expanded curation, support for machine learning, and resistome prediction at the Comprehensive Antibiotic Resistance Database. *NAR* 2023.
- Feldgarden M, et al. AMRFinderPlus and the Reference Gene Catalog. *Sci Rep* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
