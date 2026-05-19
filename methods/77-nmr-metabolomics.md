# NMR Metabolomics

**Category**: Metabolomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

¹H-NMR 으로 metabolite quantification — 비파괴, sample 회수, high reproducibility, absolute quantification. MS와 보완 관계 (NMR은 더 정량적, MS는 더 sensitive).

**누가 의뢰**: 식품 fingerprinting, body fluid (urine, plasma), 보전 시료 (NMR은 비파괴).

## Input

- Sample in NMR tube (D₂O buffer, TSP-d4 internal standard)
- 1D ¹H or 2D NMR (J-RES, HSQC)

## Pipeline / Tools

- **Chenomx NMR Suite** — commercial gold standard
- **rNMR / nmrML**
- **MetaboAnalyst** — downstream stats
- **Bayesil / BATMAN** — automated quantification
- **HMDB NMR database**

## Output

- Quantified metabolite list (mM), spectra, statistical comparison

## Limitations

- ❌ Sensitivity lower than MS (mM vs µM-nM)
- ❌ Limited coverage (~100 metabolites typical)
- ⚠ Overlap of peaks in complex mixture
- ⚠ Instrument cost high (cryoprobe NMR)

## References

- Markley JL, et al. The future of NMR-based metabolomics. *Curr Opin Biotechnol* 2017.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
