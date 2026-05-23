# DNA Methylation (Bisulfite-seq / EPIC array)

**Category**: Epigenomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

DNA 의 cytosine methylation (5mC) 상태를 게놈 전체에서 측정. **CpG island, enhancer, gene body** 의 methylation 변화를 발현·환경·질병과 연결.

- **WGBS** (Whole Genome Bisulfite Seq): 게놈 전체, 가장 비싸지만 완전
- **RRBS** (Reduced Representation BS): CpG-rich 영역만, 저비용
- **EM-seq** (NEB): bisulfite-free, DNA damage 적음, 최근 표준화 중
- **Illumina EPIC array**: 935k CpG sites 마이크로어레이, **임상 대규모 cohort 표준** (cheap, fast)

**누가 의뢰**: 임상 epigenetic biomarker (cancer methylation panel), 노화·환경 노출 연구, *in vitro* fertilization 임프린팅, 약물 epigenetic 효과.

## Input

- **WGBS/RRBS/EM-seq**: paired-end Illumina FASTQ
  - Coverage: WGBS 30×, RRBS 30-50× (CpG island), EM-seq 10-20× (효율 더 좋음)
- **EPIC array**: IDAT files (Illumina Methylation EPIC v2.0, 935k CpGs)
- **Sample sheet**: condition, age, sex, batch, slide_id (array는 batch critical)

## Pipeline (Bisulfite Sequencing)

```
FASTQ → fastp + Trim Galore (BS-specific)
  → Bismark / BS-Bolt (BS aligner) → MethylDackel (methylation calling)
  → methylKit / DSS (differential methylation: DMR, DMC)
  → Annotation (CpG island, gene body, enhancer)
  → Visualization (browser tracks, heatmaps)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Trim | Trim Galore | 0.6.10 | BS-aware adapter |
| Aligner | **Bismark** | 0.24 | BS gold standard |
| (alt) | BS-Bolt / Biscuit | 1.5 / 1.6 | faster |
| (alt EM-seq) | bwa-meth | 0.2.7 | direct mapping |
| Methyl call | MethylDackel | 0.6.1 | per-CpG calling |
| (alt) | Bismark methylation extractor | – | bundled |
| Filter | bcftools-like / custom | – | min coverage 5-10× |
| DMR | **methylKit** / DSS / **DMRseq** | R/Bioc | differential regions |
| (alt) | metilene | 0.2.8 | C++, fast |
| Annotate | ChIPseeker / annotatr | 1.42 / 1.32 | CpG island, gene |
| Visualization | methylKit, deepTools | – | tracks, heatmap |

## Pipeline (EPIC Array)

```
IDAT → minfi (read + preprocess)
  → Noob/SWAN normalization → QC (sex check, age check)
  → ChAMP (analysis framework) — type I/II bias correction
  → limma (linear model, batch correction)
  → DMP/DMR (probe-wise + region-wise differential)
  → CellMix deconvolution (혈액 cell composition)
  → Annotation (EPIC v2.0 manifest)
  → Report
