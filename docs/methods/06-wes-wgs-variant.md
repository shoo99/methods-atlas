# WES / WGS Variant Calling

**Category**: Genomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

Whole-Exome (WES) 또는 Whole-Genome (WGS) sequencing 데이터에서 SNV / Indel / CNV / SV 검출 및 임상적 해석. Germline (rare disease) 과 Somatic (cancer) 두 분기로 나뉜다.

**누가 의뢰**: 임상 유전학자, 희귀질환 진단, 종양 mutation profiling, 동반진단 (CDx) R&D, 농축산 육종.

## Input

- **Data type**: Illumina short-read paired-end (또는 Element Aviti, MGI)
- **WES targets**: Twist Comprehensive Exome, Agilent SureSelect, IDT xGen
- **Format**: FASTQ.gz, paired-end 100-150 bp
- **Coverage**:
  - **WES**: 100× mean (germline), 200-500× (somatic deep)
  - **WGS**: 30× (germline), 60-90× (somatic)
- **Trio / family**: pedigree file 필요 (rare disease)
- **Tumor-normal**: paired sample 권장 (somatic accuracy)

## Pipeline

```
FASTQ → fastp → BWA-MEM2 (align to GRCh38)
  → Picard MarkDuplicates → BQSR (GATK)
  → [Germline] HaplotypeCaller → joint genotyping → VQSR
  → [Somatic] Mutect2 (tumor+normal) → FilterMutectCalls + PoN
  → bcftools / vcftools (filter) → VEP / ANNOVAR (annotate)
  → ClinVar / OncoKB / COSMIC (interpretation)
  → CNV: CNVkit / GATK CNV / Control-FREEC
  → SV: Manta / Delly / Lumpy
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Align | BWA-MEM2 | 2.2.1 | short-read aligner |
| (alt) | DRAGMAP | 1.3 | GATK 권장 aligner |
| Dedup | Picard MarkDuplicates | 3.3 | PCR/optical dup |
| BQSR | GATK | 4.6.0 | base quality recalibration |
| Germline SNV/Indel | GATK HaplotypeCaller | 4.6.0 | local de novo assembly |
| (alt) | DeepVariant | 1.8 | DNN-based, 더 정확 |
| Joint | GenotypeGVCFs | 4.6.0 | cohort-level |
| Filter germline | VQSR / hard filter | 4.6.0 | quality filter |
| Somatic SNV/Indel | Mutect2 | 4.6.0 | tumor vs normal |
| (alt somatic) | Strelka2 | 2.9.10 | fast |
| PoN | CreateSomaticPanelOfNormals | 4.6.0 | technical noise filter |
| CNV (WES) | CNVkit | 0.9.11 | exome-friendly |
| CNV (WGS) | GATK CNV / Control-FREEC | – | depth + BAF |
| SV (WGS) | Manta + Delly | 1.6 / 1.3 | structural variants |
| Annotate | VEP / ANNOVAR | 112 / 2024 | functional impact |
| Clinical | ClinVar, OncoKB, COSMIC | latest | pathogenicity |
| Filter | bcftools | 1.21 | population freq, depth |

## Output

- `qc/multiqc_report.html` — coverage, dup rate, error rate
- `qc/coverage_summary.tsv` — % targets at 20×, 30×, 100×
- `results/germline.vcf.gz` — annotated germline VCF
- `results/somatic.vcf.gz` — annotated somatic VCF
- `results/cnv.tsv` + `figures/cnv_segments.png`
- `results/sv.vcf.gz` — structural variants
- `results/clinical_report.tsv` — ClinVar pathogenic + ACMG criteria
- `results/oncoprint.png` (cancer)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/wes-wgs:v1.0.0
docker run --rm -v $PWD:/work replisci/wes-wgs:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

또는 nf-core/sarek 사용 (production-grade Nextflow).

## Demo Dataset

- **Germline WES**: Genome in a Bottle **NA12878** (HG001) WES + Twist exome
- **Somatic WES**: TCGA SKCM small subset 또는 Illumina iGenomes COLO829 (matched tumor-normal cell line)
- **WGS**: HG002/NA24385 (Ashkenazim trio)
- **Why**: GIAB benchmark truth set 으로 sensitivity/precision 직접 측정 가능

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| WES germline (1 sample, 100×) | ~4 h | 16 cores | 32 GB | 50 GB |
| WES tumor-normal | ~8 h | 16 cores | 32 GB | 100 GB |
| WGS germline (1 sample, 30×) | ~12 h | 32 cores | 64 GB | 200 GB |
| WGS somatic (tumor-normal) | ~20 h | 32 cores | 64 GB | 400 GB |

## Limitations

- ❌ **저커버리지 sample** — 20× 미만은 sensitivity 급락, 30× 이상 권장 (WGS germline)
- ❌ **WES → CNV/SV 제한** — exome은 intergenic 정보 없어 SV 검출 한계. WGS 필요
- ⚠ **Reference bias** — GRCh38이 표준이나 인구 특이적 reference (T2T-CHM13, Korean1K) 사용 시 결과 차이
- ⚠ **VAF 낮은 somatic** — VAF < 5% mutation은 deep sequencing (500×+) 또는 ddPCR 검증 필요
- ⚠ **VUS 해석** — pathogenicity 해석은 ACMG 가이드라인 따르나 최종 결정은 임상의/유전상담
- ⚠ **GDPR/개인정보** — variant data는 식별 정보. 해외 cloud 사용 시 사전 검토 필수
- ⚠ **Repeat region** — short-read는 STR/long-tandem repeat 정확도 낮음. long-read 필요

## Quality Checks

- [x] Mean coverage ≥ target (WES 100×, WGS 30×)
- [x] % target at 20× > 95% (WES)
- [x] Duplicate rate < 20%
- [x] Ti/Tv ratio: WGS ~2.0, WES ~3.0 (over-call detection)
- [x] Het/Hom ratio reasonable (~1.5)
- [x] PCR-free library 권장 (Illumina TruSeq DNA PCR-free)
- [x] GIAB benchmark sensitivity/precision (사내 validation)

## References

**Tools**
- Li H. Aligning sequence reads, clone sequences and assembly contigs with BWA-MEM. *arXiv* 2013.
- Poplin R, et al. A universal SNP and small-indel variant caller using deep neural networks (DeepVariant). *Nat Biotechnol* 2018.
- Benjamin D, et al. Calling Somatic SNVs and Indels with Mutect2. *bioRxiv* 2019.
- McLaren W, et al. The Ensembl Variant Effect Predictor (VEP). *Genome Biology* 2016.

**Best practice**
- GATK Best Practices: https://gatk.broadinstitute.org/
- nf-core/sarek: https://nf-co.re/sarek
- ACMG/AMP 2015 variant interpretation guidelines.

**Demo / Benchmarks**
- Genome in a Bottle Consortium: https://www.nist.gov/programs-projects/genome-bottle
- Wagner J, et al. Curated variant benchmark sets for GIAB. *Nat Biotechnol* 2022.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
