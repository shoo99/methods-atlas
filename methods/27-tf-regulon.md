# TF Regulon / Gene Regulatory Network Inference

**Category**: Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Transcription factor (TF) → target gene regulatory network 추론. **DoRothEA** (manually curated), **SCENIC** (motif + co-expression), **Lisa** (ChIP enrichment) 등. TF activity 추론으로 발현 변화를 transcriptional regulator 수준에서 해석.

**누가 의뢰**: 약물의 TF-driven program 평가, lineage TF discovery (개발/암), drug repurposing via TF activity.

## Input

- **Bulk or single-cell RNA-seq** — normalized expression
- **Ranked gene list** 또는 **DE result** (TF activity inference용)
- **Optional**: scATAC-seq (Lisa/SCENIC+), ChIP-seq (refinement)
- **Species**: 인간/생쥐 reference DB 가장 풍부

## Pipeline

```
Expression / DE
  ├─ DoRothEA → VIPER (TF activity score)
  ├─ SCENIC (single-cell) — GENIE3/GRNBoost + RcisTarget + AUCell
  ├─ pySCENIC (Python implementation)
  ├─ SCENIC+ (multi-omic with scATAC)
  ├─ Lisa — ChIP enrichment-based
  ↓
TF activity matrix → cluster/condition comparison
TF-target validation (motif scanning, ChIP overlay)
Drug repurposing (CMap/LINCS) — TF signature matching
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **DoRothEA** | R/Bioc 1.18 + decoupleR 2.10 | bulk + single-cell TF activity |
| **VIPER** | R/Bioc 1.40 | aREA algorithm |
| **SCENIC** | R 1.3 | single-cell GRN |
| **pySCENIC** | 0.12 | Python, faster |
| **SCENIC+** | 1.0 | scRNA + scATAC integration |
| **Lisa** | 2.3 | ChIP enrichment-based TF inference |
| **decoupleR** | R/Python 2.10 | multi-method wrapper |
| **CollecTRI** | – | updated TF-target prior |

## Output

- `results/`:
  - `tf_activity_matrix.tsv` — TF × sample/cell
  - `tf_target_network.tsv` — edge list
  - `differential_tf_activity.tsv`
  - `regulon_auc.tsv` (SCENIC)
- `figures/`:
  - `tf_heatmap.png`
  - `tf_volcano.png`
  - `top_regulon_umap.png` (single-cell)
  - `tf_network.png` — Cytoscape-style
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/tf-regulon:v1.0.0
docker run --rm -v $PWD:/work replisci/tf-regulon:v1.0.0 \
  Rscript run_dorothea_decoupler.R expr.tsv
```

## Demo Dataset

- TCGA-BRCA — known TF drivers (FOXA1, GATA3, ESR1)
- 10x PBMC scRNA — immune lineage TFs (TBX21, GATA3, FOXP3, EBF1, PAX5)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| DoRothEA bulk (cohort) | ~10 min | 4 cores | 16 GB |
| pySCENIC scRNA (10k cells) | ~6 h | 32 cores | 64 GB |
| SCENIC+ (scRNA + scATAC) | ~12 h | 32 cores | 128 GB |

## Limitations

- ❌ **TF activity ≠ TF expression** — 결과 score는 *추론*. 직접 phosphorylation/binding 측정 ≠ inference
- ❌ **Prior network 의존** — DoRothEA/SCENIC은 인간/생쥐 우세. 비-모델 종 제한적
- ⚠ **Co-expression confounding** — GENIE3는 indirect 관계도 포함. ChIP/ATAC 검증 권장
- ⚠ **scRNA SCENIC stochasticity** — random seed에 따라 결과 변동. multiple run + consensus 권장
- ⚠ **Cohort 크기** — bulk는 ≥20 sample 권장
- ⚠ **Tissue specificity** — generic prior로는 lineage-specific TF 누락 가능

## Quality Checks

- [x] Known lineage TF (e.g., FOXP3 for Treg, PAX5 for B) detected
- [x] TF activity distribution reasonable (not all zero)
- [x] Multi-method consensus (DoRothEA + SCENIC) for top hits
- [x] Motif scan support for top edges
- [x] (scATAC available) → TF motif enrichment matches

## References

- Garcia-Alonso L, et al. Benchmark and integration of resources for the estimation of human transcription factor activities (DoRothEA). *Genome Res* 2019.
- Aibar S, et al. SCENIC: single-cell regulatory network inference and clustering. *Nat Methods* 2017.
- Bravo González-Blas C, et al. SCENIC+: single-cell multiomic inference of enhancers and gene regulatory networks. *Nat Methods* 2023.
- Müller-Dott S, et al. Expanding the coverage of regulons from high-confidence prior knowledge for accurate estimation of transcription factor activities (CollecTRI). *NAR* 2023.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
