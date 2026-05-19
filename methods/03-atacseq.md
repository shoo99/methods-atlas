# ATAC-seq (Chromatin Accessibility)

**Category**: Epigenomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

Tn5 transposase가 open chromatin에 삽입되는 원리로 게놈 전체의 **접근 가능한 (open) 영역**을 측정. 어떤 enhancer/promoter가 활성화되어 있는지, TF binding site 변화를 추정한다.

**누가 의뢰**: 전사조절 메커니즘 연구, 세포 분화/리프로그래밍 PI, 종양 epigenetics R&D.

## Input

- **Data type**: ATAC-seq paired-end Illumina FASTQ (또는 omni-ATAC, scATAC)
- **Format**: FASTQ.gz, paired-end 50-150 bp
- **Minimum cells (bulk)**: 50,000 nuclei input (omni-ATAC), 결과 100k+ unique fragments
- **Read depth**: 30-50M paired reads (mappable, deduplicated)
- **Sample sheet**: condition, replicate, batch

## Pipeline

```
FASTQ → fastp → Bowtie2/BWA-MEM2 (align)
  → picard MarkDuplicates → samtools (filter MT, low MAPQ)
  → MACS2 / Genrich (peak calling)
  → IDR (replicate consistency) → consensus peaks
  → featureCounts (peak × sample matrix) → DESeq2 (differential)
  → ChIPseeker (annotation) → HOMER/MEME (motif)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Trim | fastp | 0.23.4 | adapter |
| Align | Bowtie2 | 2.5.4 | short reads, ATAC-friendly |
| (alt) | BWA-MEM2 | 2.2.1 | faster |
| Dedup | Picard MarkDuplicates | 3.3 | PCR duplicates |
| Filter | samtools | 1.21 | MAPQ ≥ 30, no MT |
| Shift | deepTools alignmentSieve | 3.5.5 | +4/-5 Tn5 shift |
| Peak call | MACS2 | 2.2.9 | `--nomodel --shift -75 --extsize 150` |
| (alt) | Genrich | 0.6.1 | ATAC-aware |
| IDR | idr | 2.0.4 | replicate reproducibility |
| Quant | featureCounts | 2.0.6 | peak × sample matrix |
| DA | DESeq2 | 1.46 | differential accessibility |
| Annotate | ChIPseeker | 1.42 | promoter/distal/intronic |
| Motif | HOMER / MEME-ChIP | 4.11 / 5.5 | enriched TF motifs |
| Visualize | deepTools, IGV | 3.5 | bigWig tracks |

## Output

- `qc/multiqc_report.html` — FastQC, dup rates, library complexity
- `qc/fraglen_dist.png` — nucleosome periodicity (key ATAC QC)
- `qc/tss_enrichment.png` — TSS enrichment score (ENCODE standard)
- `results/peaks_consensus.bed` — IDR-filtered consensus peaks
- `results/peak_matrix.tsv` — peak × sample counts
- `results/da_results.tsv` — differential accessibility (DESeq2)
- `figures/`:
  - `pca_atac.png` — sample clustering
  - `volcano_da.png` — differential peaks
  - `heatmap_top_peaks.png`
  - `motif_enrichment.png` — HOMER top motifs
  - `annotation_pie.png` — promoter vs distal vs intron
- `tracks/*.bigwig` — normalized signal for IGV
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/atacseq:v1.0.0
docker run --rm -v $PWD:/work replisci/atacseq:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **Accession**: ENCODE **ENCSR000EOT** (GM12878 lymphoblastoid ATAC-seq)
- **Or**: GEO **GSE89212** — Buenrostro omni-ATAC original
- **Samples**: 2 biological replicates
- **Read count**: ~50M paired-end each
- **Reference**: GRCh38

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo (2 samples) | ~3 h | 16 cores | 32 GB | 100 GB |
| Full project (6-12 samples) | ~8 h | 32 cores | 64 GB | 400 GB |

## Limitations

- ❌ **MT contamination** — ATAC는 MT chromatin이 open. 30-50% reads 손실 일반적 → 충분한 sequencing depth 필요
- ❌ **Duplicates** — low input일수록 PCR dup 비율 ↑. ENCODE 기준 NRF ≥ 0.8 권장
- ⚠ **Peak caller 선택** — MACS2 default는 ChIP용. ATAC는 `--shift -75 --extsize 150 --nomodel` 권장
- ⚠ **TF binding 직접 측정 아님** — open chromatin은 TF 결합 가능성을 시사할 뿐. 직접 결합은 [ChIP-seq](04-chipseq.md) / CUT&RUN
- ⚠ **scATAC**은 별도 workflow (ArchR, Signac) — 본 1-pager는 bulk 기준

## Quality Checks

- [x] Fragment length distribution: nucleosomal pattern (~150bp 주기) 확인
- [x] TSS enrichment > 7 (ENCODE 기준)
- [x] NRF (Non-Redundant Fraction) > 0.8
- [x] FRiP (Fraction Reads in Peaks) > 0.2
- [x] IDR replicate concordance > 0.5
- [x] PCA: replicates cluster, conditions separate

## References

**Tools**
- Zhang Y, et al. Model-based analysis of ChIP-Seq (MACS). *Genome Biology* 2008.
- Buenrostro JD, et al. Transposition of native chromatin for fast and sensitive epigenomic profiling. *Nat Methods* 2013.
- Corces MR, et al. An improved ATAC-seq protocol (omni-ATAC). *Nat Methods* 2017.

**Best practice**
- ENCODE ATAC-seq pipeline: https://github.com/ENCODE-DCC/atac-seq-pipeline
- Yan F, et al. From reads to insight: a hitchhiker's guide to ATAC-seq data analysis. *Genome Biology* 2020.

**Demo**
- ENCODE Project Consortium: https://www.encodeproject.org/

---

**Lead**: Replisci
**Last updated**: 2026-05-19
