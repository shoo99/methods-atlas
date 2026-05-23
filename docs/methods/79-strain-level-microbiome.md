# Strain-level Microbiome Analysis

**Category**: Microbiome
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Shotgun metagenomics 에서 **species 내 strain 다양성** 분석 — 동일 종 내 SNV/structural variant 패턴, vertical transmission (mother→infant), strain replacement / persistence.

**누가 의뢰**: FMT (fecal microbiota transplantation) 효과 추적, 임상 phenotype과 strain 연관, evolutionary tracking.

## Input

- Shotgun metagenome (deep, 30 GB+ per sample)
- Multi-time-point 권장

## Pipeline / Tools

- **StrainPhlAn** — SNV-based, MetaPhlAn 통합
- **inStrain** — popANI/strain-level metrics
- **PanPhlAn 3** — accessory gene-level
- **mOTUs** — multi-species
- **SNVPhyl / SNVQuery** — clinical SNP phylogenomics

## Output

- Per-species strain calls, SNV profile, strain phylogeny, strain transmission events

## Limitations

- ❌ Depth requirement very high
- ⚠ Reference dependence — known species만
- ⚠ Multi-strain mixture untangling difficult

## References

- Truong DT, et al. Microbial strain-level population structure and genetic diversity from metagenomes (StrainPhlAn). *Genome Res* 2017.
- Olm MR, et al. Consistent metagenome-derived metrics verify and define bacterial species boundaries (inStrain). *Nat Biotechnol* 2021.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
