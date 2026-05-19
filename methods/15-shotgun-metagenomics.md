# Shotgun Metagenomics & MAGs

**Category**: Microbiome
**Tier**: 2
**Status**: 📝 1-pager

## Overview

16S amplicon이 분류군 동정 위주라면 shotgun metagenomics는 community **전체 DNA**를 sequencing — 종-수준 분류, 기능 (KEGG/CAZyme/ARG), 균주 다양성, 신종 게놈 (MAGs, Metagenome-Assembled Genomes) 모두 가능.

**누가 의뢰**: 임상 microbiome (sepsis 신속진단, IBD/CRC 균주-수준), 환경/산업 (bioreactor, 식품 fermentation, 토양 carbon cycling), pharma microbiome modulator R&D.

## Input

- **Data type**: Illumina shotgun PE (150bp), 또는 PacBio HiFi / ONT (long-read MAGs)
- **Read depth**: 5-20 GB / sample (community 복잡도에 따라)
- **Host removal**: 인간/host genome 매핑된 reads 제거 필수 (특히 임상 시료)
- **Sample sheet**: condition, body_site, batch, host info

## Pipeline

```
FASTQ → fastp → host removal (Bowtie2 vs host) 
  ├─ Taxonomy: Kraken2/Bracken or MetaPhlAn4 (read-based)
  ├─ Function: HUMAnN3 → KEGG/MetaCyc/CAZyme/ARG
  └─ Assembly: metaSPAdes / MEGAHIT
      → binning (metaBAT2 + CONCOCT + MaxBin2) → DAS_Tool refine
      → CheckM2 (completeness/contamination)
      → GTDB-Tk (MAG taxonomy)
      → MAG annotation (Prokka/Bakta + DRAM)
  → differential abundance + functional comparison → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| Trim | fastp | 0.23.4 | adapter |
| Host removal | Bowtie2 vs host genome | 2.5 | – |
| **Taxonomy (read)** | **Kraken2 + Bracken** | 2.1.3 / 3.0 | k-mer based, fast |
| (alt) | **MetaPhlAn4** | 4.1 | marker-gene based, accurate |
| Function | **HUMAnN3** | 3.9 | pathway abundance |
| ARG | CARD/RGI + AMRfinderPlus | 3.3 / 4.0 | antibiotic resistance |
| Virulence | VFDB | 2024 | virulence factors |
| **Assembly** | **metaSPAdes** | 4.0 | high-quality |
| (alt assembly) | MEGAHIT | 1.2.9 | low-memory |
| Mapping | Bowtie2 | 2.5 | read recruitment |
| **Binning** | **metaBAT2 + CONCOCT** | 2.16 / 1.1 | – |
| Bin refine | DAS_Tool | 1.1.7 | best across binners |
| **MAG QC** | **CheckM2** | 1.0.2 | completeness/contamination |
| dRep | dRep | 3.5 | dereplicate |
| **MAG taxonomy** | **GTDB-Tk** | 2.4 + GTDB R220 | tree-based |
| Annotation | Bakta / Prokka | 1.10 / 1.14 | gene calling |
| Metabolism | DRAM | 1.5 | metabolic potential |
| Diff abundance | ANCOM-BC, MaAsLin2 | 2.8 / 1.20 | – |

## Output

- `qc/multiqc_report.html`
- `taxonomy/`:
  - `kraken_abundance.tsv` — species × sample
  - `metaphlan_abundance.tsv`
  - `krona_plot.html`
- `function/`:
  - `humann_pathway_abundance.tsv`
  - `cazyme_profile.tsv`
  - `arg_profile.tsv` — antibiotic resistance genes
- `assembly/`:
  - `contigs.fa` per sample
  - `bins/` — refined MAGs
  - `mag_quality.tsv` — completeness, contamination
  - `gtdbtk_taxonomy.tsv`
- `figures/`:
  - `stacked_taxa.png`
  - `pcoa_bray.png`
  - `pathway_heatmap.png`
  - `arg_heatmap.png`
  - `mag_phylogeny.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/shotgun-meta:v1.0.0
docker run --rm -v $PWD:/work replisci/shotgun-meta:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

또는 nf-core/mag (production Nextflow).

## Demo Dataset

- **Human gut**: HMP Mock Community (SRA **SRR172902** — known composition)
- **Or**: Zymo D6300 mock (genuine 8-bacteria + 2-yeast)
- **Validation**: 결과 species abundance 가 known ground truth와 비교 (precision/recall)

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Read-based taxonomy (1 sample, 10GB) | ~30 min | 16 cores | 64 GB | 100 GB |
| Assembly + MAGs (10 samples) | ~24 h | 32 cores | **256 GB** | 1 TB |
| HUMAnN3 functional (1 sample) | ~3 h | 16 cores | 32 GB | – |

**Assembly + binning은 RAM-heavy** — 종 복잡도 ↑ 시 RAM 폭증.

## Limitations

- ❌ **MAG completeness** — 모든 균주가 완전 회수 X. high-abundance > 1%만 high-quality MAG 가능
- ❌ **저생체량 시료** — host contamination 비율 ↑. deeper sequencing 필요
- ⚠ **Database currency** — Kraken DB (GTDB R220) 정확도 결정. 정기 update 필요
- ⚠ **Strain-level resolution** — read-based로는 한계. specialized tool (StrainPhlAn, inStrain)
- ⚠ **Functional 정확도** — gene 존재 ≠ 발현. metatranscriptomics 권장
- ⚠ **MAG chimera** — binning artifact. CheckM2 contamination > 5% 필터
- ⚠ **Eukaryotic microbes** — fungal/protist MAGs는 binning 어려움. specialized

## Quality Checks

- [x] Host removal rate (clinical: > 70% 일반적)
- [x] Assembly N50 > 5 kb
- [x] **MAG quality**: completeness > 70%, contamination < 5% (medium-quality)
- [x] **HQ MAGs**: completeness > 90%, contamination < 5%, 5S/16S/23S rRNA + 18 tRNAs
- [x] Mock community recovery > 95%
- [x] PCoA: replicates cluster

## References

**Tools**
- Wood DE, et al. Improved metagenomic analysis with Kraken 2. *Genome Biology* 2019.
- Beghini F, et al. Integrating taxonomic, functional, and strain-level profiling with bioBakery 3. *eLife* 2021 (MetaPhlAn4, HUMAnN3).
- Nurk S, et al. metaSPAdes: a new versatile metagenomic assembler. *Genome Res* 2017.
- Chklovski A, et al. CheckM2: a rapid, scalable and accurate tool for assessing microbial genome quality. *Nat Methods* 2023.
- Chaumeil PA, et al. GTDB-Tk v2: memory friendly classification with the genome taxonomy database. *Bioinformatics* 2022.

**Best practice**
- MIMAG standards (Bowers et al. *Nat Biotechnol* 2017) — Minimum Information about a Metagenome-Assembled Genome.
- nf-core/mag

---

**Lead**: Replisci
**Last updated**: 2026-05-19
