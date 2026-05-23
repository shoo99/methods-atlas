# Similarity Network Fusion & Bayesian Multi-omics

**Category**: Multi-omics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Multi-omics 데이터를 직접 통합하지 않고 각 modality 의 **similarity network** 를 만든 뒤 fuse — sample clustering 후속 (cancer subtypes, patient stratification). Bayesian (iCluster+, BayesPrism) — uncertainty 포함.

**누가 의뢰**: cancer subtype discovery (TCGA scale), 환자 stratification, deep multi-omics 통합.

## Input

- Paired multi-omics matrices (expression + methylation + miRNA + protein)
- Sample size 200+ 권장

## Pipeline / Tools

- **SNFtool** (R) — kernel-based similarity fusion
- **iCluster+** — Bayesian integrative clustering
- **MOFAcell / SAFE-clustering / NEMO**
- **BayesPrism** — deconvolution + Bayesian

## Output

- Patient stratification (subtypes), subtype-marker features per omics, survival association

## Limitations

- ❌ Computational cost (Bayesian)
- ⚠ # subtypes 선택 heuristic
- ⚠ Modality 가중치 결정 difficult

## References

- Wang B, et al. Similarity Network Fusion for aggregating data types on a genomic scale. *Nat Methods* 2014.
- Shen R, et al. Integrative clustering of multiple genomic data types using a joint latent variable model (iCluster). *Bioinformatics* 2009.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
