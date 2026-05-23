# RNA Stability / Decay (SLAM-seq, BRIC-seq, 4sU-seq)

**Category**: Transcriptomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

mRNA half-life 측정 — synthesis (production rate) vs decay (degradation rate) 를 metabolic labeling 으로 분리. Steady-state 변화의 원인 (전사? 안정성?) 해명.

**누가 의뢰**: m6A/codon optimality 효과, miRNA target half-life 감소, stress granule effect.

## Input

- 4-thiouridine (4sU) labeled RNA + alkylation chemistry (SLAM-seq)
- Time-course sampling

## Pipeline / Tools

- **slamdunk** — SLAM-seq pipeline
- **GRAND-SLAM** — Bayesian half-life
- **pulseR** — kinetic modeling
- **BakR** — Bayesian rate inference

## Output

- Per-gene synthesis rate, decay rate, half-life

## Limitations

- ❌ 4sU toxicity at long incubation
- ⚠ Conversion efficiency variable
- ⚠ Computational kinetic model assumptions

## References

- Herzog VA, et al. Thiol-linked alkylation of RNA to assess expression dynamics (SLAM-seq). *Nat Methods* 2017.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
