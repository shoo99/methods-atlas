# CUT&RUN / CUT&Tag (Low-input Profiling)

**Category**: Epigenomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

ChIP-seq의 limit (대량 cell, 항체 의존성, fixation noise) 을 극복한 차세대 chromatin profiling. **항체-pA-MNase** (CUT&RUN) 또는 **항체-pA-Tn5** (CUT&Tag) 가 표적 단백질 주변 DNA를 *in situ* cleave/tagment 한다.

**장점**: 100-500 cells 가능, 낮은 background, 1/10 sequencing depth로 충분, 임상 sample (FFPE, biopsy) 가능.

**누가 의뢰**: 희귀 cell population profiling (CTC, sorted cell), 임상 sample histone mark, single-cell CUT&Tag, drug treatment time-course.

## Input

- **Data type**: paired-end Illumina FASTQ (25-50 bp 충분)
- **Targets**: histone marks (H3K27ac/me3, H3K4me3 등), TFs
- **Spike-in**: E.coli spike-in DNA (필수 — normalization 정확도 향상)
- **Cell count**: 100-100k (vs ChIP 1M+)
- **Read depth**: 3-8M reads / sample (ChIP의 1/5-1/10)
- **Replicates**: ≥2 biological replicates

## Pipeline

```
FASTQ → fastp → Bowtie2 (--very-sensitive, dovetail allowed)
  → samtools (filter, dedup) → spike-in normalization (E.coli reads)
  → SEACR (peak calling — CUT&RUN/Tag 특화)
  → IDR → consensus peaks → featureCounts → DESeq2
  → ChIPseeker, HOMER motif → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Trim | fastp | 0.23.4 | adapter |
| Align (target) | Bowtie2 | 2.5.4 | `--end-to-end --very-sensitive --no-mixed --no-discordant --phred33 -I 10 -X 700` |
| Align (spike) | Bowtie2 | – | `--no-overlap --no-dovetail` for E.coli |
| Filter | samtools | 1.21 | MAPQ ≥ 30, paired |
| Dedup | Picard / sambamba | 3.3 | optional (CUT&Tag has less PCR dup) |
| Normalize | spike-in scale factor | – | E.coli reads → bigWig scale |
| **Peak call** | **SEACR** | 1.4 | sparse data optimized |
| (alt) | MACS2 (`--nomodel`) | 2.2.9 | as alternative |
| Quant | featureCounts | 2.0.6 | – |
| DA | DESeq2 | 1.46 | differential |
| Annotation | ChIPseeker | 1.42 | – |
| Motif | HOMER, MEME | 4.11, 5.5 | – |
| Visualization | deepTools | 3.5.5 | bigWig, TSS profile |

## Output

- `qc/multiqc_report.html`
- `qc/spike_in_summary.tsv` — E.coli normalization factors
- `results/peaks.bed` — SEACR called peaks
- `results/da_results.tsv` — differential binding
- `figures/`:
  - `fragment_size.png` — nucleosomal vs sub-nucleosomal
  - `volcano_da.png`
  - `tss_profile.png`
  - `motif_top.png`
- `tracks/*.bigwig` — spike-normalized signal
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/cut-and-run:v1.0.0
docker run --rm -v $PWD:/work replisci/cut-and-run:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **CUT&RUN**: GEO **GSE126612** (Skene & Henikoff lab, H3K27me3 K562)
- **CUT&Tag**: GEO **GSE124557** (Kaya-Okur et al. K562 various marks)
- **Low-input**: GEO public 1k-cell CUT&Tag

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo (4 samples, 5M reads each) | ~1 h | 8 cores | 16 GB | 30 GB |
| Full project (8-12 samples) | ~3 h | 16 cores | 32 GB | 80 GB |

ChIP-seq보다 가벼움 — sequencing depth ↓, RAM ↓

## Limitations

- ❌ **Spike-in 의존** — 정확한 quantitative 비교에 E.coli spike-in 필수
- ❌ **항체 품질** — ChIP보다 background 낮지만 항체 specificity는 여전히 중요
- ⚠ **Peak caller** — SEACR 권장 (sparse 데이터). MACS2는 false positive 경향
- ⚠ **Fragment size pattern** — TF는 sub-nucleosomal (<120bp), histone은 nucleosomal (150bp+). 분리 분석 권장
- ⚠ **CUT&Tag의 Tn5 sequence bias** — sequence-dependent 편향. spike-in correction 필수
- ⚠ **Public ChIP과 직접 비교 어려움** — protocol/scale 다름. matched comparison 권장

## Quality Checks

- [x] Spike-in alignment rate 0.5-5% (정상 범위)
- [x] Target alignment rate > 80%
- [x] Fragment size: TF는 < 120bp peak, histone은 ~150bp 주기
- [x] FRiP > 5% (sharp), > 10% (broad)
- [x] Replicate Pearson correlation > 0.8

## References

- Skene PJ, Henikoff S. An efficient targeted nuclease strategy for high-resolution mapping of DNA binding sites (CUT&RUN). *eLife* 2017.
- Kaya-Okur HS, et al. CUT&Tag for efficient epigenomic profiling of small samples and single cells. *Nat Commun* 2019.
- Meers MP, Tenenbaum D, Henikoff S. Peak calling by Sparse Enrichment Analysis for CUT&RUN chromatin profiling (SEACR). *Epigenetics & Chromatin* 2019.

**Protocols**
- Henikoff lab CUT&RUN/CUT&Tag protocols on protocols.io
- 4DN consortium standards

---

**Lead**: Replisci
**Last updated**: 2026-05-19
