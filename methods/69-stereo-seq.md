# Stereo-seq (BGI High-resolution Spatial)

**Category**: Spatial
**Tier**: 3
**Status**: 📝 1-pager

## Overview

BGI Stereo-seq — DNA nanoball patterned chip 으로 **subcellular resolution (~500 nm)** + cm² 단위 large tissue 동시 capture. Visium보다 훨씬 높은 해상도, 식물·전체 organ 적용.

**누가 의뢰**: 전체 organ atlas, 식물 vasculature, embryo total organ map.

## Input

- BGI MGISeq + Stereo-seq chip (1cm × 1cm or larger)
- FFPE 또는 fresh frozen tissue

## Pipeline / Tools

- **SAW (Stereomics Analysis Workflow)** — BGI 공식
- **StereoPy** — Python downstream
- **Squidpy** — cross-platform
- **Spatial integration** (cell2location, RCTD)

## Output

- Cellbin matrix (single-cell-like), spatial UMAP, tissue domains, cell type maps

## Limitations

- ❌ BGI ecosystem dependency
- ⚠ Computational cost (수억 spot)
- ⚠ Smaller community than 10x

## References

- Chen A, et al. Spatiotemporal transcriptomic atlas of mouse organogenesis using DNA nanoball patterned arrays (Stereo-seq). *Cell* 2022.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
