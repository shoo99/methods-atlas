# Metatranscriptomics

**Category**: Microbiome
**Tier**: 3
**Status**: 📝 1-pager

## Overview

미생물 community RNA-seq — **누가 살고 있는가** (16S/shotgun DNA) → **무엇을 발현하는가** (RNA). 환경/host stress 반응, 기능 활성도 측정.

**누가 의뢰**: gut microbiome functional activity (염증 condition), 발효 활성 profile, 토양/해양 community 기능.

## Input

- rRNA-depleted total RNA from environmental/microbiome sample
- Read depth: 20-50 GB / sample (high microbial diversity)

## Pipeline / Tools

- **SqueezeMeta** — automated metatranscriptomic pipeline
- **HUMAnN3** — pathway abundance + expression
- **SAMSA2** — annotation-focused
- **Read mapping vs assembled metagenome** (paired)

## Output

- Active gene/pathway profile, expressed taxa, transcript-level functional diversity

## Limitations

- ❌ rRNA depletion variable (host + microbial)
- ⚠ Reference-DB dependency
- ⚠ Active vs dormant distinction (RNA degradation rate variable)
- ⚠ DNA + RNA paired analysis 권장

## References

- Tamames J, et al. SqueezeMeta, A Highly Portable, Fully Automatic Metagenomic Analysis Pipeline. *Front Microbiol* 2018.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
