# Xenium (10x) / CosMx (NanoString) Imaging Spatial

**Category**: Spatial
**Tier**: 3
**Status**: 📝 1-pager

## Overview

상용화된 single-cell resolution imaging spatial — 10x **Xenium** (380 gene panel) + **CosMx** (1000+ gene). Visium의 spot ↔ single-cell 갭 메움. FFPE 가능.

**누가 의뢰**: 임상 archival FFPE spatial, drug efficacy spatial, tumor-immune spatial map.

## Input

- Xenium / CosMx slide + panel
- FFPE or fresh frozen tissue

## Pipeline / Tools

- **Xenium Explorer** (10x official)
- **CosMx AtoMx** (NanoString)
- **Squidpy / Seurat (spatial)** — downstream
- **CellPose / Baysor** — segmentation refinement

## Output

- Single-cell × gene counts + spatial coordinates, cell type maps, niche analysis

## Limitations

- ❌ Panel size limited (vs Visium untargeted)
- ❌ Instrument cost high
- ⚠ Segmentation artifact 가능

## References

- 10x Genomics Xenium platform documentation.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
