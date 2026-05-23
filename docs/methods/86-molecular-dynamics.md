# Molecular Dynamics (MD) Simulation

**Category**: Structural
**Tier**: 3
**Status**: 📝 1-pager

## Overview

단백질/막/리간드의 **time-resolved atomic motion** 을 simulate — flexibility, binding kinetics, conformational change, allostery. ns–µs timescale, GPU 가속으로 routine.

**누가 의뢰**: drug binding kinetics, allosteric mechanism, membrane protein dynamics, mutation effect on stability.

## Input

- Starting structure (PDB)
- Force field (AMBER, CHARMM, GROMOS)
- Solvent + ion setup

## Pipeline / Tools

- **GROMACS** — popular, GPU-accelerated
- **OpenMM** — Python-friendly
- **AMBER** — commercial
- **NAMD** — VMD ecosystem
- **MDAnalysis / MDTraj** — trajectory analysis

## Output

- Trajectory (XTC/DCD), RMSD/RMSF, hydrogen bond, free energy landscape (FEL)

## Limitations

- ❌ Timescale limited (typically ns–µs, vs biological ms–s)
- ❌ Force field accuracy for unusual residues/lipids
- ⚠ Sampling sufficiency for rare events (enhanced sampling — REMD, metadynamics)
- ⚠ Computational cost (GPU days for large systems)

## References

- Abraham MJ, et al. GROMACS: High performance molecular simulations through multi-level parallelism. *SoftwareX* 2015.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
