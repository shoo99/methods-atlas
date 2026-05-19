# Olink / SomaScan / Antibody Arrays (Targeted Proteome)

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

**Olink PEA** (Proximity Extension Assay) / **SomaScan** (aptamer) / **Luminex** — 항체/aptamer 기반 targeted proteome. MS와 달리 plasma/serum 등 low-abundance 단백질을 sensitivity 높게 측정. 임상 biomarker cohort에 표준.

**누가 의뢰**: 대규모 plasma cohort (UK Biobank Olink Explore, 3072 panel; SomaScan 7K), longitudinal biomarker, multi-disease screening.

## Input

- **Plasma / serum / CSF**: 1-50 µL
- **Sample size**: 100-100,000 (대규모 cohort 효율 우수)
- **Format**: NPX (Olink) 또는 RFU (SomaScan) values
- **Replicate**: bridge sample 필수

## Pipeline

```
NPX/RFU matrix → QC (Limits of Detection, IQR)
  → batch correction (Olink Bridge, ComBat)
  → normalization
  → DE (linear models, limma)
  → ProteomicsDB / UniProt annotation
  → Mendelian randomization / pQTL (genome-paired)
  → Disease prediction (Cox PH, ML)
  → Pathway enrichment (GO, KEGG, STRING)
  → Report
```

| Tool / Resource | Version | Purpose |
|---|---|---|
| **Olink Analyse** | R 3.7 | official analysis package |
| **OlinkR** / Olink Insights | – | – |
| **somascan.tools** | R | SomaScan analysis |
| **OpenStudy** / ProteomicsDB | – | reference |
| **proteAaq** | – | Olink/SomaScan QC |

## Output

- Protein × sample (NPX/RFU), DE results, predictive models (AUC), pQTL associations, pathway enrichments

## Demo / Time

- UK Biobank Olink Explore 3072 (~50k participants, publicly summarized)
- Per-cohort analysis: ~2-4 h

## Limitations

- ❌ **Antibody/aptamer specificity** — cross-reactivity 가능. orthogonal MS 검증 권장
- ❌ Panel-limited — pre-selected targets만 측정
- ⚠ Olink/SomaScan 결과 직접 비교 어려움 (다른 platform)
- ⚠ Batch effect 큼 — bridge sample 필수
- ⚠ pQTL 분석 시 antibody epitope masking effect 고려

## References

- Assarsson E, et al. Homogeneous 96-plex PEA immunoassay exhibiting high sensitivity, specificity, and excellent scalability (Olink PEA). *PLoS One* 2014.
- Gold L, et al. Aptamer-based multiplexed proteomic technology for biomarker discovery (SomaScan). *PLoS One* 2010.
- Sun BB, et al. Plasma proteomic associations with genetics and health in the UK Biobank. *Nature* 2023.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
