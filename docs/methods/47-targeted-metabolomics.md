# Targeted LC-MS / MRM Metabolomics

**Category**: Metabolomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

알려진 metabolite (panel of 50-500) 의 **Multiple Reaction Monitoring (MRM)** 또는 PRM — high sensitivity, 절대 정량 (isotope-labeled internal standards). 임상 routine, 약물 PK 표준.

**누가 의뢰**: drug PK/PD, 임상 진단 panel (acylcarnitine, amino acid for inborn errors of metabolism), 영양 (vitamin/lipid panel), 위치-특이 metabolic flux.

## Input

- **Sample**: serum/plasma/tissue/cell
- **Internal standards**: isotope-labeled (¹³C, ²H, ¹⁵N) per target
- **MS**: triple-quadrupole (QqQ) — Sciex 6500, Waters TQ-XS
- **Sample size**: 20-1000 (clinical cohort)

## Pipeline

```
Sample → extract → derivatize (if needed, e.g., AccQ-Tag amino acid)
  → LC + QqQ MRM → Skyline (peak integration)
  → calibration curve → absolute quantification
  → MetaboAnalyst (univariate + multivariate)
  → clinical correlation → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **Skyline** | 23 | targeted MS gold standard |
| **MultiQuant** | – | Sciex commercial |
| **MetaboAnalyst** | 6.0 | downstream stats |
| **TargetLynx** | – | Waters commercial |

## Output

- Concentration matrix (µM), QC pass/fail, pathway/clinical interpretation

## Demo / Time

- Biocrates MxP Quant 500 standardized panel
- ~10 min / sample MRM acquisition

## Limitations

- ❌ Pre-selected targets만 — discovery 한계 (untargeted 보완)
- ⚠ Matrix effect — extraction efficiency 검증 필수
- ⚠ Internal standard 가용성 — 모든 metabolite에 isotope-label 존재 X
- ⚠ Dynamic range 한계 (1000-fold ~ 10000-fold)

## References

- MacLean B, et al. Skyline: an open source document editor for creating and analyzing targeted proteomics experiments. *Bioinformatics* 2010.
- Biocrates MxP Quant 500: standardized 630-metabolite panel.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
