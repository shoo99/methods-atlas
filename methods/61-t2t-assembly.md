# Telomere-to-Telomere (T2T) Assembly

**Category**: Genomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Gap-free, telomere-to-telomere chromosome assembly. T2T-CHM13 (Nurk 2022) 이후 인간 게놈도 가능. Centromere, ribosomal DNA, segmental duplication 등 long-time inaccessible 영역 포함.

**누가 의뢰**: 고품질 reference 구축 (인구특이/품종특이), 작물/축산 ultra-quality, centromere/repeat 영역 분석.

## Input

- PacBio HiFi 30-50× + ONT ultra-long (100kb+) 30×
- Hi-C scaffolding
- Optional: BioNano optical mapping

## Pipeline / Tools

- **hifiasm (trio/Hi-C mode)** — primary assembly
- **Verkko** (NIH official T2T pipeline)
- **NextDenovo / Canu** — alternative
- Polishing: **DeepConsensus**, **Medaka**, **DeepVariant** (consensus)
- Manual curation: **gEVAL**, **PretextView**

## Output

- Gapless chromosome FASTA, BUSCO 100%, QV > 60

## Limitations

- ❌ 수동 큐레이션 필요 — fully automated 어려움
- ❌ Cost — HiFi + ONT ultra-long 동시 비싸
- ⚠ Highly heterozygous diploid는 phased dual-haplotype 권장

## References

- Nurk S, et al. The complete sequence of a human genome (T2T-CHM13). *Science* 2022.
- Rautiainen M, et al. Telomere-to-telomere assembly of diploid chromosomes with Verkko. *Nat Biotechnol* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
