# Deep Learning Multi-omics Integration

**Category**: Multi-omics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Variational autoencoder (VAE) / contrastive learning / transformer 으로 multi-modal omics joint embedding — **non-linear** 통합 + perturbation prediction (CPA) + cross-modal imputation.

**누가 의뢰**: single-cell multi-omics, drug perturbation modeling, atlas-scale integration.

## Input

- Paired multi-omics (scRNA + scATAC, RNA + protein)
- Optional: condition/perturbation labels

## Pipeline / Tools

- **totalVI / MultiVI / scMODE** — scVI ecosystem
- **scGen / CPA** — perturbation prediction
- **GLUE** — graph-linked unified embedding
- **Multigrate**

## Output

- Joint latent embedding, cross-modal prediction, perturbation simulation, denoised expression

## Limitations

- ❌ GPU 필요, training cost
- ❌ Interpretability vs PCA/MOFA
- ⚠ Hyperparameter tuning sensitive

## References

- Ashuach T, et al. MultiVI. *Nat Methods* 2023.
- Lotfollahi M, et al. Predicting cellular responses to perturbation across diverse contexts with CPA. *Mol Syst Biol* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
