# 16S rRNA Microbiome Profiling

**Category**: Microbiome
**Tier**: 1
**Status**: 📝 1-pager

## Overview

16S rRNA 유전자의 가변영역(V3-V4 등)을 amplicon sequencing → 미생물 community 구성을 종/속 수준에서 정량. Whole-shotgun보다 저렴하고 wet-lab 친화적이며 임상 microbiome 연구의 표준.

**누가 의뢰**: 임상 (장내 microbiome, vaginal, oral), 환경/농업 R&D, 식품 fermentation, 화장품 skin microbiome.

## Input

- **Data type**: 16S amplicon paired-end Illumina (MiSeq 2×300, NovaSeq)
- **Format**: Demultiplexed FASTQ.gz per sample
- **Variable region**: V3-V4 (가장 흔함), V1-V3, V4 only
- **Primers**: 515F-806R (Earth Microbiome), 341F-805R (V3-V4)
- **Minimum reads**: 10,000 / sample (after QC)
- **Sample sheet**: sample_id, group, body_site, time_point, batch

## Pipeline

```
FASTQ → DADA2 (denoise → ASV) [QIIME2 wrapper]
  → taxonomy (SILVA/GTDB) → phylogeny (MAFFT + FastTree)
  → alpha diversity (Shannon, Simpson, observed)
  → beta diversity (Bray-Curtis, UniFrac) → PERMANOVA
  → differential abundance (ANCOM-BC, MaAsLin2, LEfSe)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Pipeline framework | QIIME 2 | 2024.10 | reproducible amplicon |
| (alt) | Nephele / nf-core/ampliseq | – | hosted / Nextflow |
| Denoise | DADA2 | 1.34 | exact ASVs (not OTUs) |
| Taxonomy | SILVA / GTDB | 138.2 / R220 | reference database |
| Classifier | sklearn naive Bayes | 1.5 | trained on V3-V4 |
| Phylogeny | MAFFT + FastTree2 | 7.5 / 2.1 | tree for UniFrac |
| Alpha div | QIIME 2 diversity | 2024.10 | Shannon, Simpson, Chao1 |
| Beta div | UniFrac (weighted/unweighted) | – | community distance |
| Stats | PERMANOVA (vegan) | 2.6 | group differences |
| DA | ANCOM-BC | 2.8 | abundance bias correction |
| (alt) | MaAsLin2 | 1.20 | multivariable mixed models |
| Viz | ggplot2, phyloseq | 1.50 | community plots |

## Output

- `qc/multiqc_report.html` + `qc/dada2_summary.tsv`
- `results/asv_table.tsv` — ASV × sample counts
- `results/taxonomy.tsv` — taxonomic assignments (Kingdom→Species)
- `results/alpha_diversity.tsv` — per-sample Shannon, Simpson, Observed
- `results/beta_distance.tsv` — pairwise Bray-Curtis / UniFrac
- `results/permanova.tsv` — group differences
- `results/da_ancombc.tsv` — differentially abundant taxa
- `figures/`:
  - `rarefaction.png` — sampling depth adequacy
  - `alpha_boxplot.png` — diversity by group
  - `pcoa.png` — beta diversity ordination
  - `stacked_taxa.png` — phylum/genus composition
  - `da_volcano.png` — differential abundance
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/16s-microbiome:v1.0.0
docker run --rm -v $PWD:/work replisci/16s-microbiome:v1.0.0 \
  snakemake --cores 8 --use-conda all
```

## Demo Dataset

- **Accession**: QIIME 2 **Moving Pictures tutorial** (Caporaso et al., body sites time series)
- **Or**: SRA **PRJEB6070** — Human Microbiome Project subset
- **Samples**: 34 samples, 5 body sites, 2 individuals, time series
- **Why**: 가장 표준 reference dataset, QIIME 2 공식 튜토리얼

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Demo (34 samples) | ~40 min | 4 cores | 16 GB | 10 GB |
| Full project (50-100 samples) | ~3 h | 16 cores | 32 GB | 50 GB |

## Limitations

- ❌ **종-수준 해상도 제한** — 16S는 보통 속(genus) 수준까지 신뢰. species 구별이 필요하면 [shotgun metagenomics] (Tier 2)
- ❌ **Function 추정 한계** — 16S로는 기능 직접 측정 불가. PICRUSt2로 추정만 가능. 정확한 functional profile은 shotgun
- ⚠ **Primer bias** — 선택한 변역에 따라 community 결과 다름. 비교 시 동일 primer 필수
- ⚠ **Batch effect 큼** — DNA 추출 키트, 시약 lot, run 차이 큰 영향. negative control 필수
- ⚠ **저생체량 시료 contamination** — 시약 microbiome (kitome) 영향. decontam 패키지 필요
- ⚠ **Compositional data** — relative abundance는 합이 1로 고정. ANCOM-BC/ALDEx2 등 compositional-aware 방법 권장

## Quality Checks

- [x] DADA2 retention rate > 50% post-filter
- [x] Rarefaction curves plateau (충분한 sequencing depth)
- [x] Negative/PCR control 결과 분리됨
- [x] 동일 group replicates beta-distance < cross-group
- [x] Top taxa biologically plausible (e.g., gut: Bacteroidetes/Firmicutes 우세)

## References

**Tools**
- Bolyen E, et al. Reproducible, interactive, scalable and extensible microbiome data science using QIIME 2. *Nat Biotechnol* 2019.
- Callahan BJ, et al. DADA2: High-resolution sample inference from Illumina amplicon data. *Nat Methods* 2016.
- Lin H, Peddada SD. Analysis of compositions of microbiomes with bias correction (ANCOM-BC). *Nat Commun* 2020.

**Best practice**
- QIIME 2 Moving Pictures tutorial: https://docs.qiime2.org/
- Earth Microbiome Project protocols: https://earthmicrobiome.org/

**Demo**
- Caporaso JG, et al. Moving pictures of the human microbiome. *Genome Biology* 2011.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
