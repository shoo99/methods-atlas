# De novo Genome Assembly (Long-read)

**Category**: Genomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

Reference 게놈이 없는 종 (비모델 생물, 신종 미생물, 임상 isolate) 의 게놈을 long-read sequencing 으로 *de novo* 조립. 최근 PacBio HiFi와 Oxford Nanopore Q20+ 의 등장으로 chromosome-level near-perfect assembly가 routine화 되었다.

**누가 의뢰**: 농수산 / 환경 신종 게놈, 미생물 isolate (병원성 / probiotic), 식물 게놈, 임상 ADR 균주 분석. **BioNexus와 일반 SaaS가 거의 다루지 않는 차별화 영역**.

## Input

- **Sequencing platform**:
  - **PacBio HiFi (CCS)** — Q20+ long reads, 15-25 kb 평균 (권장)
  - **Oxford Nanopore (R10.4.1)** — Q20+ super-accurate basecalling, ultra-long reads (100kb+) 가능
  - **Hybrid (optional)**: + Illumina short-read for polishing
  - **Scaffolding (optional)**: Hi-C, Bionano optical mapping
- **Coverage**:
  - **Microbial (~5 Mb)**: HiFi 50× / ONT 60×
  - **Plant/animal (1-3 Gb)**: HiFi 30× / ONT 40-50×
  - **Heterozygous diploid**: 1.5-2× 위 권장 (haplotype phasing)
- **Format**: BAM (HiFi unaligned) / FASTQ.gz / POD5 (ONT raw)

## Pipeline

```
Raw reads → QC (NanoPlot/LongQC) → assembler (hifiasm/Flye)
  → polishing (NextPolish/Medaka/Pilon) → contamination filter (FCS)
  → scaffolding (Hi-C: 3D-DNA/SALSA2) (optional)
  → assessment (BUSCO, QUAST, Merqury)
  → annotation (BRAKER3 / Prokka)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| QC | NanoPlot / LongQC | 1.43 / 1.20 | long-read QC |
| Assembler (HiFi) | **hifiasm** | 0.20.0 | gold standard for HiFi |
| Assembler (ONT) | **Flye** | 2.9.5 | de Bruijn graph for noisy |
| (alt ONT) | Canu | 2.2 | classic OLC |
| (alt small) | Unicycler | 0.5.1 | bacterial hybrid |
| Polish (ONT) | Medaka | 2.0.1 | ONT-aware DNN |
| Polish (hybrid) | NextPolish / Pilon | 1.4.1 / 1.24 | short-read polish |
| Decontam | NCBI FCS-GX | 0.5.5 | foreign contamination |
| Scaffolding | YaHS / 3D-DNA | 1.2 | Hi-C scaffolding |
| (alt) | SALSA2 | 2.3 | Hi-C scaffolding |
| Polish gap | RagTag | 2.1 | reference-guided fill |
| Assess completeness | **BUSCO** | 5.8 | conserved genes |
| Assess metrics | QUAST | 5.3 | N50, L50, gaps |
| Assess accuracy | Merqury | 1.3 | k-mer based QV |
| Annotation (eukaryote) | BRAKER3 | 3.0.8 | gene prediction |
| (alt) | MAKER | 3.01 | classic pipeline |
| Annotation (prokaryote) | Prokka / Bakta | 1.14 / 1.10 | bacterial |
| Repeat | RepeatModeler + RepeatMasker | 2.0.5 / 4.1.7 | TE annotation |
| tRNA | tRNAscan-SE | 2.0.12 | structural RNA |
| rRNA | barrnap | 0.9 | rRNA |

## Output

- `qc/longread_qc.html` — read length, Q score distribution
- `assembly/contigs.fa` — primary assembly
- `assembly/contigs_haplotype.fa` — haplotype 2 (HiFi-only)
- `assessment/`:
  - `busco_summary.txt` — % complete BUSCOs (target species lineage)
  - `quast_report.html` — N50, total length, gaps
  - `merqury_qv.txt` — QV score (consensus quality), k-mer completeness
- `scaffolds/scaffolds.fa` — Hi-C scaffolded (if applicable)
- `annotation/`:
  - `genes.gff3` — gene models
  - `proteins.fa` — predicted proteins
  - `functional.tsv` — eggNOG/Pfam annotations
- `figures/`:
  - `read_length_dist.png`
  - `assembly_dotplot.png` — vs related species
  - `busco_bar.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/denovo-genome:v1.0.0
