# DDA Shotgun Proteomics (Mass Spectrometry)

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Data-Dependent Acquisition (DDA) LC-MS/MS — 가장 표준적인 shotgun proteomics. 단백질 enzyme digest → peptide separation → MS1 survey + MS2 fragmentation. 수천 단백질 정량.

**누가 의뢰**: 약물 작용기전 (drug target identification), biomarker 발견, 호스트-병원체 상호작용, 분비단백체 (secretome).

## Input

- **Sample prep**: protein extract → tryptic digest → desalting → LC-MS
- **MS instrument**: Orbitrap (Exploris, Eclipse), timsTOF, Q-TOF
- **Format**: `.raw` (Thermo), `.d` (Bruker), `.mzML` (convert)
- **Replicate**: ≥3 biological per group
- **Loading**: 100-500 ng peptide / injection

## Pipeline

```
RAW → MaxQuant / FragPipe (MSFragger) — peptide ID + quant (LFQ)
  → protein group assembly → razor peptide
  → MaxLFQ / iBAQ / IceR (intensity quantification)
  → Perseus (filter, impute) or limma/MSstats (R)
  → DEqMS / limma (differential)
  → annotation: UniProt + ProteomicsDB
  → enrichment (GSEA, STRING) → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MaxQuant** | 2.6 | gold standard, free |
| **FragPipe + MSFragger** | 21.1 | fast, modern |
| (alt) Spectronaut | – | commercial, library-free |
| (alt) Proteome Discoverer | – | Thermo commercial |
| **Perseus** | 2.0 | post-MaxQuant analysis |
| **limma** | R/Bioc | linear models |
| **MSstats** | R/Bioc 4.4 | proteomics-specific stats |
| **DEqMS** | R/Bioc 1.24 | better variance estimation |
| Annotation | UniProt, STRING | – | functional |

## Output

- `results/`:
  - `proteinGroups.tsv` — MaxQuant output
  - `lfq_intensity.tsv` — protein × sample
  - `de_results.tsv` — log2FC, padj
  - `enrichment.tsv` — GO, KEGG, Reactome
- `figures/`:
  - `pca.png`, `volcano.png`, `heatmap_top.png`
  - `coverage_histogram.png` — % sequence coverage
  - `id_per_sample.png` — protein/peptide counts
  - `cv_distribution.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/dda-proteomics:v1.0.0
docker run --rm -v $PWD:/work replisci/dda-proteomics:v1.0.0 \
  bash run_pipeline.sh
```

## Demo Dataset

- PRIDE **PXD000561** (HEK293 deep proteome)
- 또는 PXD007280 (well-characterized cancer cell line)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| MaxQuant single run | ~1 h | 16 cores | 32 GB |
| Cohort (24 samples) | ~24 h | 32 cores | 64 GB |

## Limitations

- ❌ **Stochastic sampling** — DDA는 abundant peptide 위주 선택. low-abundance 단백질 missing 많음. DIA 권장
- ❌ **Missing values 많음** — sample 간 missing pattern 차이. imputation 필요
- ⚠ **Razor peptide ambiguity** — protein group은 unique peptide 부족시 ambiguous
- ⚠ **Sample prep variability** — lysis/digestion 효율 변동. spike-in standards 권장
- ⚠ **PTM 누락** — phospho/ubiq 등은 enrichment 필요 (별도 workflow)
- ⚠ **MS contamination** — keratin, BSA 등 contaminant 필터 필요

## Quality Checks

- [x] PSM / peptide / protein FDR < 1%
- [x] Missed cleavage < 2 per peptide
- [x] % sequence coverage median > 30%
- [x] Replicate CV < 20%
- [x] PCA: replicate cluster
- [x] Known marker proteins detected

## References

- Cox J, Mann M. MaxQuant enables high peptide identification rates, individualized p.p.b.-range mass accuracies and proteome-wide protein quantification. *Nat Biotechnol* 2008.
- Kong AT, et al. MSFragger: ultrafast and comprehensive peptide identification in mass spectrometry-based proteomics. *Nat Methods* 2017.
- Choi M, et al. MSstats: an R package for statistical analysis of quantitative mass spectrometry-based proteomic experiments. *Bioinformatics* 2014.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
