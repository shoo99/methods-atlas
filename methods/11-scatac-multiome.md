# Single-cell ATAC-seq & Multiome (snRNA + snATAC)

**Category**: Single-cell / Epigenomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

단일 세포 수준의 **chromatin accessibility** (scATAC-seq) 및 RNA+ATAC 동시 측정 (10x Multiome). 세포 type별 enhancer/promoter 활성, TF binding 활동, *cis*-regulatory program 해부.

**누가 의뢰**: 세포 분화/리프로그래밍 메커니즘, 종양 epigenetic heterogeneity, 면역세포 state 분석, 약물 enhancer 효과 평가.

## Input

- **Platform**:
  - 10x Genomics scATAC-seq v2
  - **10x Multiome (RNA + ATAC)** — single nucleus, 같은 cell barcode
  - BD Rhapsody ATAC
  - sciATAC-seq (combinatorial indexing)
- **Format**: FASTQ + barcode → CellRanger ATAC / CellRanger ARC (multiome)
- **Minimum nuclei**: 1,000-10,000 per sample
- **Read depth**: 25k-50k unique fragments / cell

## Pipeline

```
FASTQ → CellRanger ATAC/ARC (count) → fragments.tsv.gz
  → ArchR / Signac (QC, peak call per cluster)
  → LSI/TF-IDF → UMAP → Leiden clusters
  → Marker peaks per cluster → Motif enrichment (chromVAR)
  → (Multiome) RNA-ATAC integration (WNN, MultiVI)
  → Gene activity scores → peak-gene linkage
  → Trajectory / differential per cluster
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Count | CellRanger ATAC | 2.1.0 | 10x official |
| Count multiome | CellRanger ARC | 2.0.2 | RNA+ATAC paired |
| Analysis (R) | **ArchR** | 1.0.2 | scalable, fragments-based |
| (alt R) | **Signac** | 1.14 | Seurat ecosystem |
| (alt Python) | episcanpy / SnapATAC2 | 0.4 / 2.7 | Python |
| Motif | chromVAR | 1.28 | TF activity per cell |
| Integration | WNN (Seurat) / MultiVI | 5.1 / scvi 1.2 | RNA+ATAC joint |
| Peak-gene link | ArchR addPeak2GeneLinks | – | regulatory potential |
| Footprint | TOBIAS | 0.16 | TF binding inference |
| Trajectory | ArchR Trajectory / scVelo | – | pseudotime |

## Output

- `qc/`: TSS enrichment per cell, nucleosome score, fragment dist
- `results/`:
  - `peaks_per_cluster.bed`
  - `cell_embeddings.h5ad/Arrow` — ATAC + multiome
  - `motif_activity.tsv` — chromVAR z-scores per cell
  - `peak_gene_links.tsv` (multiome)
  - `marker_peaks_per_cluster.tsv`
- `figures/`:
  - `umap_atac.png`, `umap_multiome.png`
  - `tss_enrichment.png`
  - `motif_heatmap.png` — TF activity per cluster
  - `peak_track_per_cluster.png` — IGV-style
  - `peak_gene_linkage.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/scatac-multiome:v1.0.0
docker run --gpus all --rm -v $PWD:/work replisci/scatac-multiome:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

## Demo Dataset

- **scATAC**: 10x Genomics **5k PBMC scATAC** v2
- **Multiome**: 10x **PBMC granulocyte-sorted 10k multiome**
- **Validation**: T/B/Mono cluster + lineage TF activity (CTCF, PU.1, EBF1)

## Time & Resources

| Stage | Wall-clock | CPU | GPU | RAM |
|-------|-----------|-----|-----|-----|
| Demo (5k cells, fragments input) | ~30 min | 8 cores | – | 32 GB |
| FASTQ → counts | ~6 h | 16 cores | – | 64 GB |
| Multiome 20k cells | ~3 h | 16 cores | optional | 64 GB |

## Limitations

- ❌ **Sparsity 극심** — scATAC는 cell당 ~30k fragments. 단일 peak 의 binary nature (0/1) 통계 어려움
- ❌ **Peak calling cluster-level** — single cell 단위 peak X. cluster pseudobulk → MACS2
- ⚠ **Doublet detection 어려움** — ATAC는 RNA만큼 명확한 doublet score X. AMULET 사용 권장
- ⚠ **Reference genome 의존**, motif DB 의존
- ⚠ **scATAC-only 한계** — RNA 없이는 cell type 정확 annotation 어려움. Multiome 또는 reference mapping 권장
- ⚠ **Computational cost** — ArchR Arrow file이 큰 데이터에서 RAM-heavy

## Quality Checks

- [x] TSS enrichment per cell > 4
- [x] Fragments per cell > 1,000 (filter)
- [x] Nucleosome periodicity 확인
- [x] Doublet rate < 10% post-AMULET
- [x] **Multiome**: same barcode RNA-ATAC pairing 확인
- [x] Known lineage motif enrichment in expected clusters

## References

- Granja JM, et al. ArchR is a scalable software package for integrative single-cell chromatin accessibility analysis. *Nat Genet* 2021.
- Stuart T, et al. Single-cell chromatin state analysis with Signac. *Nat Methods* 2021.
- Ashuach T, et al. MultiVI: deep generative model for the integration of multimodal data. *Nat Methods* 2023.
- Schep AN, et al. chromVAR: inferring transcription-factor-associated accessibility from single-cell epigenomic data. *Nat Methods* 2017.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