```

| Step | Tool | Version |
|------|------|---------|
| Read IDAT | minfi | 1.52 |
| Preprocess | minfi (Noob, SWAN, BMIQ) | – |
| QC | minfiData, ChAMP | 2.36 |
| Cell composition | EpiDISH / FlowSorted refs | 2.22 |
| Differential | limma + champ.DMP | 3.62 / 2.36 |
| DMR | DMRcate / bumphunter | 3.0 / 1.48 |
| Age | DNAmAge (Horvath, Hannum, GrimAge) | – |
| Annotation | IlluminaHumanMethylationEPICmanifest | – |

## Output

- `qc/multiqc_report.html` (sequencing)
- `qc/array_qc.html` — sex prediction, detection p, β-density (array)
- `results/methylation.tsv` — per-CpG/probe β values
- `results/dmps.tsv` — differentially methylated probes (array)
- `results/dmrs.tsv` — differentially methylated regions (seq + array)
- `results/dna_age.tsv` — epigenetic age estimates (Horvath, Hannum)
- `results/cell_composition.tsv` — estimated cell-type proportions
- `figures/`:
  - `beta_density.png`
  - `pca.png`
  - `volcano_dmp.png`
  - `manhattan_dmp.png`
  - `dmr_heatmap.png`
  - `cpg_distribution.png` — promoter/island/shore
- `tracks/*.bigwig` (WGBS/RRBS)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/methylation:v1.0.0
docker run --rm -v $PWD:/work replisci/methylation:v1.0.0 \
  snakemake --cores 16 --use-conda all
```

## Demo Dataset

- **WGBS**: ENCODE H1-hESC WGBS (ENCSR888FON)
- **RRBS**: GEO **GSE27584** — Meissner classic
- **EPIC array**: GEO **GSE40279** (Hannum age cohort, 656 samples) — DNAmAge benchmark
- **Cancer methylation**: TCGA BRCA EPIC subset

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo array (24 EPIC samples) | ~30 min | 4 cores | 32 GB | 5 GB |
| Demo RRBS (4 samples) | ~3 h | 16 cores | 32 GB | 50 GB |
| WGBS project (8-12 samples) | ~12 h | 32 cores | 64 GB | 500 GB |
| EPIC cohort (200+ samples) | ~2 h | 16 cores | 128 GB | 20 GB |

## Limitations

- ❌ **저커버리지 WGBS** — CpG-resolution는 30× 권장. 10× 미만은 single-CpG 신뢰도 낮음
- ❌ **Bisulfite damage** — DNA degradation 큼. EM-seq가 대안
- ⚠ **PCR bias** — bisulfite converted DNA는 GC bias 큼. PCR-free 또는 low-cycle 권장
- ⚠ **EPIC array probe coverage 편향** — promoter/CpG island 위주. enhancer/gene body sparse
- ⚠ **Cell composition confounding** — 혈액 sample은 cell type 비율 차이가 변동의 주 원인. EpiDISH/CellMix로 보정 필수
- ⚠ **Age effect** — 모든 methylation 분석에서 age는 강한 covariate. 모델 포함 필수
- ⚠ **Imprinting/allelic effects** — bulk는 alleles 합산. allele-specific 분석은 별도 phasing 필요
- ⚠ **5hmC 구별 불가** — bisulfite는 5mC와 5hmC 구별 못 함. 구별 필요시 oxBS-seq / TAB-seq

## Quality Checks

- [x] Bisulfite conversion rate > 99% (spike-in lambda DNA)
- [x] Mean coverage at target (CpG sites) > 10×
- [x] **Array**: detection p < 0.01 for > 95% probes
- [x] **Array**: predicted sex matches metadata
- [x] **Array**: DNAmAge correlates with chronological age (Pearson > 0.9 expected)
- [x] β value distribution bimodal (0 and 1)
- [x] PCA: replicates cluster, batch corrected

## References

**Tools**
- Krueger F, Andrews SR. Bismark: a flexible aligner and methylation caller for Bisulfite-Seq applications. *Bioinformatics* 2011.
- Aryee MJ, et al. Minfi: a flexible and comprehensive Bioconductor package for the analysis of Infinium DNA methylation microarrays. *Bioinformatics* 2014.
- Akalin A, et al. methylKit: a comprehensive R package for the analysis of genome-wide DNA methylation profiles. *Genome Biology* 2012.
- Tian Y, et al. ChAMP: updated methylation analysis pipeline for Illumina BeadChips. *Bioinformatics* 2017.

**Age clocks**
- Horvath S. DNA methylation age of human tissues and cell types. *Genome Biology* 2013.
- Hannum G, et al. Genome-wide methylation profiles reveal quantitative views of human aging rates. *Molecular Cell* 2013.
- Lu AT, et al. DNA methylation GrimAge strongly predicts lifespan and healthspan. *Aging* 2019.

**Best practice**
- Bock C. Analysing and interpreting DNA methylation data. *Nat Rev Genet* 2012.
- ENCODE WGBS pipeline.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
