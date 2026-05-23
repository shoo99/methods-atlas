# Protein-Protein Interaction (PPI) Network Analysis

**Category**: Functional
**Tier**: 2
**Status**: 📝 1-pager

## Overview

DEGs / hit genes 를 **PPI network** (STRING, BioGRID, HuRI, BioPlex) 에 임베드 → topology 분석 (hub, betweenness), 모듈 발견, 경로 enrichment. PPI 컨텍스트로 결과 해석에 유용.

**누가 의뢰**: 모든 omics 보고서의 mechanistic 해석 단계, drug target 우선순위, complex assembly.

## Input

- **Gene list** (DEGs, somatic mutated, etc.)
- 또는 **expression matrix** for co-expression overlay

## Pipeline

```
Gene list → STRING API (combined score) / BioGRID / IntAct
  → network construction → topology metrics (degree, betweenness)
  → modules (MCODE, ClusterONE, Leiden)
  → hub gene identification
  → pathway enrichment per module
  → Cytoscape visualization
  → Report
```

| Tool / Resource | Version | Purpose |
|---|---|---|
| **STRING DB** | 12.0 | combined PPI evidence |
| **BioGRID** | 4.4 | curated interactions |
| **IntAct** | 2024 | molecular interactions |
| **HuRI** | – | human reference interactome |
| **BioPlex 3.0** | – | AP-MS interactome |
| **Cytoscape** | 3.10 | viz |
| **igraph** / **NetworkX** | R/Python | network metrics |
| **MCODE / ClusterONE** | – | module detection |

## Output

- Network graph, topology metrics, modules + enrichments, hub gene table

## Demo / Time

- DEGs from any project → STRING enrichment
- ~5 min – 1 h

## Limitations

- ❌ Database bias — well-studied gene 가 highly connected → bias 가능
- ⚠ PPI evidence 종류 (binary vs co-complex vs predicted) 명시
- ⚠ Tissue specificity 미반영 — generic interactome
- ⚠ Dynamic vs static — context-dependent interaction 누락

## References

- Szklarczyk D, et al. The STRING database in 2023: protein-protein association networks. *NAR* 2023.
- Oughtred R, et al. The BioGRID interaction database: 2021 update. *NAR* 2021.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
