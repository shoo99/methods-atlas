# Single-cell Lineage Tracing (CellTagging, MARC1, LARRY)

**Category**: Single-cell
**Tier**: 3
**Status**: 📝 1-pager

## Overview

Heritable DNA barcode 를 단일 세포에 도입 → 자손 세포 family tree 추적. Static scRNA의 trajectory 추론 한계를 극복하고 실제 lineage 확인.

**누가 의뢰**: 발달 lineage, stem cell hierarchy, 종양 clonal evolution, drug resistance clone tracking.

## Input

- Lentivirus-mediated barcode + scRNA-seq capture
- Multi-time-point sampling

## Pipeline / Tools

- **LARRY** — lentiviral barcode + state inference
- **CoSpar** — clonal + dynamic
- **CellTagging** — combinatorial barcode
- **GASTRULA / scGESTALT** — CRISPR scarring barcode

## Output

- Cell-to-clone assignment, lineage tree, fate prediction per clone

## Limitations

- ❌ Wet-lab setup 복잡
- ⚠ Barcode dropout in scRNA-seq
- ⚠ Limited barcode diversity (combinatorial 필요)

## References

- Weinreb C, et al. Lineage tracing on transcriptional landscapes links state to fate (LARRY). *Science* 2020.

---

**Lead**: Replisci · **Last updated**: 2026-05-19
