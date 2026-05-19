# TMT / iTRAQ Multiplexed Quantitative Proteomics

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Isobaric tagging — 동일 mass / 다른 reporter ion 으로 **6-18 samples 한 번에** quantitative MS. Channel 간 quant 정확도 ↑, missing value 적음 (vs LFQ), 대규모 cohort에 효율적.

**누가 의뢰**: 임상 cohort proteomics (CPTAC), drug perturbation panel, kinetic time-course, multi-condition comparison.

## Input

- **Reagent**: TMT 10/11/16/18-plex (Thermo), iTRAQ 4/8-plex (Sciex)
- **Sample**: 100 µg / channel (TMT16: 1.6 mg total)
- **Bridge sample 필수** — multi-set normalization
- **MS**: MS3 권장 (SPS-MS3, Orbitrap Eclipse/Exploris) — interference 감소

## Pipeline

```
Lysis → tryptic digest → TMT labeling → pool 16 channels
  → fractionation (HpRP) → LC-MS3
  → MaxQuant / FragPipe (TMT search)
  → IsobarQuant / DEqMS / MSstatsTMT (stats)
  → batch correction across TMT sets
  → DE + pathway → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MaxQuant** (TMT mode) | 2.6 | – |
| **FragPipe TMT-Integrator** | 21.1 | open-source TMT |
| **MSstatsTMT** | R 2.14 | stats with normalization |
| **DEqMS** | R 1.24 | variance shrinkage |
| **IsobarQuant** | – | EMBL pipeline |

## Output

- Protein × sample log-ratio, DE results, bridge-normalized cross-set, batch QC

## Demo / Time

- CPTAC TMT11 lung cancer dataset
- ~24 h per TMT16 set

## Limitations

- ❌ **Ratio compression** (co-isolation) — MS3 mitigates but not eliminates
- ⚠ Bridge sample → cross-set comparison 정확도 의존
- ⚠ Reagent batch 차이 — same lot 권장
- ⚠ Limited dynamic range vs LFQ for very low abundance
- ⚠ Cost — reagent 비용 sample당 $$$

## References

- Thompson A, et al. Tandem mass tags: a novel quantification strategy for comparative analysis of complex protein mixtures by MS/MS. *Anal Chem* 2003.
- McAlister GC, et al. MultiNotch MS3 enables accurate, sensitive, and multiplexed detection of differential expression across cancer cell line proteomes. *Anal Chem* 2014.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
