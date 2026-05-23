# Cell–Cell Interaction (CCI) Inference

**Category**: Single-cell
**Tier**: 2
**Status**: 📝 1-pager

## Overview

scRNA-seq 에서 세포 type 간 ligand-receptor co-expression 을 바탕으로 추정되는 signaling interaction. **Bulk RNA**가 평균을 보는 반면 scRNA는 누가 누구에게 신호를 보내는가를 추론. 종양 microenvironment·발달·면역 시그널링 연구에 표준.

**누가 의뢰**: 종양 면역미세환경 (TME), 발달 organogenesis signaling, 약물의 cell-cell communication 변화 평가.

## Input

- **scRNA-seq** AnnData/Seurat (cell type annotation 필요)
- **(Optional) spatial transcriptomics** — co-localization 정보 활용
- **(Optional) condition labels** — differential CCI

## Pipeline

```
Annotated scRNA → ligand-receptor DB matching
  ├─ CellChat (gold standard, R, 100+ pathways)
  ├─ CellPhoneDB (Python, complex multimerized receptors)
  ├─ NicheNet (downstream target inference)
  ├─ LIANA (consensus framework, benchmarks all)
  ↓
Pathway-level aggregation → directed networks
Differential CCI (condition vs control)
Spatial overlay (if Visium/MERFISH)
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **CellChat 2** | R 2.1 | 1,939 ligand-receptor pairs + signaling pathways |
| **CellPhoneDB v5** | 5.0 | multimer complexes (e.g., TGFB-TGFBR1/2) |
| **NicheNet** | R 2.1 | ligand→target gene prediction |
| **LIANA / LIANA+** | R/Python 1.0 | consensus of 7+ methods |
| **MultiNicheNet** | R 1.0 | multi-sample, condition-aware |
| **stLearn / Squidpy CCI** | – | spatial context |

## Output

- `results/`:
  - `lr_pairs_significant.tsv` — ligand, receptor, sender, receiver, p
  - `pathway_communication_scores.tsv`
  - `differential_cci.tsv` (condition contrast)
- `figures/`:
  - `chord_plot.png` — sender↔receiver
  - `circle_plot.png` — pathway-level
  - `bubble_plot.png` — top LR pairs per pair-of-cell-types
  - `pathway_heatmap.png`
  - `spatial_overlay.png` (if spatial)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/cci:v1.0.0
docker run --rm -v $PWD:/work replisci/cci:v1.0.0 \
  Rscript run_cellchat.R adata.h5ad
```

## Demo Dataset

- 10x PBMC 10k (TNF/IL signaling between myeloid and T cells)
- 또는 **Tabula Sapiens** subset (organ-specific signaling)
- Spatial: 10x Visium FFPE breast cancer

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| CellChat (10k cells) | ~30 min | 8 cores | 32 GB |
| LIANA consensus (10k cells) | ~1 h | 8 cores | 32 GB |
| Multi-sample comparison | ~2 h | 16 cores | 64 GB |

## Limitations

- ❌ **Co-expression ≠ interaction** — 결과는 **추론**일 뿐, 실제 physical interaction 아님
- ❌ **Spatial 정보 없음** — non-spatial scRNA는 인접성 미반영. spatial 데이터 결합 권장
- ⚠ **Tool 간 일치율 ~50%** — LIANA consensus 사용 권장
- ⚠ **Cell type annotation 의존** — 부정확한 annotation은 잘못된 CCI 양산
- ⚠ **Soluble vs membrane ligand 구별 X** — 모든 LR을 동등 취급
- ⚠ **Downstream signaling 검증 필요** — pathway target gene 발현 / phosphoproteomics 권장

## Quality Checks

- [x] Cell type annotation 신뢰도 검증 (known markers)
- [x] Permutation test p-value
- [x] LIANA consensus (≥3 methods 일치)
- [x] Pathway top hits biologically plausible
- [x] Known LR (예: TNF-TNFR, IL6-IL6R) 검출

## References

- Jin S, et al. Inference and analysis of cell-cell communication using CellChat. *Nat Commun* 2021.
- Efremova M, et al. CellPhoneDB: inferring cell-cell communication from combined expression of multi-subunit ligand-receptor complexes. *Nat Protocols* 2020.
- Browaeys R, Saelens W, Saeys Y. NicheNet: modeling intercellular communication by linking ligands to target genes. *Nat Methods* 2020.
- Dimitrov D, et al. Comparison of methods and resources for cell-cell communication inference from single-cell RNA-Seq data (LIANA). *Nat Commun* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
