# Single-cell RNA-seq

**Category**: Single-cell
**Tier**: 1
**Status**: 📝 1-pager

## Overview

수천~수십만 개 단일 세포의 transcriptome을 동시에 측정 → 세포 type/state 발견, 발달 경로 추적, rare population 검출. Bulk RNA-seq가 평균을 보는 데 비해 scRNA-seq는 세포 이질성을 해부한다.

**누가 의뢰**: 면역학·종양학·발달생물학·신경과학 PI, CAR-T·single-cell 약효 평가 R&D.

## Input

- **Data type**: 10x Genomics Chromium (가장 흔함), Smart-seq2/3, BD Rhapsody, Drop-seq, Parse Biosciences
- **Format**:
  - 10x: FASTQ → `cellranger count` → filtered_feature_bc_matrix.h5
  - 또는 raw FASTQ → STARsolo / kallisto|bustools
- **Minimum cells**: 500-5,000 per sample (rare population은 더 많이)
- **Read depth**: 20-50k reads / cell (10x 3' standard)
- **Sample metadata**: condition, donor, tissue, capture batch

## Pipeline

```
FASTQ → CellRanger/STARsolo (mapping + counting)
  → Scanpy/Seurat (QC, filter, normalize, scale)
  → PCA → neighbors → UMAP → Leiden clustering
  → Marker genes → cell-type annotation (manual + automated)
  → DE between conditions per cluster → composition tests
  → Trajectory (optional) → CCI (optional) → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Demux + count | CellRanger | 8.0.1 | 10x official |
| Alt counter | STARsolo / kb-python | 2.7.11 / 0.28 | open-source, faster |
| Doublet removal | Scrublet / scDblFinder | 0.2.3 / 1.20 | technical artifacts |
| QC + cluster | Scanpy | 1.10.3 | python-based |
| (alt) | Seurat | 5.1 | R-based |
| Integration | Harmony / scVI | 0.0.10 / 1.2 | batch correction |
| Annotation | CellTypist / SingleR | 1.6 / 2.8 | reference-based |
| DE | MAST / pseudobulk + DESeq2 | 1.32 / 1.46 | per-cluster DE |
| Trajectory | scVelo / Monocle3 | 0.3 / 1.3 | dynamics |
| CCI | CellChat / CellPhoneDB | 2.1 / 5.0 | cell-cell signaling |

## Output

- `qc/qc_metrics.html` — % MT, n_genes, n_counts, doublet score
- `results/adata_filtered.h5ad` — AnnData with embeddings, clusters
- `figures/`:
  - `qc_violin.png` — quality distributions
  - `umap_clusters.png` — Leiden clusters on UMAP
  - `umap_celltype.png` — annotated cell types
  - `markers_dotplot.png` — top markers per cluster
  - `composition_stacked.png` — cell-type % by condition
  - `de_volcano_per_cluster.png` — per-cluster DE
- `results/markers.tsv`, `results/de_per_cluster.tsv`
- `report/report.pdf` — 통합 보고서

## Reproducible Environment

```bash
docker pull replisci/scrnaseq:v1.0.0
docker run --gpus all --rm -v $PWD:/work replisci/scrnaseq:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

GPU 권장 (scVI integration, UMAP 가속).

## Demo Dataset

- **Accession**: 10x Genomics public **PBMC 10k** (v3 chemistry)
- **Cells**: ~10,000 human PBMC
- **Expected output**: T/B/NK/Mono/DC clusters
- **Why this dataset**: 가장 표준적인 reference, 모든 tutorial이 사용
- **Validation**: cell-type proportions match published distribution

대안 데모: Tabula Sapiens (multi-organ atlas), HCA datasets.

## Time & Resources

| Stage | Wall-clock | CPU | GPU | RAM | Disk |
|-------|-----------|-----|-----|-----|------|
| Demo (10k cells, h5 input) | ~15 min | 8 cores | optional | 16 GB | 5 GB |
| FASTQ → counts (10k cells) | ~3 h | 16 cores | – | 64 GB | 200 GB |
| Full project (50k+ cells, multi-sample integration) | ~6 h | 32 cores | A100 권장 | 128 GB | 500 GB |

## Limitations

- ❌ **Dropout** — single-cell 특성상 zero-inflation 큼. low expression 유전자는 검출력 낮음
- ⚠ **Batch effect 큼** — 다른 날 capture 시 integration 필수 (Harmony/scVI)
- ⚠ **Ambient RNA** — empty droplet의 background. SoupX/CellBender로 제거 권장
- ⚠ **Cell-type annotation 신뢰도** — automated tool은 reference 의존. manual marker 검증 필수
- ⚠ **DE between cluster ≠ between condition** — 같은 cluster 내 condition 비교는 pseudobulk 권장 (Squair et al. 2021)
- ❌ **Pseudo-bulk N** — biological replicate 부족하면 condition DE 통계 검정력 매우 낮음

## Quality Checks

- [x] % MT < 20% (보통 5-15%)
- [x] n_genes per cell: 200 < x < 7,000 (조직별 조정)
- [x] Doublet rate < 10% after removal
- [x] Replicate sample correlation by cluster proportions
- [x] Known markers validate cluster annotations
- [x] Integration: batch effect score (kBET, LISI) reduced

## References

**Tools**
- Wolf FA, Angerer P, Theis FJ. SCANPY: large-scale single-cell gene expression data analysis. *Genome Biology* 2018.
- Hao Y, et al. Dictionary learning for integrative, multimodal and scalable single-cell analysis. *Nat Biotechnol* 2024 (Seurat v5).
- Lopez R, et al. Deep generative modeling for single-cell transcriptomics. *Nat Methods* 2018 (scVI).
- Korsunsky I, et al. Fast, sensitive and accurate integration of single-cell data with Harmony. *Nat Methods* 2019.

**Best practice**
- Heumos L, et al. Best practices for single-cell analysis across modalities. *Nat Rev Genet* 2023.
- Sanbomics, Theis lab tutorials, Scanpy/Seurat vignettes.

**Demo dataset**
- 10x Genomics: https://www.10xgenomics.com/datasets

---

**Lead**: Replisci
**Last updated**: 2026-05-19
