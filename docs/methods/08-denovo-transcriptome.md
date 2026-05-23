# De novo Transcriptome Assembly

**Category**: Transcriptomics
**Tier**: 1
**Status**: 📝 1-pager

## Overview

Reference 게놈/transcriptome 이 없는 종에서 RNA-seq 데이터만으로 transcript 카탈로그를 *de novo* 조립. 비모델 생물 발현 연구, 비교 transcriptomics, 새로운 isoform 발견, lncRNA/circRNA 카탈로그 구축에 활용.

**누가 의뢰**: 농수산 비-모델 종 R&D, 환경/생태 transcriptomics, 비교진화 연구, fermentation/probiotic 균주 expression.

## Input

- **Data type**: Bulk RNA-seq paired-end Illumina (또는 long-read Iso-Seq for hybrid)
- **Tissue strategy**: 다양한 조직 / 발달 단계 / 처리 조건의 sample을 pooling — 가능한 모든 transcript capture 목표
- **Format**: FASTQ.gz, 100-150 bp paired-end
- **Read depth**: **합계 50-200M reads** (single sample 20-30M보다 더 깊게)
- **Strand library 권장**: dUTP / TruSeq Stranded — antisense/sense 구별
- **Long-read (옵션)**: PacBio Iso-Seq 또는 ONT direct cDNA — full-length transcript

## Pipeline

```
FASTQ → fastp + rRNA filter (SortMeRNA)
  → Trinity (또는 rnaSPAdes) → de novo assembly
  → CD-HIT-EST (redundancy reduction)
  → TransRate / BUSCO transcripts mode (assess)
  → TransDecoder (ORF prediction)
  → BLAST/DIAMOND vs SwissProt + eggNOG-mapper (annotation)
  → Trinotate (통합 functional annotation)
  → Salmon quant (per-sample 발현) → DESeq2 (DE)
  → Report
```

| Step | Tool | Version | Purpose |
|------|------|---------|---------|
| Trim | fastp | 0.23.4 | adapter |
| rRNA filter | SortMeRNA | 4.3.7 | rRNA contamination 제거 |
| **Assembler (short-read)** | **Trinity** | 2.15.2 | gold standard |
| (alt) | rnaSPAdes | 4.0 | faster, less RAM |
| (alt) | Bridger | 2014 | alternative |
| (alt long-read) | StringTie2 | 2.2.3 | hybrid with Iso-Seq |
| Reduce redundancy | CD-HIT-EST | 4.8.1 | 95% identity cluster |
| (alt) | EvidentialGene | tr2aacds | best transcript 선택 |
| Assess completeness | **BUSCO** (transcripts) | 5.8 | lineage-specific |
| Assess quality | TransRate / rnaQUAST | 1.0.3 / 2.2.3 | contig metrics |
| ORF prediction | TransDecoder | 5.7.1 | likely coding |
| Homology | DIAMOND (vs SwissProt) | 2.1.10 | functional ortholog |
| Annotation | eggNOG-mapper | 2.1.12 | GO, KEGG, COG |
| Integration | Trinotate | 4.0.2 | DB+homology+Pfam+TM |
| Quant | Salmon | 1.10.3 | alignment-free |
| DE | DESeq2 | 1.46 | (with tximport) |

## Output

- `qc/multiqc_report.html`
- `assembly/Trinity.fasta` — raw assembly (수십만 transcripts)
- `assembly/Trinity_clustered.fasta` — CD-HIT 95% reduced
- `assessment/busco_transcripts.txt` — BUSCO transcript mode
- `assessment/transrate_report.html` — per-contig quality
- `orfs/longest_orfs.pep` — TransDecoder protein
- `annotation/`:
  - `blast_swissprot.tsv` — best hit per transcript
  - `eggnog_annotations.tsv` — GO/KEGG terms
  - `trinotate_report.tsv` — 통합 annotation table
