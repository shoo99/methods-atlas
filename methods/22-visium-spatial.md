# Visium Spatial Transcriptomics

**Category**: Spatial
**Tier**: 2
**Status**: 📝 1-pager

## Overview

조직 절편의 위치 정보 + 약 5,000 spot의 transcriptome 동시 측정. **Tissue architecture**, tumor margin, niche identification 등 공간적 맥락이 중요한 질문에 답한다. Visium HD는 8 µm 해상도로 single-cell 수준 접근.

**누가 의뢰**: 종양 (margin/TME), 뇌과학 (brain region transcriptome), 발달 (organogenesis), 임상 병리 (FFPE 기반 archival sample).

## Input

- **Platform**:
  - **10x Visium v2** (55 µm spots, ~5,000 spots/section, fresh frozen)
  - **Visium FFPE** (probe-based, formalin-fixed)
  - **Visium HD** (8 µm bins, near-single-cell)
- **Tissue**: H&E or IF image + RNA-seq library
- **Format**: FASTQ + tissue image → Space Ranger → filtered_feature_bc_matrix

## Pipeline

```
Space Ranger → spot × gene matrix + image
  → QC (Squidpy/STUtility)
  → Normalize → cluster (Leiden) → annotate spatial domains
  → Deconvolution (cell2location/SpaCET/RCTD) — spot → cell type proportions
  → SVG (spatially variable genes — SpatialDE2/SpaGFT)
  → Niche / domain detection (BayesSpace, GraphST)
  → CCI in spatial context (stLearn, COMMOT)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Count | Space Ranger | 3.0 | 10x official |
| **Squidpy** | 1.6 | Python spatial framework |
| **Seurat (Spatial)** | 5.1 | R alternative |
| **cell2location** | 0.1.4 | reference-based deconvolution |
| (alt) **RCTD** | R 2.2 | spacexr |
| (alt) **SpaCET** | 1.1 | tumor-specific |
| **BayesSpace** | R 1.16 | spatial clustering |
| **GraphST** | 1.1 | graph-based domain |
| **SpatialDE2** | 0.1 | spatially variable genes |
| **stLearn** | 0.4 | spatial CCI |

## Output

- `qc/spatial_qc.html` — spot QC, tissue overlay
- `results/`:
  - `adata_spatial.h5ad`
  - `domains.tsv` — spatial clusters
  - `celltype_proportions.tsv` — deconvolution
  - `svg.tsv` — spatially variable genes
- `figures/`:
  - `spot_overlay.png` — clusters on H&E
  - `celltype_proportion_maps.png`
  - `svg_examples.png`
  - `niche_heatmap.png`
  - `tumor_margin.png` (cancer)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/visium-spatial:v1.0.0
docker run --gpus all --rm -v $PWD:/work replisci/visium-spatial:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- 10x **Visium mouse brain coronal** (public, well-annotated regions)
- 10x **Visium FFPE breast cancer** (with cell2location reference)
- Visium HD: 10x public colon cancer

## Time & Resources

| Stage | Wall-clock | CPU | GPU | RAM |
|-------|-----------|-----|-----|-----|
| Single section analysis | ~1 h | 8 cores | optional | 32 GB |
| cell2location deconvolution | ~2 h | 16 cores | A100 권장 | 64 GB |
| Multi-section integration | ~4 h | 16 cores | A100 | 128 GB |

## Limitations

- ❌ **Spot ≠ single cell** (regular Visium) — 1 spot = 1-10 cells. deconvolution 필수
- ❌ **Tissue 손상 sensitive** — FFPE는 RNA quality 변동 큼
- ⚠ **Reference scRNA 의존** — deconvolution 정확도는 reference 품질에 비례
- ⚠ **Detection sensitivity** — low-expression gene 검출 어려움
- ⚠ **Image registration 필수** — 조직 변형 보정. manual 검토 필요
- ⚠ **Visium HD는 데이터 큼** — 수백만 bin, RAM-intensive

## Quality Checks

- [x] Median UMI per spot > 1000
- [x] Median genes per spot > 500
- [x] Tissue coverage (% spots under tissue)
- [x] Cluster spatial coherence (not random)
- [x] Known anatomy (e.g., brain regions) recovered
- [x] Deconvolution sum-to-1 sanity

## References

- Ståhl PL, et al. Visualization and analysis of gene expression in tissue sections by spatial transcriptomics. *Science* 2016.
- Kleshchevnikov V, et al. Cell2location maps fine-grained cell types in spatial transcriptomics. *Nat Biotechnol* 2022.
- Palla G, et al. Squidpy: a scalable framework for spatial omics analysis. *Nat Methods* 2022.
- Zhao E, et al. Spatial transcriptomics at subspot resolution with BayesSpace. *Nat Biotechnol* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
