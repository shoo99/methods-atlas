# Stable Isotope Tracing (¹³C MFA)

**Category**: Metabolomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

¹³C / ²H / ¹⁵N-labeled substrate (e.g., [U-¹³C]-glucose, glutamine) 를 세포/조직에 공급 → metabolite의 isotope incorporation pattern 측정 → **metabolic flux** (TCA, glycolysis, anaplerosis) 정량.

**누가 의뢰**: 종양 metabolic reprogramming, 면역세포 metabolism, drug metabolic effect.

## Input

- ¹³C-labeled substrate-treated sample
- Time-course quenching → extract → LC-MS/GC-MS or NMR

## Pipeline / Tools

- **INCA** — isotopomer network flux analysis
- **mfapy** — Python MFA
- **OpenFLUX2**
- **MAVEN / X¹³CMS** — LC-MS isotope analysis

## Output

- Isotopologue distribution, calculated fluxes (mmol/g DCW/h), pathway activity map

## Limitations

- ❌ Steady-state assumption (or modeling time-resolved)
- ❌ Compartmentalization (cytosol vs mitochondria) ambiguous
- ⚠ Cost of labeled substrate

## References

- Antoniewicz MR. A guide to metabolic flux analysis in metabolic engineering. *Metab Eng* 2021.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