docker run --rm -v $PWD:/work replisci/denovo-genome:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

또는 [nf-core/genomeassembler](https://nf-co.re/genomeassembler), VGP pipeline (Vertebrate Genome Project).

## Demo Dataset

- **Microbial**: *E. coli* K-12 PacBio HiFi (SRA **SRR11434954**) → chromosome-level closure
- **Eukaryote small**: *S. cerevisiae* HiFi (BioProject **PRJNA808910**) → 16 chromosome assembly + BUSCO 99%+
- **Plant**: VGP/EBP releases (e.g., *Arabidopsis* 등)
- **Validation**: 결과 BUSCO completeness, N50, QV 를 GenBank reference와 직접 비교

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Microbial (~5 Mb) | ~30 min | 16 cores | 32 GB | 50 GB |
| Yeast/fungal (~15 Mb) | ~2 h | 32 cores | 64 GB | 100 GB |
| Plant/animal (1-3 Gb) | ~24-72 h | 64 cores | **256-512 GB** | 2 TB |
| + Hi-C scaffolding | +6-12 h | 32 cores | 128 GB | – |

**RAM이 큰 게놈에서 병목** — 식물/동물 chromosome-level은 high-mem 서버 필요.

## Limitations

- ❌ **Polyploid/highly repetitive** — 5x+ ploidy 또는 80%+ repeat은 여전히 어려움. trio binning 또는 phased assembly 필요
- ❌ **Coverage 부족** — 권장 미만은 N50 급락, gap 다수. seq cost 절감보다 적정 coverage 우선
- ⚠ **ONT vs HiFi 정확도** — ONT는 polishing 후에도 homopolymer 오류 잔존. HiFi가 일반적으로 더 정확
- ⚠ **Annotation 신뢰도** — automated gene prediction은 비-모델 생물에서 50-70% 정확. 수동 큐레이션 필요
- ⚠ **Sample contamination** — host-microbe / 환경 시료는 contamination filter 필수
- ⚠ **Heterozygosity 처리** — diploid는 hifiasm `--hom-cov` 또는 trio binning 권장

## Quality Checks

- [x] BUSCO completeness (target lineage) > 90% (좋음), > 95% (탁월)
- [x] N50: microbial chromosome-level, eukaryote ≥ 1 Mb 권장
- [x] Number of contigs near expected chromosome count
- [x] Merqury QV > 40 (Q40 = 1 error/10kb), QV > 50 권장
- [x] No major contamination (FCS-GX clean)
- [x] Gene model BUSCO > 90% (annotation 단계)

## References

**Tools**
- Cheng H, et al. Haplotype-resolved de novo assembly using phased assembly graphs with hifiasm. *Nat Methods* 2021.
- Kolmogorov M, et al. Assembly of long, error-prone reads using repeat graphs (Flye). *Nat Biotechnol* 2019.
- Manni M, et al. BUSCO Update: Novel and Streamlined Workflows along with Broader and Deeper Phylogenetic Coverage. *Mol Biol Evol* 2021.
- Rhie A, et al. Merqury: reference-free quality, completeness, and phasing assessment for genome assemblies. *Genome Biology* 2020.

**Best practice**
- Vertebrate Genome Project (VGP) pipeline: https://github.com/VGP/vgp-assembly
- Earth BioGenome Project standards.

**Demo / Benchmarks**
- HiFi public datasets: https://downloads.pacbcloud.com/public/dataset/

---

**Lead**: Replisci
**Differentiator**: 비모델 생물 / 임상 isolate 게놈은 BioNexus 등 일반 SaaS가 다루지 않는 영역 — 본 메소드는 핵심 차별화 자산
**Last updated**: 2026-05-19
