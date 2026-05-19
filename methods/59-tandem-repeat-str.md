# Tandem Repeat / STR Genotyping

**Category**: Genomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Short tandem repeat (STR) 및 variable number tandem repeat (VNTR) 변이 검출 — Huntington (CAG), 척수소뇌실조 (SCA), Fragile X (CGG) 등 repeat expansion 질환 진단의 핵심.

**누가 의뢰**: 임상 신경유전학 (HD, ALS, SCA), forensic STR profiling, 인구유전 VNTR.

## Input

- Short-read WGS 30× 또는 long-read 20× HiFi
- (임상) targeted panel for known disease loci

## Pipeline / Tools

- **TRGT** (PacBio, long-read 권장) — gold standard
- **ExpansionHunter** (Illumina, known loci)
- **STRetch / GangSTR / STRiver** — discovery
- **STRipy** — visualization
- Long-read: HiFi + TRGT enables previously inaccessible loci

## Output

- Genotype per locus (allele lengths), expansion call, IGV-style plot

## Limitations

- ❌ Short-read는 large expansion 검출 한계 (read length 초과)
- ⚠ Reference 종/loci 정의 의존
- ⚠ Mosaic expansion 어려움 (single allele 가정)

## References

- Dolzhenko E, et al. ExpansionHunter. *Genome Res* 2019.
- English AC, et al. TRGT for tandem repeats. *Nat Biotechnol* 2024.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
