# Pangenome Graph Construction

**Category**: Genomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

여러 게놈을 graph 구조로 통합 (HPRC 47 phased haplotypes) — reference bias 제거, SV 검출 향상, 종/품종 다양성 표현.

**누가 의뢰**: 인구특이 reference 구축 (한국인 1K), 작물 pangenome, 인구다양성 SV.

## Input

- ≥10 high-quality assemblies (HiFi recommended)
- Same species / closely related

## Pipeline / Tools

- **PGGB** (PanGenome Graph Builder) — wfmash + seqwish + smoothxg
- **minigraph-cactus** — VG + cactus alignment
- **VG (Variation Graph)** — variant calling on graph
- **Pangene** — gene-level graph

## Output

- GFA graph file, pangenome variants (VCF), per-haplotype paths, gene-level analysis

## Limitations

- ❌ Computational cost 높음 (TB RAM, days)
- ⚠ Tool maturity 발전 중
- ⚠ Variant calling against graph reference 새 standard 부재

## References

- Garrison E, et al. Building pangenome graphs (PGGB). *bioRxiv* 2023.
- Liao WW, et al. A draft human pangenome reference (HPRC). *Nature* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
