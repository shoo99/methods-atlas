# Affinity Pull-down MS (AP-MS / IP-MS / TurboID)

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

특정 단백질 (bait) 의 **상호작용자 (prey)** 를 immunoprecipitation 또는 proximity labeling (BioID/TurboID/APEX2) 으로 enrich → MS로 동정. PPI 발견의 gold standard.

**누가 의뢰**: drug target interactome, transcription complex 구성, signaling pathway 구성원, viral host factor 발굴.

## Input

- **Bait**: tagged protein (FLAG, HA, GFP) over-expressed + IgG/parental control
- **Or proximity labeling**: BioID/TurboID/APEX2 + biotin substrate
- **Replicate**: ≥3 bait + ≥3 control
- **MS**: standard DDA or DIA

## Pipeline

```
Lysate → IP (tag-specific antibody) or streptavidin (biotinylated)
  → on-bead digest → LC-MS/MS (DDA or DIA)
  → MaxQuant LFQ
  → SAINT / SAINTexpress / MiST — interaction probability scoring
  → CompPASS — control-aware filter
  → Cytoscape network visualization
  → ProHits-viz / dot plot → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **SAINTexpress** | 3.6 | Bayesian interaction probability |
| **MiST** | – | mass spec interaction statistics |
| **CompPASS** | – | comparative AP-MS scoring |
| **Cytoscape** | 3.10 | network viz |
| **ProHits-viz** | – | dot plot generation |
| **STRING / BioGRID** | – | reference PPI DB |

## Output

- Bait × prey interaction table + SAINT BFDR, network plot, dot plot, known vs novel interactor

## Demo / Time

- BioPlex 3.0 public AP-MS data
- ~4 h per bait (with multiple control)

## Limitations

- ❌ Indirect interactors capture — co-complex vs direct binding 불분명
- ❌ Tag overexpression artifact — endogenous tagging (CRISPR knock-in) 권장
- ⚠ Lysis condition (gentle vs strong) — interactome 크게 변함
- ⚠ Control 의 중요성 — proper isotype/parental/untagged 필수
- ⚠ Transient interaction missed (crosslinking 보완)

## References

- Teo G, et al. SAINTexpress: improvements and additional features in Significance Analysis of INTeractome software. *J Proteomics* 2014.
- Roux KJ, et al. A promiscuous biotin ligase fusion protein identifies proximal and interacting proteins in mammalian cells (BioID). *J Cell Biol* 2012.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
