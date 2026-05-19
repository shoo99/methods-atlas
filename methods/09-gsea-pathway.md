# GSEA / Pathway Enrichment Analysis

**Category**: Functional
**Tier**: 1
**Status**: 📝 1-pager

## Overview

DEG / DA peak / variant 등 hit gene set을 **biological pathway** (GO, KEGG, Reactome, MSigDB Hallmark, WikiPathways) 와 매핑해 어떤 생물학적 프로세스가 활성/억제되었는지 해석. ORA (Over-Representation Analysis) 와 **GSEA** (Gene Set Enrichment Analysis) 두 paradigm.

**누가 의뢰**: 거의 모든 omics 분석의 **마지막 단계**. PI는 결과를 pathway로 정리해야 논문/보고서 작성 가능.

## Input

- **Gene list (ORA용)**:
  - Hit set: DEGs (e.g., padj<0.05, |log2FC|>1)
  - Background: 측정된 모든 유전자 (universe)
  - ID: Ensembl, Entrez, gene symbol, UniProt
- **Ranked gene list (GSEA용)**:
  - 전체 측정 유전자의 ranked metric: `-log10(padj) × sign(log2FC)`, log2FC, t-statistic 등
  - 형식: 2-column TSV (`gene_id`, `score`)
- **Optional**: species, gene length (GO bias correction)

## Pipeline

```
DEGs / ranked list
  ↓
ID conversion (Ensembl ↔ Entrez ↔ symbol) — biomaRt / pyensembl / mygene
  ↓
ORA → clusterProfiler / gseapy / enrichR / g:Profiler
GSEA → fgsea / GSEApy
  ↓
Multi-database aggregation (GO BP/MF/CC, KEGG, Reactome, Hallmark, WikiPathways)
  ↓
Visualization: dotplot, enrichment map, network, ridgeplot
  ↓
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **clusterProfiler** | R/Bioc 4.14 | R-based, dotplot/cnetplot/emapplot |
| **fgsea** | R/Bioc 1.32 | fast GSEA, multilevel preranked |
| **GSEApy** | 1.1.3 | Python-based, Enrichr API |
| g:Profiler (gprofiler2) | R 0.2 / API | webserver + R/Python |
| ReactomePA | R/Bioc 1.50 | Reactome 특화 |
| EnrichmentMap (Cytoscape) | 3.4 | network visualization |
| WebGestalt | API | ORA + GSEA + NTA |
| MSigDB | 2025.1 | curated gene sets (Hallmark 핵심) |
| DOSE | R/Bioc 4.0 | disease ontology |
| pathfindR | R 2.4 | active subnetwork enrichment |

## Output

- `results/ora_go_bp.tsv` — GO Biological Process ORA
- `results/ora_kegg.tsv`, `ora_reactome.tsv`, `ora_hallmark.tsv`
- `results/gsea_hallmark.tsv` — Hallmark GSEA (NES, pvalue, FDR, leading edge genes)
- `results/gsea_kegg.tsv`, `gsea_reactome.tsv`
- `figures/`:
  - `dotplot_top20.png` — gene ratio × p-value
  - `enrichment_map.png` — cluster of related pathways
  - `ridgeplot.png` — expression distribution per pathway
  - `cnetplot.png` — gene-pathway network
  - `barplot_nes.png` — GSEA Normalized Enrichment Score
- `report/report.pdf` — pathway interpretation chapter

## Reproducible Environment

```bash
docker pull replisci/pathway-enrichment:v1.0.0
docker run --rm -v $PWD:/work replisci/pathway-enrichment:v1.0.0 \
  Rscript run_enrichment.R input.tsv
```

또는 `gseapy.prerank()` Python one-liner — Enrichr API call.

## Demo Dataset

- **Input**: GSE52778 (airway DEX) DE 결과 (이미 Replisci 데모로 확보)
- **Expected**: TLR signaling, IL-10 anti-inflammatory, IL-17 signaling, glucocorticoid response 검출
- **Validation**: 결과 pathway 가 Himes et al. 2014 논문의 결론과 일치

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| ORA (5-10k DEGs vs 10 DBs) | ~5 min | 1 core | 4 GB | – |
| GSEA preranked (전체 유전자 × 10 DBs) | ~15 min | 4 cores | 8 GB | – |
| 통합 보고서 생성 | ~20 min | 4 cores | 8 GB | – |

가장 가벼운 메소드 — laptop에서도 실행 가능.

## Limitations

- ❌ **ORA의 background 의존성** — universe 선택이 결과 좌우. 모든 측정 유전자 vs 전체 게놈 차이 명확히 명시 필요
- ❌ **다중검정 보정** — DB마다 N pathway 다름. global FDR vs per-DB FDR 명시
- ⚠ **Pathway annotation 노후** — KEGG/Reactome 업데이트 주기 있음. 최신 release 명시
- ⚠ **Gene set redundancy** — GO terms는 hierarchical, 중복 다수. enrichment map / REVIGO 로 정리
- ⚠ **Species 한정** — 인간/생쥐/래트는 풍부, 비-모델 종은 ortholog mapping 필요
- ⚠ **GSEA의 ranked metric 선택** — log2FC vs t-statistic vs -log10p × sign 결과 다름. 사전 정의 후 일관 사용
- ⚠ **Interpretation 위험** — pathway hit은 가설일 뿐, downstream 검증 없이는 결론 X

## Quality Checks

- [x] ID conversion loss rate < 10%
- [x] Top hits biologically plausible (sanity check vs 실험 가설)
- [x] FDR 보정 명시 (BH q < 0.05 권장)
- [x] Leading edge gene 개수 reasonable (5-30)
- [x] 다중 DB cross-validation (GO BP + KEGG + Reactome 일관성)

## References

**Tools**
- Subramanian A, et al. Gene set enrichment analysis: A knowledge-based approach for interpreting genome-wide expression profiles. *PNAS* 2005.
- Korotkevich G, et al. Fast gene set enrichment analysis (fgsea). *bioRxiv* 2021.
- Wu T, et al. clusterProfiler 4.0: A universal enrichment tool for interpreting omics data. *Innovation* 2021.
- Fang Z, et al. GSEApy: a comprehensive package for performing gene set enrichment analysis in Python. *Bioinformatics* 2023.

**Databases**
- MSigDB (Liberzon A, et al. *Cell Systems* 2015), KEGG (Kanehisa M), Reactome (Jassal B, et al. *NAR* 2020), GO (Ashburner M, et al. *Nat Genet* 2000).

**Best practice**
- Reimand J, et al. Pathway enrichment analysis and visualization of omics data using g:Profiler, GSEA, Cytoscape and EnrichmentMap. *Nat Protocols* 2019.

---

**Lead**: Replisci
**Note**: 거의 모든 omics 의뢰에 종속 메소드로 포함됨
**Last updated**: 2026-05-19
