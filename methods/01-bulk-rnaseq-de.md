# Bulk RNA-seq Differential Expression

**Category**: Transcriptomics
**Tier**: 1
**Status**: ✅ Demo reproducible (`~/_clients/20260518_demo-airway/`)

## Overview

조건 간 (예: 처리 vs 대조, 환자 vs 정상) 유전자 발현 차이를 통계적으로 검정. Wet-lab의 가장 흔한 의뢰 — "어떤 유전자가 바뀌었는가?"에 답한다.

**누가 의뢰**: PI of wet-lab, 임상 연구자, 산업 R&D — 비-bioinformatician.

## Input

- **Data type**: Bulk RNA-seq paired-end (또는 single-end) FASTQ
- **Format**: FASTQ.gz from Illumina (NovaSeq / NextSeq)
- **Minimum sample size**: **≥3 replicate per condition** (n=3 vs 3 minimum, n=5+ 권장)
- **Read depth**: 20-30M reads / sample (DE 분석 기준)
- **Sample sheet**: `sample_id, condition, batch, replicate` (TSV)

## Pipeline

```
FastQC → fastp (trim) → STAR/Salmon (align/quantify) → tximport
  → DESeq2 (DE) → MultiQC + Volcano/MA/Heatmap/PCA → PDF report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| QC | FastQC | 0.12.1 | per-base quality |
| QC summary | MultiQC | 1.25 | 모든 단계 통합 |
| Trim | fastp | 0.23.4 | adapter + Q20 trim |
| Align | STAR | 2.7.11b | splice-aware aligner |
| (alt) Quant | Salmon | 1.10.3 | alignment-free (faster) |
| Count import | tximport | R/Bioconductor 1.34 | transcript→gene |
| DE | DESeq2 / pyDESeq2 | 1.46 / 0.5.0 | negative binomial GLM |
| Annotation | biomaRt / pyensembl | latest | gene symbols, biotypes |
| Figures | matplotlib + seaborn | 3.9 / 0.13 | publication-grade |
| Report | Pandoc + XeLaTeX | 3.6 / TL2024 | PDF with Noto Sans CJK |

## Output

- `qc/multiqc_report.html` — QC dashboard (FastQC, STAR, dup rates)
- `results/counts.tsv` — raw gene count matrix
- `results/normalized.tsv` — VST-normalized expression
- `results/de_results.tsv` — log2FC, lfcSE, pvalue, padj, gene symbols
- `figures/`:
  - `pca.png` — sample clustering
  - `ma_plot.png` — mean vs fold-change
  - `volcano.png` — significance vs effect size
  - `heatmap_top50.png` — top DE genes
  - `qc_metrics.png` — alignment summary
- `enrichment/` — GO BP, KEGG, Reactome (via gseapy/Enrichr)
- `report/report.pdf` — 종합 보고서 (한글/영문)

## Reproducible Environment

```bash
docker pull replisci/bulk-rnaseq-de:v1.0.0
docker run --rm -v $PWD:/work replisci/bulk-rnaseq-de:v1.0.0 \
  snakemake --cores 8 --use-conda all
```

또는 conda:
```bash
conda env create -f env.yml  # python 3.11, R 4.4, DESeq2, STAR, ...
conda activate replisci-rnaseq
make demo
```

## Demo Dataset

- **Accession**: GEO **GSE52778** — Himes et al. 2014, *PLoS One*
- **Samples**: 4 dexamethasone-treated + 4 control airway smooth muscle cells
- **Read count**: 8 × ~25M paired-end reads
- **Reference**: GRCh38 / Ensembl 110
- **Expected DEGs**: ~600 at padj<0.05, |log2FC|>1 (well-characterized)
- **Validation**: Replisci 데모 결과 — sensitivity 94.3%, precision 99.3% (vs published)

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo (8 samples, public counts) | ~5 min | 4 cores | 8 GB | 2 GB |
| End-to-end from FASTQ | ~2 h | 16 cores | 32 GB | 100 GB |
| Typical project (12-24 samples) | ~4 h | 16 cores | 32 GB | 200 GB |

## Limitations

- ❌ **n < 3 per group** — DESeq2의 dispersion 추정 불안정. n=2 가능하나 검정력 매우 낮음
- ❌ **Batch confounding** — condition과 batch가 완전 교란되면 통계적 분리 불가
- ⚠ **Reference 종 한정** — 인간/생쥐/래트 등 잘 annotation된 종. 비모델 생물은 [de novo transcriptome](08-denovo-transcriptome.md) 필요
- ⚠ **Isoform-level 분석** — Salmon으로 transcript-level 가능하나 별도 workflow ([rMATS, SUPPA2])
- ⚠ **Cell type 혼합** — bulk는 평균 신호. 세포 다양성 큰 조직은 [scRNA-seq](02-scrnaseq.md) 권장

## Quality Checks

- [x] Q30 > 80% per sample
- [x] STAR alignment rate > 85%
- [x] Ribosomal RNA contamination < 10%
- [x] PCA: samples cluster by condition (not batch)
- [x] Replicate Pearson correlation > 0.9
- [x] DESeq2 dispersion fit visually inspected
- [x] Independent biological validation (qPCR/Western) 권장

## References

**Tools**
- Love MI, Huber W, Anders S. Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. *Genome Biology* 2014.
- Dobin A, et al. STAR: ultrafast universal RNA-seq aligner. *Bioinformatics* 2013.
- Patro R, et al. Salmon provides fast and bias-aware quantification of transcript expression. *Nat Methods* 2017.

**Best practice**
- Bioconductor RNA-seq workflow: https://bioconductor.org/packages/release/workflows/vignettes/rnaseqGene/
- nf-core/rnaseq: https://nf-co.re/rnaseq

**Demo dataset**
- Himes BE, et al. RNA-Seq Transcriptome Profiling Identifies CRISPLD2 as a Glucocorticoid Responsive Gene that Modulates Cytokine Function in Airway Smooth Muscle Cells. *PLoS One* 2014.

---

**Lead**: Replisci
**Demo path**: `~/_clients/20260518_demo-airway/`
**Last updated**: 2026-05-19
