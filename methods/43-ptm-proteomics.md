# PTM Proteomics (Phospho / Ubiq / Acetyl)

**Category**: Proteomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

후번역수정 (Post-Translational Modification) 의 site-specific identification + quantification. **Phosphoproteome** (kinase signaling), **ubiquitinome** (protein degradation), **acetylome** (epigenetic, metabolic regulation).

**누가 의뢰**: kinase drug 효과 평가 (pY/pS/pT), targeted protein degradation (PROTACs), 신호전달 pathway, metabolic flux.

## Input

- **Enrichment 필수**:
  - Phospho: TiO₂, IMAC (Fe³⁺), HILIC
  - Ubiq: K-ε-GG antibody (PTMScan)
  - Acetyl: K-Ac antibody
- **MS**: high-resolution Orbitrap with HCD/ETD
- **Sample**: 1-5 mg protein input (low enrichment ratio)

## Pipeline

```
Enriched peptide → LC-MS/MS
  → MaxQuant / FragPipe (with variable modification site search)
  → site localization (PTMscore, PhosphoRS, ptmRS, MSFragger PTM-LFP)
  → site-level quantification (LFQ / TMT)
  → KSEA (kinase-substrate enrichment) — kinase activity
  → ssGSEA / GSEA on PTM signatures
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **MaxQuant** | 2.6 | comprehensive PTM search |
| **FragPipe + MSFragger** | 21.1 | open-search, site localization |
| **Skyline** | 23 | targeted PTM |
| **KSEA** | R 1.2 | kinase-substrate enrichment |
| **PhosphoSitePlus / iPTMnet** | – | site annotation DB |
| **PTMcentral** | – | PTM proteomics standards |

## Output

- Site × sample modification level, KSEA scores (kinase activity), enriched pathways, site occupancy (with paired total proteome)

## Demo / Time

- PRIDE phospho HEK293 EGF stimulation
- ~6 h per sample

## Limitations

- ❌ Site localization ambiguity — adjacent S/T/Y, ScorePT, PEAKS PTM 권장
- ❌ Enrichment efficiency 변동 — biological replicate 차이 ↑
- ⚠ Stoichiometry 측정 어려움 — total proteome paired 필수
- ⚠ Crosstalk between modifications (e.g., O-GlcNAc/phospho on same Ser)
- ⚠ Low-abundance modifications missed

## References

- Ochoa D, et al. The functional landscape of the human phosphoproteome. *Nat Biotechnol* 2020.
- Mertins P, et al. Reproducible workflow for multiplexed deep-scale proteome and phosphoproteome analysis of tumor tissues. *Nat Protocols* 2018.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
