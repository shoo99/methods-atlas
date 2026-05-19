# Protein–Ligand Docking (Small Molecule)

**Category**: Structural
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Small molecule (drug, metabolite, inhibitor) 의 단백질 binding pose 와 affinity 예측 — drug discovery, target-fragment optimization, repurposing.

**누가 의뢰**: drug screening / virtual screening, lead optimization, metabolite-enzyme interaction.

## Input

- Receptor structure (X-ray, cryo-EM, AlphaFold)
- Ligand library (SMILES, SDF)
- Binding site definition (or blind docking)

## Pipeline / Tools

- **AutoDock Vina / Vina-GPU** — classical
- **DiffDock** — DL-based
- **Glide** (Schrödinger commercial)
- **GNINA** — DL scoring
- **rDock / Smina**
- **MOE / OpenEye**

## Output

- Top binding poses (PDB), predicted affinity (kcal/mol), interaction map

## Limitations

- ❌ Scoring vs binding affinity correlation moderate
- ❌ Protein flexibility limited
- ⚠ Solvent/entropy effects underestimated
- ⚠ Experimental validation (IC50, ITC) needed

## References

- Eberhardt J, et al. AutoDock Vina 1.2.0. *J Chem Inf Model* 2021.
- Corso G, et al. DiffDock: Diffusion Steps, Twists, and Turns for Molecular Docking. *ICLR* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
