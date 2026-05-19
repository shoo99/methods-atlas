# Single-cell Trajectory / Pseudotime Analysis

**Category**: Single-cell
**Tier**: 2
**Status**: 📝 1-pager

## Overview

세포 분화·발달·활성화의 연속적 변화를 single-cell 데이터에서 *in silico* 복원. 세포를 "pseudotime"으로 정렬해 lineage tree·branch·gene module dynamics를 추론. RNA velocity로 미래 state까지 예측.

**누가 의뢰**: 발달생물학·줄기세포·암 EMT·면역세포 활성화·CAR-T persistence·organoid maturation.

## Input

- **scRNA-seq** AnnData/Seurat object (이미 QC, clustered)
- **RNA velocity**: spliced + unspliced count matrices (`velocyto run`, kallisto|bustools)
- **Optional**: time-point sampling (developmental stages)
- **Optional**: lineage barcodes (CellTagging, MARC1)

## Pipeline

```
QC'd scRNA → branchpoint hypothesis
  ├─ DPT/Diffusion pseudotime (Scanpy/destiny)
  ├─ Monocle3 / Slingshot — graph-based pseudotime
  ├─ scVelo — RNA velocity → directed pseudotime
  ├─ Palantir — terminal state probability
  ├─ PAGA — coarse-grained lineage graph
  └─ CellRank — Markov chain on velocity
  ↓
Gene module dynamics → driver genes per branch
Cell-cell similarity (e.g., scFates) → TF dynamics
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **Monocle3** | R 1.3.7 | tree, branch, DE along pseudotime |
| **scVelo** | 0.3 | RNA velocity (dynamical model) |
| **CellRank 2** | 2.0 | Markov chain on velocity/kernels |
| **Palantir** | 1.3 | terminal state probability |
| **PAGA** (Scanpy) | 1.10 | coarse-grained graph |
| **Slingshot** | R 2.14 | curve fitting |
| **scFates** | 1.0 | TF dynamics, bifurcation |
| **dynverse** | – | benchmark wrapper |

## Output

- `results/`:
  - `pseudotime_per_cell.tsv`
  - `branch_assignment.tsv`
  - `driver_genes_per_branch.tsv`
  - `velocity_arrows.h5ad`
- `figures/`:
  - `umap_pseudotime.png` — colored by pseudotime
  - `velocity_stream.png` — velocity arrows on UMAP
  - `paga_graph.png`
  - `gene_dynamics_heatmap.png` — top genes along pseudotime
  - `branch_logfc_volcano.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/trajectory:v1.0.0
docker run --gpus all --rm -v $PWD:/work replisci/trajectory:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **Pancreas endocrinogenesis** (Bastidas-Ponce et al. 2019) — classic scVelo demo
- 또는 hematopoiesis dataset (Setty et al. 2019, Palantir paper)

## Time & Resources

| Stage | Wall-clock | CPU | GPU | RAM |
|-------|-----------|-----|-----|-----|
| Demo (~10k cells) | ~30 min | 8 cores | optional | 32 GB |
| Large dataset (100k+ cells) | ~2 h | 16 cores | A100 권장 | 128 GB |

## Limitations

- ❌ **Static snapshot** — single time-point으로 dynamics 추론. 실제 dynamic은 time-course 또는 lineage tracing 필요
- ❌ **Branch hypothesis 인간 입력 필요** — root cell 지정 필수
- ⚠ **Velocity 가정** — splicing dynamics 안정 가정. mature cell type에서 부정확
- ⚠ **Tool 간 결과 차이 큼** — benchmark (dynverse) 권장
- ⚠ **Trajectory ≠ lineage** — *in silico* trajectory는 가설. 실제 lineage는 barcoding 또는 in vivo tracing 필요
- ⚠ **Batch effect** — integration 후 trajectory 권장

## Quality Checks

- [x] Root cell selection biologically motivated
- [x] Known markers expressed in expected direction
- [x] Velocity consistency score (scVelo) > 0
- [x] Cross-method agreement (Monocle vs scVelo) on major branches
- [x] In vivo / orthogonal validation suggested (FACS-sorted stages)

## References

- Trapnell C, et al. The dynamics and regulators of cell fate decisions are revealed by pseudotemporal ordering of single cells (Monocle). *Nat Biotechnol* 2014.
- Bergen V, et al. Generalizing RNA velocity to transient cell states through dynamical modeling (scVelo). *Nat Biotechnol* 2020.
- Setty M, et al. Characterization of cell fate probabilities in single-cell data with Palantir. *Nat Biotechnol* 2019.
- Lange M, et al. CellRank for directed single-cell fate mapping. *Nat Methods* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
