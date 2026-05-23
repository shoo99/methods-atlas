# Structural Variant Detection (Long-read)

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

50 bp 이상의 **구조변이** (deletion, insertion, duplication, inversion, translocation) 를 long-read sequencing으로 검출. Short-read가 놓치던 repeat-rich 영역, complex SV, transposable element insertion 을 정확히 잡는다. 임상 진단·인구유전·암 driver SV·식물 게놈학에 핵심.

**누가 의뢰**: 희귀질환 진단 (short-read negative), 인구 SV diversity 연구, 종양 SV (fusion, complex rearrangement), 식물·축산 육종 SV 마커.

## Input

- **Platform**:
  - **PacBio HiFi** (CCS) — Q20+, 15-25 kb
  - **Oxford Nanopore** — R10.4.1 sup, ultra-long (100kb+) 가능
- **Coverage**:
  - Germline: 20-30× (HiFi 20× → 99%+ SV recall)
  - Somatic: 30-60× tumor + 30× normal
- **Format**: BAM (HiFi unaligned) / FASTQ / POD5
- **Reference**: GRCh38 또는 T2T-CHM13 (T2T가 SV 검출에 더 정확)

## Pipeline

```
Long-read → minimap2/winnowmap2 (align)
  → Sniffles2 / SVision / cuteSV / Delly (SV calling)
  → Truvari / sv-eval (benchmarking)
  → bcftools + custom (filter, annotate)
  → VEP / AnnotSV (annotation)
  → SV class assignment (DEL/DUP/INV/INS/BND)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **minimap2** | 2.28 | long-read aligner |
| **winnowmap2** | 2.03 | repeat-aware alternative |
| **Sniffles2** | 2.5 | fast, well-validated SV caller |
| **SVision** / **SVisionPro** | 2.5 | deep-learning complex SV |
| **cuteSV** | 2.1 | high recall |
| **Delly2** (long-read mode) | 1.3 | classical |
| **PBSV** | 2.10 | PacBio official |
| **Severus** | 0.2 | somatic SV (tumor-normal) |
| **SAVANA** | 1.2 | somatic SV (alternative) |
| **Truvari** | 4.3 | SV benchmarking vs truth |
| **AnnotSV** | 3.4 | functional annotation |
| **SURVIVOR** | 1.0.7 | merge SV calls across callers |

## Output

- `qc/longread_stats.html` — N50, Q score
- `results/sv_calls.vcf.gz` — SV VCF (DEL, INS, DUP, INV, BND)
- `results/sv_annotated.tsv` — gene impact, OMIM, DGV
- `results/sv_per_class.tsv` — DEL/INS/DUP/INV counts
- `results/somatic_sv.vcf.gz` (cancer)
- `figures/`:
  - `sv_size_dist.png` — distribution by SV class
  - `circos.png` — genome view with translocations
  - `chord_translocations.png`
  - `sv_per_chr.png`
  - `igv_screenshot.png` (representative loci)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/long-read-sv:v1.0.0
docker run --rm -v $PWD:/work replisci/long-read-sv:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

## Demo Dataset

- **HG002** PacBio HiFi (GIAB) + curated SV benchmark (Tier 1 SV truth set)
- **HG002 ONT** R10.4.1 (Q20+)
- **COLO829** long-read (somatic SV cancer benchmark)
- Sensitivity/precision direct vs GIAB SV v0.6 truth set

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| Germline (1 sample, 30× HiFi WGS) | ~6 h | 32 cores | 64 GB |
| Somatic tumor-normal | ~12 h | 32 cores | 128 GB |
| Cohort joint calling (n=20) | ~24 h | 64 cores | 256 GB |

## Limitations

- ❌ **Coverage 부족** — 10× 미만 long-read는 SV recall 급락
- ❌ **Repeat 종류별 어려움** — VNTR/satellite는 여전히 어려움. specialized tool 필요 (TRGT)
- ⚠ **Caller 간 일치율 ~70%** — multi-caller + SURVIVOR merge 권장
- ⚠ **Insertion sequence resolution** — assembly-based가 더 정확 (e.g., hifiasm + assembly2VCF)
- ⚠ **Somatic VAF 낮은 SV** — subclonal SV는 deep coverage 필요
- ⚠ **Reference choice** — T2T-CHM13가 centromere/segdup 영역 SV에서 더 정확. GRCh38은 임상 호환성 ↑
- ⚠ **Pseudogene/repeat 영역 false positive** — manual review 필수

## Quality Checks

- [x] Mean coverage ≥ 20× (HiFi)
- [x] Read N50 ≥ 12 kb (HiFi) / 20 kb (ONT)
- [x] SV count 인구 평균 vs sample 비교 (excess = potential FP)
- [x] DEL/INS ratio ~1 (balanced for HiFi)
- [x] Mendel error rate (trio) < 1%
- [x] GIAB benchmark sensitivity/precision documented

## References

- Smolka M, et al. Detection of mosaic and population-level structural variants with Sniffles2. *Nat Biotechnol* 2024.
- Lin J, et al. SVision: Multi-object detection-based annotation of structural variants. *Nat Methods* 2022.
- Tham CY, et al. Severus: detection of somatic structural variation from long-read sequencing. *bioRxiv* 2024.
- Geoffroy V, et al. AnnotSV: an integrated tool for structural variation annotation. *Bioinformatics* 2018.

**Benchmark**
- Zook JM, et al. A robust benchmark for detection of germline large deletions and insertions. *Nat Biotechnol* 2020.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
