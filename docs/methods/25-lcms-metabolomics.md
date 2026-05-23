# Untargeted LC-MS Metabolomics

**Category**: Metabolomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Liquid chromatography + high-resolution MS로 수백~수천 metabolite (small molecule, < 1500 Da) 의 untargeted profiling. 대사 변화, 약물 PD/PK, 환경 노출 (exposome), microbiome 메타볼릭 작용 등 광범위한 응용.

**누가 의뢰**: pharmacokinetics R&D, biomarker (당뇨/혈관 등), nutrition/exposome, fermentation/식품, microbiome metabolome.

## Input

- **Sample type**: serum/plasma, urine, tissue, cell, microbial supernatant
- **Extraction**: methanol/chloroform/water 추출 (Bligh-Dyer 등)
- **MS instrument**: Orbitrap (HRMS), Q-TOF — positive + negative mode 둘 다 권장
- **Format**: `.raw` / `.d` / `.mzML`
- **QC samples**: pooled QC (every 5-10 injections) — drift monitoring
- **Replicate**: ≥6 biological per group

## Pipeline

```
RAW → mzML (ProteoWizard msconvert)
  → XCMS / MZmine / OpenMS (peak detection + alignment)
  → drift correction (LOESS on pooled QC)
  → batch correction (waveICA, ComBat)
  → annotation (MS1 m/z + MS2 spectra vs HMDB/MoNA/METLIN)
  → CAMERA / RAMClustR (adduct/isotope grouping)
  → MetaboAnalyst (stats + enrichment)
  → pathway (mummichog, MetExplore)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Convert | ProteoWizard msconvert | – | open format |
| **XCMS** | R 4.4 | peak detection (CentWave) |
| **MZmine 3** | 3.9 | GUI-friendly alternative |
| **OpenMS** | 3.2 | C++ pipeline |
| Annotate | **Sirius 5** | de novo + database |
| (alt annot) MS-DIAL | 5.4 | – |
| Adduct | CAMERA | R 1.62 | – |
| Stats | **MetaboAnalyst 6.0** | web/R | comprehensive |
| Pathway | mummichog | 2.7 | network-based |
| Library | HMDB, MoNA, METLIN, NIST | – | – |

## Output

- `qc/`:
  - `pooled_qc_drift.png`
  - `tic_chromatograms.png`
  - `cv_distribution.png`
- `results/`:
  - `feature_table.tsv` — feature × sample intensity
  - `feature_annotated.tsv` — putative ID, MS2 match score
  - `de_results.tsv` — fold-change, padj
  - `pathway_enrichment.tsv`
- `figures/`:
  - `pca.png`, `oplsda.png`
  - `volcano_metab.png`
  - `boxplot_top_metabolites.png`
  - `pathway_map.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/lcms-metabolomics:v1.0.0
docker run --rm -v $PWD:/work replisci/lcms-metabolomics:v1.0.0 \
  Rscript run_xcms_metaboanalyst.R
```

## Demo Dataset

- MetaboLights **MTBLS136** (plasma diabetes case-control)
- 또는 **ST000061** (Metabolomics Workbench)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| XCMS (50 samples) | ~2 h | 16 cores | 64 GB |
| Full project incl. annotation | ~6 h | 16 cores | 64 GB |

## Limitations

- ❌ **Annotation은 hypothesis** — MS1 m/z 만으로는 putative. MS2 spectra + MS/MS library + 표준품 매칭 (Level 1 ID) 필요
- ❌ **Isomer 구분 어려움** — 동일 m/z + 유사 MS2. orthogonal separation (chromatography, IM-MS) 필요
- ⚠ **Coverage 한계** — single method로는 metabolome의 20-40% 정도. multi-platform 권장 (RP + HILIC, pos + neg)
- ⚠ **Batch effect 큼** — 측정 순서, column condition, 시약. QC-based 보정 필수
- ⚠ **Concentration 정량 불가** — 절대 정량은 isotope-labeled internal standard + 검량선 필요
- ⚠ **Adduct/isotope 군집** — 동일 분자에서 여러 feature. CAMERA 등으로 그룹핑 필요

## Quality Checks

- [x] Pooled QC injection RSD < 30% on > 80% features (FDA guideline)
- [x] PCA: QC tight cluster
- [x] Peak count consistency across samples
- [x] Known internal standard recovery
- [x] Pathway enrichment biologically plausible

## References

- Smith CA, et al. XCMS: processing mass spectrometry data for metabolite profiling. *Anal Chem* 2006.
- Pang Z, et al. MetaboAnalyst 5.0: narrowing the gap between raw spectra and functional insights. *NAR* 2021.
- Dührkop K, et al. SIRIUS 4: a rapid tool for turning tandem mass spectra into metabolite structure information. *Nat Methods* 2019.
- Li S, et al. Predicting Network Activity from High Throughput Metabolomics (mummichog). *PLoS Comput Biol* 2013.

**Reporting standards**
- Sumner LW, et al. Proposed minimum reporting standards for chemical analysis (Metabolomics Standards Initiative). *Metabolomics* 2007.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
