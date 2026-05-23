# Copy Number Variation (CNV) Detection

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

게놈 영역의 **복제수 변화** (deletion, duplication, amplification) 를 검출. 게놈 안정성·암 driver·태아 임상검사·발달지연·약물 반응성 등 다양한 임상/연구 응용.

**Data source**:
- **WGS** — 가장 정확, B-allele frequency + read depth 통합
- **WES** — 비용 효율, off-target/target depth 활용
- **SNP array** (Illumina Omni, Affy) — 임상 routine
- **Long-read** — large CNV/repeat-rich 영역 우수
- **scRNA / scWGS** — 종양 heterogeneity (inferCNV, CopyKAT)

**누가 의뢰**: 임상 유전학 (NIPT, 발달지연 microdeletion), 암 (HER2/MYC/MDM2 amplification), 유전육종학 (CNV-trait 연관), CHO/세포주 안정성 모니터링.

## Input

- **WGS**: 30× germline, 60-90× somatic (tumor-normal pair)
- **WES**: 100-300× target depth + matched normal (somatic)
- **SNP array**: IDAT/GTC files, manifest
- **Long-read**: PacBio HiFi 20-30× / ONT 30×
- **Sample metadata**: condition, tumor purity (somatic), trio/family pedigree

## Pipeline

```
[WES/WGS Germline]
BAM → CNVkit / GATK gCNV / ExomeDepth (WES) / Control-FREEC (WGS)
  → genotype CNV → annotate (DGV, gnomAD, ClinGen)
  → ACMG/ClinGen pathogenicity

[WES/WGS Somatic]
Tumor+Normal BAM → CNVkit / GATK CNV / FACETS / Sequenza
  → segment, log2 ratio + BAF → purity/ploidy
  → focal vs arm-level CNV → driver detection (GISTIC2)

[Array]
IDAT → GenomeStudio / PennCNV / ASCAT
  → CNV calls → quality filter → annotate
```

| Tool | Version | Use case |
|------|---------|----------|
| **CNVkit** | 0.9.11 | WES + WGS, fast, well-documented |
| **GATK gCNV** | 4.6.0 | WES germline (cohort) |
| **GATK CNV (somatic)** | 4.6.0 | tumor-normal WES/WGS |
| **Control-FREEC** | 11.6 | WGS, no matched normal possible |
| **ExomeDepth** | 1.1 | WES, between-sample comparison |
| **FACETS** | 0.16 | somatic WGS/WES with BAF |
| **Sequenza** | 3.0 | tumor purity + ploidy |
| **ASCAT** | 3.2 | array + WGS allele-specific |
| **GISTIC2** | 2.0.23 | cohort focal driver detection |
| **PennCNV** | 1.0.5 | SNP array |
| **inferCNV** | R 1.22 | tumor scRNA pseudoCNV |
| **CopyKAT** | 1.1 | scRNA-based, aneuploid vs diploid |
| **Sniffles2 / Delly** | 2.5 / 1.3 | long-read SV/CNV |

## Output

- `results/cnv_segments.tsv` — chr, start, end, log2_ratio, copy_number
- `results/cnv_annotated.tsv` — DGV/ClinVar/gnomAD overlap
- `results/purity_ploidy.tsv` (somatic) — tumor cellularity
- `results/gistic_lesions.tsv` (cohort) — focal amp/del peaks
- `figures/`:
  - `genome_view.png` — log2 ratio whole-genome
  - `chr_view.png` — per-chromosome
  - `baf_log2_scatter.png` — purity inference
  - `oncoprint.png` (cancer cohort)
  - `circos.png` — genome + CNV + SV
- `vcf/cnv.vcf.gz` — VCF formatted CNV
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/cnv:v1.0.0
docker run --rm -v $PWD:/work replisci/cnv:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **Germline WES**: GIAB NA12878 WES + Twist exome (known CNV calls)
- **Somatic WES**: COLO829 melanoma cell line (matched tumor-normal, public CNV truth set)
- **Array**: GSE21518 (Affy SNP6, breast cancer cohort)
- **Long-read**: HG002 PacBio HiFi (CMRG benchmark)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| WES single sample CNV | ~30 min | 8 cores | 16 GB |
| WGS single sample CNV | ~3 h | 16 cores | 32 GB |
| Tumor-normal somatic (WGS) | ~6 h | 32 cores | 64 GB |
| Cohort GISTIC2 (n=100+) | ~2 h | 8 cores | 32 GB |

## Limitations

- ❌ **Low purity tumor** — 30% 미만 purity는 somatic CNV 검출 어려움
- ❌ **GC bias** — read depth-based 방법은 GC normalization 필수 (CNVkit auto)
- ⚠ **Repeat regions** — short-read는 segmental duplication에서 noisy. long-read 권장
- ⚠ **Mosaic CNV** — low VAF mosaic은 검출 어려움. deeper coverage 또는 specialized tool
- ⚠ **Breakpoint resolution** — short-read CNV는 ±100bp 부정확. SV caller (Manta, Delly) 결합 권장
- ⚠ **Array vs sequencing 결과 차이** — 호환 안 됨. 동일 platform 비교 권장
- ⚠ **Clinical interpretation** — pathogenicity는 ACMG/ClinGen CNV 가이드라인 따름. VUS 많음

## Quality Checks

- [x] Median coverage ≥ target
- [x] CNVkit cnr median absolute deviation < 0.3
- [x] **Somatic**: tumor purity > 30%, ploidy estimate consistent
- [x] Known CNV (e.g., HER2 amplification in HER2+ samples) 검출
- [x] Cohort: GISTIC q-value < 0.25 for known drivers (e.g., MYC, CDKN2A)

## References

- Talevich E, et al. CNVkit: Genome-wide copy number detection and visualization. *PLoS Comput Biol* 2016.
- Mermel CH, et al. GISTIC2.0 facilitates sensitive and confident localization of the targets of focal somatic copy-number alteration. *Genome Biology* 2011.
- Shen R, Seshan VE. FACETS: allele-specific copy number and clonal heterogeneity analysis. *NAR* 2016.
- Wang K, et al. PennCNV: an integrated hidden Markov model for high-resolution copy number detection from SNP genotyping data. *Genome Res* 2007.
- Riester M, et al. PureCN: copy number calling and SNV classification using targeted short read sequencing. *Source Code Biol Med* 2016.

**Clinical guidelines**
- Riggs ER, et al. Technical standards for the interpretation and reporting of CNV in clinical settings (ACMG/ClinGen). *Genet Med* 2020.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
