# ESMFold / OmegaFold (Single-sequence Structure)

**Category**: Structural
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Single-sequence based structure prediction — MSA 없이 protein language model (ESM-2) 으로 빠른 예측. AlphaFold 2/3 보다 약간 낮은 정확도이나 **수십배 빠름** — high-throughput, MSA-poor sequences.

**누가 의뢰**: 대규모 proteome structure prediction, AlphaFold MSA 부족 sequences (rare/divergent), 빠른 prototype.

## Input

- Single FASTA (single sequence)
- No MSA, no template required

## Pipeline / Tools

- **ESMFold (ESM-2)** — Meta AI
- **OmegaFold** — Helixon
- **HelixFold-Single**

## Output

- PDB structure + pLDDT confidence

## Limitations

- ❌ Accuracy slightly lower than AlphaFold (especially for very rare folds)
- ⚠ Quality drops for designed/de novo proteins
- ⚠ No multimer support (use AF-Multimer)

## References

- Lin Z, et al. Evolutionary-scale prediction of atomic-level protein structure with a language model (ESMFold). *Science* 2023.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
