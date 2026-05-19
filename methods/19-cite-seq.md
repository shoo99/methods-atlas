# CITE-seq (Single-cell RNA + Surface Protein)

**Category**: Single-cell
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Antibody-Derived Tag (ADT) — DNA barcode 부착 항체 — 를 single-cell RNA-seq 와 동시 측정. 한 세포에서 **transcriptome + 표면 단백질 100+**을 함께 본다. Flow cytometry의 정확한 phenotyping + scRNA의 unbiased clustering을 결합.

**누가 의뢰**: 면역세포 phenotyping (T/B/NK subset), CAR-T 세포 monitoring, COVID/암 면역 profile, vaccine response, drug 면역 effect.

## Input

- **Platform**: 10x Genomics 3' v3.1 또는 5' Immune Profiling + ADT
- **Antibody panel**: 30-300 ADT (TotalSeq-A/B/C — BioLegend; 10x BD-curated panels)
- **Format**: FASTQ → CellRanger (RNA + Antibody Capture libraries)
- **Cell count**: 1,000-10,000 / sample

## Pipeline

```
CellRanger multi (RNA + ADT) → filtered_feature_bc_matrix
  → DSB or CLR normalization (ADT)
  → totalVI (joint embedding RNA+ADT) 또는 Seurat WNN
  → UMAP + clustering on joint
  → ADT-guided cell type annotation
  → RNA + protein co-expression discovery
  → DE per cluster (both modalities)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Count | CellRanger multi | 8.0 | RNA + ADT combined |
| ADT normalize | **DSB** | R 1.0 | denoised + scaled |
| (alt) CLR | – | – | centered log ratio |
| Integration | **totalVI** (scvi-tools) | 1.2 | deep gen model |
| (alt) | **Seurat WNN** | 5.1 | weighted nearest neighbor |
| Annotation | manual + Azimuth | – | reference-based |

## Output

- `results/`:
  - `adata_wnn.h5ad` — RNA + ADT joint
  - `protein_markers.tsv` — top ADT per cluster
  - `co_expression.tsv` — RNA vs surface protein
- `figures/`:
  - `umap_clusters.png`
  - `adt_violin_per_cluster.png`
  - `rna_vs_adt_scatter.png` (top markers)
  - `dotplot_protein_panel.png`
  - `flow-like_scatter.png` — CD4 vs CD8 biaxial
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/cite-seq:v1.0.0
docker run --gpus all --rm -v $PWD:/work replisci/cite-seq:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- 10x **PBMC 10k 5' v2 + ADT panel** (50+ markers)
- Or Hao et al. PBMC v3 dataset (Seurat WNN paper)

## Time & Resources

| Stage | Wall-clock | CPU | GPU | RAM |
|-------|-----------|-----|-----|-----|
| Demo (10k cells, 50 ADTs) | ~30 min | 8 cores | optional | 32 GB |
| Full (multi-sample integration) | ~3 h | 16 cores | A100 권장 | 64 GB |

## Limitations

- ❌ **Isotype control 필수** — non-specific binding 측정 (DSB 정규화에 사용)
- ❌ **ADT background 큼** — denoising (DSB, totalVI) 없이는 정확도 낮음
- ⚠ **Panel 비용** — 200+ ADT panel은 sample당 수십만원 추가
- ⚠ **Cross-reactivity** — 항체 specificity 검증 필요. lot 차이 큼
- ⚠ **Cell type bias** — antibody panel choice가 결과 좌우. 가설 기반 선정
- ⚠ **RNA vs protein 불일치 가능** — translation/turnover 차이. 둘 다 보고 권장

## Quality Checks

- [x] ADT background (empty droplets) distribution
- [x] Isotype control < specific marker
- [x] RNA-ADT cell barcode match > 90%
- [x] Known marker (CD3/CD19/CD56) → expected cluster
- [x] DSB normalized values reasonable distribution

## References

- Stoeckius M, et al. Simultaneous epitope and transcriptome measurement in single cells (CITE-seq). *Nat Methods* 2017.
- Mulè MP, et al. Normalizing and denoising protein expression data from droplet-based single cell profiling (DSB). *Nat Commun* 2022.
- Gayoso A, et al. Joint probabilistic modeling of single-cell multi-omic data with totalVI. *Nat Methods* 2021.
- Hao Y, et al. Integrated analysis of multimodal single-cell data (Seurat WNN). *Cell* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
