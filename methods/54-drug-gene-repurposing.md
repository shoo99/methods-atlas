# Drug–Gene Interaction & Repurposing

**Category**: Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

유전자 변이/발현 signature 으로 **drug-gene interaction DB** 매칭 (DGIdb, OncoKB, DrugBank) — actionable target 추출 — 또는 **transcriptomic signature reversal** (CMap/LINCS L1000) 으로 drug repurposing 후보 도출.

**누가 의뢰**: 종양 mutation-driven 약물 선정 (precision oncology), 희귀질환 repurposing, COVID-19 등 emerging disease 신속 후보 발굴.

## Input

- **Gene list / signature** (DE results, mutated genes)
- 또는 **transcriptomic profile** (gene × condition log2FC) — CMap query

## Pipeline

```
[Drug-gene matching]
Gene list / variants → DGIdb / OncoKB / DrugBank API
  → actionable variant → drug list + evidence level

[Signature repurposing]
DE signature → LINCS L1000 / CMap query
  → connectivity score → reverse-correlated drugs (anti-signature)
  → tissue/cell-line specificity filter
  → mechanism enrichment
→ Report
```

| Tool / Resource | Version | Purpose |
|---|---|---|
| **DGIdb** | 5.0 | drug-gene interaction |
| **OncoKB** | API | clinical actionability |
| **DrugBank** | 5.1 | drug DB |
| **LINCS L1000 / clue.io** | – | signature reversal |
| **CLUE Connectivity Map** | – | tau score |
| **Open Targets** | – | target-disease + drug evidence |
| **CIVIC** | – | curated cancer variants |

## Output

- Actionable variants → drug recommendations, repurposing candidate list with connectivity scores, mechanism annotation

## Demo / Time

- TCGA mutation → OncoKB matched drugs
- ~30 min – 2 h

## Limitations

- ❌ Evidence level 다양 — Level 1 (FDA approved) vs Level 4 (preclinical) 명시 필수
- ⚠ Signature reversal 가설 — clinical validation 별도
- ⚠ Cell-line의 L1000 → patient extrapolation 제한
- ⚠ Drug 다중 target / off-target 정보 누락 가능

## References

- Cotto KC, et al. DGIdb 5.0: rebuilding the drug-gene interaction database. *NAR* 2024.
- Chakravarty D, et al. OncoKB: A Precision Oncology Knowledge Base. *JCO Precis Oncol* 2017.
- Subramanian A, et al. A Next Generation Connectivity Map: L1000 Platform and the First 1,000,000 Profiles. *Cell* 2017.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
