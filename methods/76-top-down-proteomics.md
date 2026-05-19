# Top-down Proteomics

**Category**: Proteomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Trypsin digest 없이 **whole intact protein** 을 MS로 분석 — proteoform (PTM 조합, splice isoform, sequence variant) 의 전체 상태 보존. Bottom-up 의 peptide-level loss 회피.

**누가 의뢰**: histone proteoform, antibody/biologics 분석, hemoglobin variants, intact MAb.

## Input

- Intact protein extract (denatured or native)
- High-resolution FTMS (Orbitrap Eclipse, FT-ICR)

## Pipeline / Tools

- **TopPIC** — proteoform identification
- **MASH Suite**
- **ProSight** — Northwestern flagship tool
- **mMass / TopFD** — deconvolution

## Output

- Proteoform mass + sequence + PTM combinations, isotope deconvolution

## Limitations

- ❌ Protein size limited (~30 kDa typical)
- ❌ Lower throughput
- ⚠ Sample prep + LC challenging (precipitation)

## References

- Kelleher NL. Top-down proteomics. *Anal Chem* 2004.
- Smith LM, et al. Proteoforms as the next proteomics currency. *Science* 2018.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
