# Small RNA-seq (miRNA, piRNA, tRF)

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

18-40 nt 짧은 non-coding RNA — microRNA (miRNA), piRNA, tRF (tRNA fragments), snoRNA — profiling. 발현 조절, 종양 biomarker (혈장 miRNA), germ cell biology.

**누가 의뢰**: 혈장/exosome miRNA biomarker, 종양 miRNA dysregulation, 정자/난자 piRNA, 임상 액체생검.

## Input

- **Library**: TruSeq Small RNA, NEXTFLEX, QIAseq miRNA — size selection 필수
- **Length**: 15-50 nt fragments
- **Read depth**: 5-15M / sample
- **UMI** 권장 (low-input)

## Pipeline

```
FASTQ → fastp (adapter, length filter 15-40 nt)
  → miRDeep2 (mapping + miRBase annotation + novel miRNA)
  → sRNAbench / mirtop (consensus across tools)
  → DESeq2 (DE)
  → miRNA target prediction (TargetScan, miRDB) + integration with mRNA
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **miRDeep2** | 2.0.1.3 | miRBase + novel miRNA |
| **sRNAbench** | – | small RNA profiling |
| mirtop | 0.4 | mirGFF3 standardization |
| miRBase | v22.1 | reference DB |
| piRNAbank / piPipes | – | piRNA |
| MINTmap | 2.0 | tRF |
| TargetScan | 8.0 | target prediction |
| miRDB | 6.0 | target prediction |

## Output

- miRNA × sample count, novel miRNA, DE results, target genes, Volcano, heatmap

## Demo / Time

- ENCODE small RNA dataset
- ~2 h per sample

## Limitations

- ❌ Adapter/length filter strict — small RNA 회수율 변동
- ⚠ Cross-mapping ambiguity (miRNA isomiR, near-identical)
- ⚠ Target prediction false positive 많음 — luciferase 검증 권장
- ⚠ Plasma/exosome miRNA hemolysis sensitive — QC 필수

## References

- Friedländer MR, et al. miRDeep2 accurately identifies known and hundreds of novel microRNA genes in seven animal clades. *NAR* 2012.
- Pliatsika V, et al. MINTmap: rapid and accurate profiling of nuclear and mitochondrial tRNA-derived RNA fragments. *Sci Reports* 2016.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
