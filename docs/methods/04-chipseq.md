# ChIP-seq (Protein–DNA Binding)

**Category**: Epigenomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

특정 단백질 (TF, histone modification, chromatin regulator) 의 게놈 결합 위치를 측정. 항체로 면역침전(IP) 후 sequencing. ATAC가 "어디가 열려있나"를 본다면 ChIP-seq는 "거기에 무엇이 붙어있나"를 직접 본다.

**누가 의뢰**: 전사인자 메커니즘, histone mark profiling (H3K27ac/H3K4me3 등), epigenetic drug 평가, chromatin state mapping.

## Input

- **Data type**: ChIP-seq paired-end (또는 single-end) Illumina FASTQ
- **Control 필수**: Input DNA 또는 IgG mock IP (peak calling의 false-positive 제거에 필수)
- **Format**: FASTQ.gz, 50-150 bp
- **Minimum cells**: 100k-1M cells (TF), 10k-100k (histone)
- **Read depth**:
  - **Sharp peaks (TF, H3K4me3)**: 20M unique reads
  - **Broad peaks (H3K27me3, H3K36me3)**: 40-60M
- **Replicates**: ≥2 biological replicates 필수 (IDR)

## Pipeline

```
FASTQ → fastp → BWA-MEM2/Bowtie2 → samtools dedup
  → MACS2 (sharp) / SICER2 / epic2 (broad) — paired with input control
  → IDR → consensus peaks
  → featureCounts → DiffBind/DESeq2 (differential binding)
  → ChIPseeker + HOMER (annotation + motif)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Trim | fastp | 0.23.4 | adapter |
| Align | BWA-MEM2 | 2.2.1 | short reads |
| (alt) | Bowtie2 | 2.5.4 | very short reads |
| Dedup | Picard | 3.3 | PCR dup |
| Peak call (sharp) | MACS2 | 2.2.9 | `--call-summits` for TF |
| Peak call (broad) | MACS2 `--broad` | 2.2.9 | H3K27me3 etc |
| (alt broad) | epic2 / SICER2 | 0.0.52 | broad-mark optimized |
| IDR | idr | 2.0.4 | replicate reproducibility |
| Quant | featureCounts | 2.0.6 | peak × sample matrix |
| DB | DiffBind | 3.16 | differential binding |
| Annotate | ChIPseeker | 1.42 | genomic features |
| Motif | HOMER / MEME-ChIP | 4.11 / 5.5 | enriched motifs |
| Visualize | deepTools | 3.5.5 | profile/heatmap around TSS |

## Output

- `qc/multiqc_report.html`
- `qc/fingerprint.png` — deepTools plotFingerprint (IP enrichment vs input)
- `qc/cross_correlation.png` — phantompeakqualtools NSC/RSC scores
- `results/peaks_consensus.bed` — IDR-filtered
- `results/peak_matrix.tsv`
- `results/db_results.tsv` — differential binding
- `figures/`:
  - `tss_profile.png` — average signal around TSS
  - `tss_heatmap.png` — per-gene signal heatmap
  - `volcano_db.png` — differential peaks
  - `motif_top.png` — HOMER motifs
  - `peak_annotation.png` — distribution
- `tracks/*.bigwig` — normalized signal
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/chipseq:v1.0.0
docker run --rm -v $PWD:/work replisci/chipseq:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **Accession**: ENCODE **ENCSR000EUA** (CTCF ChIP-seq in GM12878) + matched input
- **Or**: GEO **GSE29611** — Histone mark ENCODE Roadmap data
- **Samples**: 2 IP replicates + 1 input control
- **Reference**: GRCh38

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo (3 samples, sharp TF) | ~2 h | 16 cores | 32 GB | 80 GB |
| Full project (8-16 samples) | ~6 h | 32 cores | 64 GB | 300 GB |

## Limitations

- ❌ **Input control 필수** — 없으면 peak의 신뢰도 매우 낮음. 의뢰 전 sequencing 단계에서 input 확보 필수
- ❌ **항체 품질 결정적** — 비특이적 결합은 false peaks 양산. ENCODE validated 항체 권장
- ⚠ **TF vs histone broad peak 알고리즘 차이** — MACS2 default는 sharp용. broad mark는 `--broad` 또는 epic2
- ⚠ **Peak ≠ functional binding** — 단순히 모티프 contains는 부족. ATAC/RNA-seq 통합 권장
- ⚠ **Low-input ChIP** — 100 cells 미만은 [CUT&RUN/CUT&Tag] (Tier 2) 권장
- ⚠ **In vivo ChIP** — fixation 효율 변동 → spike-in normalization (ChIP-Rx) 권장

## Quality Checks

- [x] NSC (Normalized Strand Cross-correlation) > 1.05
- [x] RSC (Relative Strand Cross-correlation) > 0.8
- [x] Fingerprint plot: IP curve clearly above input
- [x] FRiP > 1% (sharp) / 5% (broad)
- [x] IDR replicate concordance
- [x] Top peaks match expected motif (TF) or genomic distribution (histone)

## References

**Tools**
- Zhang Y, et al. Model-based analysis of ChIP-Seq (MACS). *Genome Biology* 2008.
- Ross-Innes CS, et al. Differential oestrogen receptor binding is associated with clinical outcome in breast cancer. *Nature* 2012 (DiffBind).

**Best practice**
- ENCODE ChIP-seq pipeline v2: https://github.com/ENCODE-DCC/chip-seq-pipeline2
- Bailey T, et al. Practical guidelines for the comprehensive analysis of ChIP-seq data. *PLoS Comput Biol* 2013.
- Park PJ. ChIP-seq: advantages and challenges of a maturing technology. *Nat Rev Genet* 2009.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
