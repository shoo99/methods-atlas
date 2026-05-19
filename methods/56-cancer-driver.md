# Cancer Driver Gene Detection

**Category**: Functional / Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

종양 cohort 의 somatic mutation 패턴에서 **driver gene** (positive selection 의 표적) 검출. Background mutation rate 보정 후 비유전적 enrichment 통계로 식별. precision oncology의 핵심.

**누가 의뢰**: 종양 cohort sequencing → driver landscape, 신규 cancer-driver discovery, 임상 actionable list 작성.

## Input

- **Somatic mutation calls**: MAF / VCF (tumor-normal pair)
- **Cohort**: ≥50 samples 권장 (검정력)
- **Tumor type**: 동일 cancer type (cross-cancer는 별도)

## Pipeline

```
Somatic MAF
  ├─ MutSigCV / MutSig2CV — background-aware mutation rate
  ├─ dNdScv — dN/dS (positive selection)
  ├─ OncodriveCLUSTL — mutation clustering in protein
  ├─ OncodriveFML — functional impact
  ├─ HotMAPS / 3D — structural hotspot
  ↓
Consensus drivers + functional annotation (OncoKB, COSMIC, Cancer Gene Census)
Oncoplot / driver landscape → cohort 해석 → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MutSig2CV** | – | gold standard |
| **dNdScv** | R 0.1 | Sanger method |
| **OncodriveCLUSTL** | 1.1 | clustering |
| **OncodriveFML** | 2.4 | functional |
| **HotMAPS** | – | 3D hotspot |
| **maftools** | R/Bioc 2.22 | viz + downstream |
| **OncoKB / Cancer Gene Census** | – | annotation |

## Output

- Driver gene list with FDR, oncoplot, mutation hotspots (3D structure 가능), pathway-level driver summary

## Demo / Time

- TCGA-LUAD MAF
- ~2-6 h per cohort

## Limitations

- ❌ Sample size 의존 — n<30은 검정력 부족
- ⚠ Background rate 모델 의존 — TMB 차이 큰 cohort 보정 필요
- ⚠ Non-coding driver는 별도 (NCDriver, OncodriveFML-nc)
- ⚠ Driver ≠ therapeutic target — functional validation 필요

## References

- Lawrence MS, et al. Mutational heterogeneity in cancer and the search for new cancer-associated genes (MutSigCV). *Nature* 2013.
- Martincorena I, et al. Universal Patterns of Selection in Cancer and Somatic Tissues (dNdScv). *Cell* 2017.
- Sondka Z, et al. The COSMIC Cancer Gene Census: describing genetic dysfunction across all human cancers. *Nat Rev Cancer* 2018.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
