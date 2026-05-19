# {{Method Name}}

**Category**: {{Genomics / Transcriptomics / Single-cell / Spatial / Epigenomics / Proteomics / Metabolomics / Microbiome / Multi-omics / Functional / Structural}}
**Tier**: {{1 / 2 / 3}}
**Status**: {{📝 1-pager / 🐳 Docker ready / ✅ Demo reproducible}}

## Overview

한 줄 정의. 어떤 생물학적 질문에 답하는가, 누가 의뢰하는가.

## Input

- **Data type**: e.g., paired-end Illumina FASTQ
- **Format**: FASTQ.gz, BAM, VCF, h5ad, MatrixMarket 등
- **Minimum sample size**: e.g., ≥3 replicates per condition (DE 분석 기준)
- **Quality requirements**: e.g., Q30 ≥ 80%, no adapter contamination
- **Sample sheet**: 필요 metadata (condition, batch, time point 등)

## Pipeline

```
QC → Trim → Align → Quantify → Statistical analysis → Visualization → Report
```

| Step | Tool | Version | Notes |
|------|------|---------|-------|
| 1. QC | FastQC + MultiQC | 0.12.1 / 1.25 | 모든 단계 wrap |
| 2. Trim | fastp | 0.23.4 | adapter + quality |
| ... | ... | ... | ... |

## Output

산출물 종류 (figure, table, report). 예시:

- `qc/multiqc_report.html` — QC 종합
- `results/counts.tsv` — gene-level count matrix
- `results/de_results.tsv` — log2FC, padj, gene symbols
- `figures/volcano.png`, `figures/heatmap.png`, `figures/pca.png`
- `report/report.pdf` — Pandoc + XeLaTeX 종합 보고서

## Reproducible Environment

```bash
# Docker
docker pull replisci/{{method-name}}:v1.0.0

# Or conda
conda env create -f env.yml
conda activate replisci-{{method-name}}

# Run demo
make demo  # 또는 snakemake --cores N
```

**Pinned versions**: 모든 도구는 `env.yml` / `Dockerfile`에 정확한 버전 명시.

## Demo Dataset

- **Accession**: e.g., GEO GSE52778 (airway smooth muscle, Himes et al. 2014)
- **Samples**: e.g., 4 treated + 4 control
- **Size**: e.g., 8 × ~25M paired-end reads
- **Why this dataset**: 잘 알려진 reference, 결과 재현 가능, 공개 라이선스

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| End-to-end (demo) | ~30 min | 8 cores | 16 GB | 50 GB |
| Full project (typical) | ~4 h | 32 cores | 64 GB | 200 GB |

## Limitations

정직성 원칙 — 이 메소드의 **한계 / 부적합 케이스**:

- ❌ N < 3 일 때 통계 검정력 부족
- ❌ Batch confounding이 심하면 분리 불가
- ⚠ Reference genome 의존 (de novo 필요 시 별도 메소드)
- ⚠ ...

## Quality Checks

분석 결과의 신뢰도를 검증하는 자동 체크:

- [ ] QC pass: Q30 > 80%, alignment rate > 80%, ribosomal < 10%
- [ ] Replicate clustering by PCA
- [ ] Volcano plot symmetry (no batch bias)
- [ ] Independent biological validation suggested

## References

- 도구 논문: Love et al. 2014 (DESeq2), ...
- Best practice: ENCODE pipelines, Bioconductor workflows, ...
- 데모 출처: Himes et al. 2014, PLoS One

---

**Lead**: Replisci
**Last updated**: 2026-05-19
