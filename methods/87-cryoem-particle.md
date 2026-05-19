# Cryo-EM Single Particle Analysis

**Category**: Structural
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Cryo-EM 으로 macromolecular complex 의 high-resolution structure (3-4 Å typical, 1-2 Å with new detectors) 결정. X-ray crystallography 대안 — crystallization 불가능 sample 가능.

**누가 의뢰**: 거대 complex (ribosome, RNA polymerase II, viral capsid), membrane protein, dynamic complex.

## Input

- Cryo-EM micrographs (Krios / Glacios)
- 1000-10000 micrographs / dataset
- 10⁵-10⁶ particle picks

## Pipeline / Tools

- **RELION 5** — open-source gold standard
- **cryoSPARC** — commercial, GPU-fast
- **CTFFIND / Gctf** — CTF estimation
- **Topaz / crYOLO** — DL particle picking
- **Phenix** — refinement (atomic model)
- **ChimeraX** — visualization

## Output

- 3D density map (MRC), atomic model (PDB), FSC resolution curve

## Limitations

- ❌ Sample prep (vitrification, ice quality) decisive
- ❌ Computational cost (GPU days for large datasets)
- ⚠ Preferred orientation bias
- ⚠ Conformational heterogeneity → continuous variation (3DFlex)

## References

- Punjani A, et al. cryoSPARC: algorithms for rapid unsupervised cryo-EM structure determination. *Nat Methods* 2017.
- Scheres SHW. RELION: implementation of a Bayesian approach to cryo-EM structure determination. *J Struct Biol* 2012.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