- `expression/`:
  - `salmon_quant/*` — per-sample TPM
  - `gene_trans_map.tsv` — gene-level rollup
  - `de_results.tsv` — DESeq2
- `figures/`:
  - `length_dist.png` — transcript length
  - `busco_bar.png`
  - `expression_pca.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/denovo-transcriptome:v1.0.0
docker run --rm -v $PWD:/work replisci/denovo-transcriptome:v1.0.0 \
  snakemake --cores 32 --use-conda all
```

## Demo Dataset

- **공식 Trinity 데모**: *Schizosaccharomyces pombe* RNA-seq (Trinity tutorial data, 작은 fission yeast)
- **혹은**: SRA **SRR493366-SRR493371** (Trinity reference paper data)
- **비모델 생물 예**: SRA **PRJNA****** — 임의 곤충/식물 RNA-seq
- **Validation**: BUSCO transcript completeness, BLAST hit ratio vs SwissProt

## Time & Resources

| Stage | Wall-clock | CPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| Yeast demo (~30M reads) | ~3 h | 16 cores | 64 GB | 50 GB |
| Insect/small eukaryote (~200M reads pooled) | ~24 h | 32 cores | **256 GB** | 500 GB |
| Plant (~500M reads) | ~3-5 days | 64 cores | **500+ GB** | 1 TB |

**Trinity는 RAM-heavy** — 1 GB RAM per 1M reads 권장. rnaSPAdes는 더 적게 사용.

## Limitations

- ❌ **Chimera artifacts** — de novo는 PE 짝 잘못 연결로 chimeric contig 생성 가능. CD-HIT + EvidentialGene으로 완화
- ❌ **Isoform 분리 어려움** — paralogs / repeat region에서 isoform-level 오해 가능. long-read (Iso-Seq) 통합 권장
- ⚠ **Redundancy** — 동일 transcript이 여러 contig로 (10x+ 추정). CD-HIT 또는 EvidentialGene 필수
- ⚠ **Coverage 부족 transcript 누락** — rare/low-expression은 assemble 실패. pooling 전략으로 보완
- ⚠ **Annotation 의존** — SwissProt에 homolog 없는 transcript는 unknown protein. de novo 종일수록 비율 ↑
- ⚠ **Splicing 정보 없음** — genome 부재로 intron 구조 모름. long-read 필요
- ⚠ **메모리** — 식물/동물 크기 transcriptome은 high-mem 서버 필수

## Quality Checks

- [x] BUSCO transcript completeness > 80% (target lineage)
- [x] Trinity Ex90N50 (top-90% expression의 N50) ≥ 1000 bp
- [x] % reads mapping back to assembly > 80% (representative)
- [x] DIAMOND vs SwissProt hit rate > 50% (잘 알려진 종) / > 20% (de novo)
- [x] Strand-specific library면 forward/reverse strand consistency
- [x] CD-HIT reduction: 50-80% transcript 감소 일반적

## References

**Tools**
- Grabherr MG, et al. Full-length transcriptome assembly from RNA-Seq data without a reference genome (Trinity). *Nat Biotechnol* 2011.
- Bushmanova E, et al. rnaSPAdes: a de novo transcriptome assembler and its application to RNA-Seq data. *GigaScience* 2019.
- Smith-Unna R, et al. TransRate: reference-free quality assessment of de novo transcriptome assemblies. *Genome Res* 2016.
- Bryant DM, et al. A tissue-mapped axolotl de novo transcriptome enables identification of limb regeneration factors. *Cell Reports* 2017 (Trinotate workflow).

**Best practice**
- Trinity wiki: https://github.com/trinityrnaseq/trinityrnaseq/wiki
- Hölzer M, Marz M. De novo transcriptome assembly: A comprehensive cross-species comparison. *GigaScience* 2019.

---

**Lead**: Replisci
**Differentiator**: 비-모델 생물 transcriptome 분석 — 일반 SaaS가 거의 다루지 않음
**Last updated**: 2026-05-19
