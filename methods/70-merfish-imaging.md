# MERFISH / seqFISH (Imaging-based Spatial)

**Category**: Spatial
**Tier**: 3
**Status**: 📝 1-pager

## Overview

수백~수천 유전자의 single-molecule FISH 를 sequential imaging 으로 측정 — single-cell, single-molecule resolution. NGS-based spatial (Visium 등) 와 본질적으로 다른 paradigm.

**누가 의뢰**: 뇌 brain region (Allen, MERFISH brain atlas), 종양 sub-clonal spatial structure.

## Input

- MERSCOPE (Vizgen) 또는 custom MERFISH setup
- Tissue section + probe panel (140-500+ genes)

## Pipeline / Tools

- **starfish** — image processing
- **MERlin** — Vizgen pipeline
- **VPT (Vizgen Post-processing Tool)**
- **Squidpy** — downstream spatial analysis
- **CellPose / Baysor** — segmentation

## Output

- Per-cell gene counts + 2D/3D location, cell type maps, spatial niche, sub-cellular localization

## Limitations

- ❌ Panel-limited (untargeted Visium과 trade-off)
- ❌ Imaging time hours per slide
- ⚠ Segmentation 정확도 결정적

## References

- Chen KH, et al. RNA imaging. Spatially resolved, highly multiplexed RNA profiling in single cells (MERFISH). *Science* 2015.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
