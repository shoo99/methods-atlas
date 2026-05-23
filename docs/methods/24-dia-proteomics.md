# DIA-MS Proteomics (Data-Independent Acquisition)

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Data-Independent Acquisition — MS가 모든 m/z window를 동시 fragment. DDA의 stochastic sampling 문제 해결, **reproducibility 우수**, missing value 적음, deep proteome (8000+ proteins).

대규모 clinical cohort proteomics의 새로운 표준 (Olink antibody와 함께).

**누가 의뢰**: clinical biomarker discovery (수백 sample), longitudinal/time-course, plasma proteome.

## Input

- **Sample prep**: 동일 (digest → desalting), 또는 SP3/iST quick prep
- **MS**: SWATH (Sciex), thermo Exploris/Eclipse DIA, timsTOF diaPASEF
- **Format**: `.raw`/`.d`/`.mzML`
- **Library**: spectral library (pre-built) 또는 **library-free** (DIA-NN, Spectronaut directDIA)
- **Replicate**: ≥3

## Pipeline

```
RAW + spectral library (or library-free)
  → DIA-NN / Spectronaut → peptide/protein quant matrix
  → R analysis: limma + MSstats + DEqMS
  → batch correction (ComBat / limma::removeBatchEffect)
  → DE → enrichment (GSEA) → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **DIA-NN** | 1.9 | free, fast, library-free (deep learning) |
| **Spectronaut** | 19 | commercial, gold-standard accuracy |
| (alt) MaxDIA (MaxQuant) | 2.6 | open-source DIA |
| **EncyclopeDIA** | – | chromatogram library workflow |
| Stats | MSstats, limma | – | – |
| Imputation | missForest, DreamAI | – | proteomics-aware |

## Output

- `results/`:
  - `precursor_report.tsv` (DIA-NN)
  - `protein_matrix.tsv` — protein × sample LFQ
  - `de_results.tsv`
  - `peptide_quant.tsv` (optional)
- `figures/`:
  - `qc_consistency.png` — CV vs intensity
  - `pca.png`, `volcano.png`, `heatmap.png`
  - `missing_value_pattern.png`
  - `protein_id_per_sample.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/dia-proteomics:v1.0.0
docker run --rm -v $PWD:/work replisci/dia-proteomics:v1.0.0 \
  diann --f sample.raw --lib spectral_library.tsv --out report.tsv
```

## Demo Dataset

- PRIDE **PXD028735** (DIA HeLa benchmark)
- 또는 Plasma DIA (Bruderer et al.)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| DIA-NN single run | ~30 min | 16 cores | 32 GB |
| 100-sample cohort | ~24 h | 32 cores | 128 GB |

## Limitations

- ❌ **Co-isolation interference** — wide MS2 window. very-deep proteome에서 false co-elution
- ⚠ **Library 의존 (구식)** — modern library-free (DIA-NN, directDIA) 가 표준
- ⚠ **PTM 분석 까다로움** — fragment 모호성. specialized search 필요
- ⚠ **Calibration sensitive** — RT, m/z drift. internal standard 권장
- ⚠ **Quantification mode 선택** — MaxLFQ vs xic-based 결과 다름. 일관성 필요

## Quality Checks

- [x] # precursors > 50,000 (HeLa standard)
- [x] # proteins > 5,000 (deep), > 8,000 (very deep)
- [x] Replicate Pearson > 0.95
- [x] CV < 10% (technical), < 30% (biological)
- [x] Missing rate < 30% (DIA 강점)

## References

- Demichev V, et al. DIA-NN: neural networks and interference correction enable deep proteome coverage in high throughput. *Nat Methods* 2020.
- Bruderer R, et al. Optimization of experimental parameters in data-independent mass spectrometry significantly increases depth and reproducibility of results. *Mol Cell Proteomics* 2017 (Spectronaut).
- Meier F, et al. diaPASEF: parallel accumulation–serial fragmentation combined with data-independent acquisition. *Nat Methods* 2020.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
