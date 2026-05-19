# WGCNA — Weighted Gene Co-expression Network Analysis

**Category**: Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

유전자 발현 상관성을 바탕으로 **co-expression modules** (cluster of co-regulated genes) 를 식별하고 임상 trait 와 연관 분석. Module eigengene → 핵심 hub gene 발견. Bulk transcriptome cohort 분석의 핵심 도구.

**누가 의뢰**: 대규모 cohort RNA-seq (TCGA, GTEx 활용 또는 자체 cohort), trait-gene 연관, 약물 반응 module 분석.

## Input

- **Expression matrix**: gene × sample (normalized, e.g., VST/TMM/log2CPM)
- **Sample size**: **≥20 samples** (50+ 권장)
- **Trait metadata**: continuous or binary (age, BMI, survival, treatment response)
- **(Optional) cohort**: TCGA, GTEx, GEO 등 공개 데이터

## Pipeline

```
Expression matrix
  → soft thresholding power selection (scale-free topology)
  → topological overlap matrix (TOM)
  → dynamic tree cut → modules (colored)
  → module eigengene (ME) computation
  → trait correlation: ME × trait
  → hub gene identification (kME, kIM)
  → enrichment per module (GO, KEGG)
  → preservation analysis (validation cohort)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **WGCNA** | R/Bioc 1.73 | classic |
| **hdWGCNA** | R 0.4 | high-dimensional + single-cell |
| **CEMiTool** | R/Bioc 1.30 | automated WGCNA + report |
| **MEGENA** | R 1.3 | multi-scale modules |
| **PandaR/LIONESS** | R | single-sample networks |
| Visualization | Cytoscape, ggplot2 | – | – |

## Output

- `results/`:
  - `module_membership.tsv` — gene, module color, kME
  - `module_eigengenes.tsv` — ME × sample
  - `trait_correlation.tsv` — ME × trait correlation + p
  - `hub_genes_per_module.tsv` — top kME genes
  - `module_enrichment.tsv`
- `figures/`:
  - `power_scale_free.png` — soft threshold selection
  - `dendrogram_modules.png` — clustering tree
  - `module_trait_heatmap.png` — central output
  - `kME_distribution.png`
  - `hub_network.png` — per module
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/wgcna:v1.0.0
docker run --rm -v $PWD:/work replisci/wgcna:v1.0.0 \
  Rscript run_wgcna.R expr.tsv traits.tsv
```

## Demo Dataset

- TCGA-BRCA RNA-seq (n>1000) — Module-survival association
- Or female mouse liver (Langfelder & Horvath classic data)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| ~5,000 genes × 50 samples | ~30 min | 4 cores | 16 GB |
| ~20,000 genes × 500 samples | ~4 h | 16 cores | 64 GB |
| TCGA scale (20k × 1000+) | ~12 h | 32 cores | 256 GB |

## Limitations

- ❌ **Correlation ≠ causation** — module은 co-regulation 가설일 뿐
- ❌ **Sample size 부족** — 20 미만은 module 안정성 낮음. preservation analysis 어려움
- ⚠ **Soft threshold 선택** — scale-free fit이 정답 아닐 수 있음. heuristic
- ⚠ **Single-cell 직접 적용 X** — dropout/sparsity로 부적합. hdWGCNA pseudobulk 권장
- ⚠ **Batch effect 큼** — 사전 보정 (ComBat) 필수
- ⚠ **Module size 편향** — large module은 generic, small module은 specific. 해석 주의

## Quality Checks

- [x] Soft threshold scale-free fit R² > 0.8
- [x] Module size median 50-500 (너무 작거나 크면 over/under-fit)
- [x] Module eigengene PC1 variance > 50%
- [x] Trait correlation FDR < 0.05
- [x] Validation cohort module preservation > 0.5

## References

- Langfelder P, Horvath S. WGCNA: an R package for weighted correlation network analysis. *BMC Bioinformatics* 2008.
- Morabito S, et al. hdWGCNA identifies co-expression networks in high-dimensional transcriptomics data. *Cell Reports Methods* 2023.
- Russo PST, et al. CEMiTool: a Bioconductor package for performing comprehensive modular co-expression analyses. *BMC Bioinformatics* 2018.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
