# Protein–Protein Docking

**Category**: Structural
**Tier**: 3
**Status**: 📝 1-pager

## Overview

두 단백질 구조를 결합한 **complex** 의 docking pose 예측 — antibody-antigen, signaling complex, drug-protein. AlphaFold-Multimer가 새로운 기준이나 specialized docking이 여전히 유용.

**누가 의뢰**: 항체-항원 docking (CDR engineering), 단백질 design 검증, signaling complex hypothesis.

## Input

- Two protein structures (or sequences if AF-Multimer)
- Optional: binding interface hint (mutagenesis, HDX-MS)

## Pipeline / Tools

- **HADDOCK 3** — flexible docking + experimental data integration
- **ClusPro** — global rigid + clustering
- **AlphaFold-Multimer** — DL-based
- **ZDOCK / pyDOCK**
- **PIPER**

## Output

- Top complex poses (PDB), interface residues, ranking scores

## Limitations

- ❌ Flexibility limited (rigid/semi-flexible)
- ⚠ Scoring functions inaccurate for membrane proteins
- ⚠ Experimental restraints often needed for accurate

## References

- van Zundert GCP, et al. The HADDOCK2.4 Web Server: a Suite for Integrative Modeling. *J Mol Biol* 2024.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
