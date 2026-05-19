# HLA Typing

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

**HLA (Human Leukocyte Antigen)** 의 6-digit / 8-digit allele 수준 typing — 면역치료 매칭, 자가면역 risk, 약물 hypersensitivity (HLA-B*57:01 → abacavir), 이식 적합성. NGS 기반 in-silico typing은 PCR-based typing 대비 cost-effective.

**누가 의뢰**: 이식의학·면역치료 (CAR-T HLA matching), 신경면역 (narcolepsy DQB1*06:02), pharmacogenomics, COVID/HBV 등 감염 association.

## Input

- **Data type**:
  - WGS / WES (정확도 ↑ for class I & II)
  - RNA-seq (mRNA 발현 기반, class I 비교적 정확)
  - Targeted HLA panel (best accuracy)
- **Coverage**: 30× WGS / 100× WES / 30M RNA-seq
- **Reference**: IPD-IMGT/HLA database (frequently updated)

## Pipeline

```
FASTQ/BAM → HLA-specific aligner / DB matcher
  → multi-tool consensus (OptiType + HLA-LA + Polysolver + HISAT-genotype)
  → 6-digit allele + class I/II
  → optional: pVACseq for neoantigen prediction
  → Report
```

| Tool | Version | Best for |
|------|---------|---------|
| **OptiType** | 1.3.5 | class I, WES/RNA-seq, very accurate |
| **HLA-LA** | 1.0.4 | WGS, class I+II, accurate |
| **Polysolver** | 4.0 | tumor WES (cancer immunotherapy) |
| **HISAT-genotype** | 1.3 | RNA-seq, fast |
| **arcasHLA** | 0.6 | RNA-seq, simple, accurate |
| (clinical) **Athlates / NGSengine** | – | targeted panel |
| **pVACseq** | 5.4 | neoantigen prediction (tumor) |

## Output

- `results/hla_calls.tsv` — class I (A, B, C) + class II (DPA, DPB, DQA, DQB, DRB)
- `results/multi_tool_consensus.tsv` — agreement
- `results/neoantigens.tsv` (tumor) — MHC binding predictions
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/hla-typing:v1.0.0
docker run --rm -v $PWD:/work replisci/hla-typing:v1.0.0 \
  snakemake --cores 8 --use-conda all
```

## Demo Dataset

- GIAB NA12878 (known HLA types via Sanger validation)
- 1000 Genomes Project samples (high-coverage WGS)
- Validation: vs published Sanger/PCR-SSP truth

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| WES single sample | ~30 min | 8 cores | 16 GB |
| WGS single sample | ~2 h | 16 cores | 32 GB |

## Limitations

- ❌ **6-digit vs 8-digit** — 보통 6-digit 신뢰. 8-digit (synonymous level) 은 specialized panel 필요
- ⚠ **Rare alleles** — 동아시아·아프리카 등 인구 특이 allele에서 정확도 낮을 수 있음. database 최신화 필수
- ⚠ **Class II 정확도 < Class I** — DRB3/4/5 paralogs 어려움
- ⚠ **Clinical decision** — 임상 결정은 PCR-SSP / Sanger validation 권장
- ⚠ **Phasing** — DR-DQ haplotype phasing은 long-read 또는 trio 필요

## Quality Checks

- [x] Multi-tool agreement > 90% for class I
- [x] Heterozygosity expected (대부분 het)
- [x] Population frequency consistency (IPD-IMGT)
- [x] Known sample → published allele match

## References

- Szolek A, et al. OptiType: precision HLA typing from next-generation sequencing data. *Bioinformatics* 2014.
- Dilthey AT, et al. HLA*LA — HLA typing from linearly projected graph alignments. *Bioinformatics* 2019.
- Shukla SA, et al. Comprehensive analysis of cancer-associated somatic mutations in class I HLA genes (Polysolver). *Nat Biotechnol* 2015.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
