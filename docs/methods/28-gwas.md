# Genome-Wide Association Study (GWAS)

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

수천~수십만 SNP × 수천~수십만 sample 에서 phenotype 과 유의 연관 변이를 찾는 분석. 흔한 질병 risk variant 발견, 약물 반응성 (PGx), 산업동물·작물 형질 마커 발굴.

**누가 의뢰**: 임상 cohort 유전 risk, 약물 반응 PGx, 농수산 육종 (GWAS + GS), bioGS-PRS 산업.

## Input

- **Genotype**: SNP array (Affy, Illumina Omni) 또는 WGS-imputed
- **Format**: PLINK (.bed/.bim/.fam), VCF, BGEN
- **Sample size**: ≥1,000 (effect size 큰 trait), 10,000+ (common traits), 100,000+ (complex traits)
- **Phenotype**: continuous (height, BMI) or binary (case-control)
- **Covariates**: age, sex, batch, PC1-10 (population structure)
- **Imputation reference**: TOPMed Bravo, 1000G, KBA (한국)

## Pipeline

```
Genotype QC: HWE, MAF > 0.01, call rate > 95%
Sample QC: missingness, het, relatedness (PI_HAT < 0.185)
Imputation (TOPMed/SOAPsnp): SNP density ↑ to ~10M
Population structure: PCA → top 10 PCs as covariates
Association test:
  ├─ PLINK2 (linear/logistic + firth)
  ├─ REGENIE (mixed model, biobank scale)
  ├─ SAIGE (rare variant, case-control imbalance)
  ↓
Manhattan + QQ plot
Genome-wide significance (p < 5e-8 for European, adjusted for other populations)
Fine-mapping (FINEMAP, SuSiE)
Functional annotation (FUMA, OpenTargets)
Pathway enrichment (MAGMA)
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **PLINK 2** | 2.0 alpha 6 | classic, fast |
| **REGENIE** | 4.1 | UK Biobank scale mixed model |
| **SAIGE** | 1.4 | imbalanced case-control |
| **BOLT-LMM** | 2.4 | linear mixed model |
| **GCTA** | 1.94 | heritability, GREML |
| **FUMA** | web | functional annotation, MAGMA |
| **LDSC** | 1.0 | LD-score regression |
| **FINEMAP / SuSiE** | – | fine-mapping |
| **Open Targets Genetics** | API | druggability |

## Output

- `qc/sample_qc.tsv`, `qc/snp_qc.tsv`
- `results/`:
  - `gwas_results.tsv` — SNP, beta, SE, p, MAF, INFO
  - `genome_wide_sig.tsv` — p < 5e-8
  - `loci_summary.tsv` — independent lead SNPs
  - `fine_mapping.tsv` — credible set
- `figures/`:
  - `manhattan.png`
  - `qq.png` — λ_GC
  - `pca_population.png`
  - `regional_plot.png` — per top locus
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/gwas:v1.0.0
docker run --rm -v $PWD:/work replisci/gwas:v1.0.0 \
  bash run_gwas.sh
```

## Demo Dataset

- 1000 Genomes Project + simulated phenotype
- 또는 UK Biobank summary statistics (public download — already analyzed traits)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| QC (10k samples × 1M SNPs) | ~2 h | 16 cores | 32 GB |
| Imputation (10k samples) | ~24 h (TOPMed server) | – | – |
| GWAS REGENIE (100k × 10M) | ~12 h | 64 cores | 128 GB |
| Fine-mapping single locus | ~30 min | 8 cores | 16 GB |

## Limitations

- ❌ **Sample size 의존** — 검정력 부족하면 lead SNP 발견 X. n>10,000 권장
- ❌ **Population stratification** — 인종 혼합 시 false positive. PC 보정 + LDSC 권장
- ❌ **Rare variant 한계** — MAF<1%는 표준 GWAS로 검출 어려움. SAIGE-GENE, exome-wide collapsing 필요
- ⚠ **GWAS hit ≠ causal variant** — LD에 의한 association. fine-mapping + functional validation 필요
- ⚠ **Cross-ancestry transferability** — 유럽 GWAS hit은 비-유럽 인구에서 효과 작을 수 있음
- ⚠ **Phenotype heterogeneity** — disease subtype 혼합은 효과 희석
- ⚠ **Multiple testing** — genome-wide 5e-8 표준. less strict는 false positive

## Quality Checks

- [x] λ_GC < 1.05 (population stratification minimal)
- [x] LDSC intercept ~1 (no inflation)
- [x] Genome-wide significant hits replicated in independent cohort
- [x] Lead SNP MAF, INFO score check
- [x] PC1-10 population stratification visualized
- [x] Per-chromosome lambda

## References

- Chang CC, et al. Second-generation PLINK: rising to the challenge of larger and richer datasets. *GigaScience* 2015.
- Mbatchou J, et al. Computationally efficient whole-genome regression for quantitative and binary traits (REGENIE). *Nat Genet* 2021.
- Zhou W, et al. Efficiently controlling for case-control imbalance and sample relatedness in large-scale genetic association studies (SAIGE). *Nat Genet* 2018.
- Watanabe K, et al. FUMA: a platform for functional mapping and annotation of genetic associations. *Nat Commun* 2017.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
