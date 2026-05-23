# Spatial Niche / Domain Detection

**Category**: Spatial
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Spatial transcriptomics에서 **공간적 cluster (niche, tissue domain)** 검출 — BayesSpace, GraphST, STAGATE 등. Sample 의 anatomical region (cortex layer, tumor margin) 자동 식별.

**누가 의뢰**: brain region atlas, tumor heterogeneity spatial, organoid maturation zones.

## Input

- Visium / Stereo-seq / MERFISH AnnData
- Cell type annotations (선택)

## Pipeline / Tools

- **BayesSpace** — spatial Bayesian clustering
- **GraphST** — graph deep learning
- **STAGATE** — graph attention
- **SpaGCN** — graph convolution + histology

## Output

- Domain/niche labels per spot, refined high-resolution boundaries, marker genes per niche

## Limitations

- ❌ # niches hyperparameter
- ⚠ Computational cost on large slides
- ⚠ Histology integration variable

## References

- Zhao E, et al. BayesSpace. *Nat Biotechnol* 2021.
- Long Y, et al. GraphST. *Nat Commun* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
