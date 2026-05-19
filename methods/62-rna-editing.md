# RNA Editing (A-to-I, C-to-U)

**Category**: Transcriptomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

ADAR (A→I) 또는 APOBEC (C→U) 효소에 의한 site-specific RNA 변이. 신경계 (Q/R editing of GluR-B), 면역 (innate immune sensing), 종양 (transcriptomic diversity).

**누가 의뢰**: 신경/면역 RNA editing 변화, 종양 hyper-editing.

## Input

- RNA-seq + WGS/WES (DNA reference)
- High coverage 권장 (50M+)

## Pipeline / Tools

- **REDItools 2** — DNA-RNA 비교, site detection
- **JACUSA 2** — replicate-based
- **RES-Scanner**
- DB: REDIportal (가장 큰 editing site catalog)
- Filter: SNP DB (dbSNP) 제거 필수

## Output

- Editing sites with EI (editing index), Alu vs non-Alu, gene context

## Limitations

- ❌ False positive 큼 — SNP, mapping error 구분
- ⚠ Low VAF editing 검출 어려움 — depth 의존
- ⚠ Strand-specific library 필수

## References

- Lo Giudice C, et al. REDItools 2.0. *NAR* 2020.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
