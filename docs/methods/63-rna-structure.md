# RNA Secondary Structure (SHAPE, icSHAPE, DMS-MaP)

**Category**: Transcriptomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Chemical probing 으로 in vivo / in vitro RNA secondary structure 측정. mRNA UTR / lncRNA / viral RNA functional element 발견.

**누가 의뢰**: viral RNA (SARS-CoV-2 frame-shift element), lncRNA mechanism, riboswitch 발견.

## Input

- SHAPE/DMS-modified RNA + control + MaP sequencing
- Read depth: 1000× per nt

## Pipeline / Tools

- **ShapeMapper2** — reactivity calculation
- **RNAfold (ViennaRNA)** — structure prediction with SHAPE constraint
- **DRACO** — alternative structure ensemble
- **eCLIP-SHAPE integration**

## Output

- Reactivity per nucleotide, predicted structure (dot-bracket), structural conservation

## Limitations

- ❌ Cell-permeable reagent 의존 (in vivo)
- ⚠ Alternative structures 모호
- ⚠ Read-depth requirement 높음

## References

- Smola MJ, et al. Detection of RNA-protein interactions with SHAPE-MaP. *Nat Protocols* 2015.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
