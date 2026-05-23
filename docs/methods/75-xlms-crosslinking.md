# Cross-linking Mass Spectrometry (XL-MS)

**Category**: Proteomics
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Chemical cross-linker (DSSO, BS3, PhoX 등) 로 protein interactome 의 **direct contact** 를 covalently fix 후 MS로 cross-linked peptide identify → 구조 + interaction 동시 정보.

**누가 의뢰**: 단백질 complex 구조 (AlphaFold 검증), drug-target binding interface, large complex assembly.

## Input

- XL-treated protein extract / complex
- Cleavable crosslinker (DSSO) 권장 — MS/MS friendly

## Pipeline / Tools

- **pLink 2 / 3** — XL-MS gold standard
- **MeroX**
- **xQuest / xProphet**
- **MS Annika** — Proteome Discoverer

## Output

- Cross-linked peptide pairs with residue-level info, distance constraints, network

## Limitations

- ❌ Search space combinatorial — false positive 통제 어려움
- ❌ Sub-stoichiometric crosslinks → low signal
- ⚠ Sample prep complex

## References

- Yu C, Huang L. Cross-Linking Mass Spectrometry: An Emerging Technology for Interactomics and Structural Biology. *Anal Chem* 2018.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
