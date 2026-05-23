# Lipidomics

**Category**: Metabolomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

LC-MS 또는 shotgun-MS로 **수천 lipid species** 동시 정량 — class (PC, PE, PS, PI, SM, Cer, TAG, DAG, FA) × acyl chain 다양성. Membrane composition, signaling lipid, energy storage, biomarker.

**누가 의뢰**: NAFLD/MAFLD 간질환, 심혈관 (oxidized lipid), 신경 (sphingolipid), 종양 (lipogenesis), nutrition study.

## Input

- **Sample**: serum/plasma, tissue, cell — modified Bligh-Dyer 또는 MTBE 추출
- **MS**: Orbitrap HRMS (RP + HILIC) or shotgun (lipid class-resolved)
- **Internal standards**: SPLASH LIPIDOMIX (Avanti)
- **Sample size**: 10-1000

## Pipeline

```
Lipid extract → LC-MS (pos + neg) or shotgun-MS
  → LipidSearch / LipidMatch / MS-DIAL — lipid ID
  → adduct/isotope filtering
  → MetaboAnalyst / LipidR / iSALSA — stats
  → lipid class pathway (LIPID MAPS)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **LipidSearch** | 5.0 | Thermo commercial |
| **MS-DIAL** | 5.4 | open-source, lipidomics support |
| **LipidMatch** | 4.0 | open-source match |
| **lipidr** | R/Bioc 2.20 | R stats package |
| **Lipid Data Analyzer (LDA)** | – | open |
| **LIPID MAPS Lipidomics Gateway** | – | nomenclature + DB |

## Output

- Lipid × sample intensity, class-level summary, DE, pathway, structural classification

## Demo / Time

- Metabolomics Workbench ST001065 NAFLD lipidomics
- ~3 h per sample

## Limitations

- ❌ Isomer 구별 어려움 — sn-position, double bond geometry
- ⚠ Annotation incomplete — novel lipid 다수
- ⚠ Calibration이 lipid class별로 다름 — class-specific 권장
- ⚠ Oxidized lipid 검출 specialized — separate workflow

## References

- Yamada T, et al. Comprehensive lipid analysis by MS-DIAL 4. *Nat Methods* 2020.
- LIPID MAPS Consortium: https://www.lipidmaps.org/

---

**Lead**: Replisci
**Last updated**: 2026-05-19
