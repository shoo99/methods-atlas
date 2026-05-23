# Methods Atlas — 전체 카테고리 인벤토리

> Replisci가 제공 가능한 모든 오믹스 분석 메소드의 인벤토리.
> ✅ = 1-pager 완성, 📝 = stub, 🔬 = 계획됨

각 메소드는 Tier 분류:
- **Tier 1**: 임상·wet-lab 80% 커버 (우선 1-pager + 재현 데모)
- **Tier 2**: 자주 의뢰되는 specialized 메소드
- **Tier 3**: long-tail / 고난도 / 신기술

---

## ① Genomics (DNA, 변이, 게놈 구조)

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| G1 | **WES / WGS variant calling** (germline + somatic) | 1 | ✅ | [06-wes-wgs-variant.md](methods/06-wes-wgs-variant.md) |
| G2 | **De novo genome assembly (long-read)** | 1 | ✅ | [07-denovo-genome.md](methods/07-denovo-genome.md) |
| G3 | **CNV detection (WES/WGS/array)** | 2 | ✅ | [12-cnv-detection.md](methods/12-cnv-detection.md) |
| G4 | **Structural variants (SV) — long-read** | 2 | ✅ | [13-long-read-sv.md](methods/13-long-read-sv.md) |
| G5 | **Tandem repeat / STR genotyping** | 3 | ✅ | [59-tandem-repeat-str.md](methods/59-tandem-repeat-str.md) |
| G6 | **Pangenome graph construction** | 3 | ✅ | [60-pangenome.md](methods/60-pangenome.md) |
| G7 | **Telomere-to-telomere (T2T) assembly** | 3 | ✅ | [61-t2t-assembly.md](methods/61-t2t-assembly.md) |
| G8 | **Phylogenomics / population genomics** | 2 | ✅ | [32-phylogenomics-popgen.md](methods/32-phylogenomics-popgen.md) |
| G9 | **GWAS** | 2 | ✅ | [28-gwas.md](methods/28-gwas.md) |
| G10 | **Polygenic risk score (PRS)** | 2 | ✅ | [31-prs.md](methods/31-prs.md) |
| G11 | **Cancer mutational signatures** | 2 | ✅ | [29-mutational-signatures.md](methods/29-mutational-signatures.md) |
| G12 | **HLA typing** | 2 | ✅ | [18-hla-typing.md](methods/18-hla-typing.md) |
| G13 | **TCR/BCR immune repertoire** | 2 | ✅ | [33-tcr-bcr-repertoire.md](methods/33-tcr-bcr-repertoire.md) |

## ② Bulk Transcriptomics (RNA)

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| T1 | **Bulk RNA-seq Differential Expression** | 1 | ✅ | [01-bulk-rnaseq-de.md](methods/01-bulk-rnaseq-de.md) |
| T2 | **De novo transcriptome assembly (Trinity)** | 1 | ✅ | [08-denovo-transcriptome.md](methods/08-denovo-transcriptome.md) |
| T3 | **Isoform / alternative splicing** | 2 | ✅ | [16-isoform-splicing.md](methods/16-isoform-splicing.md) |
| T4 | **Gene fusion detection** | 2 | ✅ | [17-fusion-detection.md](methods/17-fusion-detection.md) |
| T5 | **Long-read RNA (Iso-Seq, ONT direct)** | 2 | ✅ | [34-long-read-rna.md](methods/34-long-read-rna.md) |
| T6 | **Small RNA-seq (miRNA, piRNA, tRF)** | 2 | ✅ | [35-small-rna.md](methods/35-small-rna.md) |
| T7 | **circRNA detection** | 2 | ✅ | [36-circrna.md](methods/36-circrna.md) |
| T8 | **RNA editing (A→I, C→U)** | 3 | ✅ | [62-rna-editing.md](methods/62-rna-editing.md) |
| T9 | **RNA secondary structure (SHAPE-MaP)** | 3 | ✅ | [63-rna-structure.md](methods/63-rna-structure.md) |
| T10 | **Ribo-seq (translation profile)** | 2 | ✅ | [37-ribo-seq.md](methods/37-ribo-seq.md) |
| T11 | **Nascent transcription (GRO/PRO/NET-seq)** | 3 | ✅ | [64-nascent-transcription.md](methods/64-nascent-transcription.md) |
| T12 | **RNA stability / decay (SLAM-seq)** | 3 | ✅ | [65-rna-stability.md](methods/65-rna-stability.md) |

## ③ Single-cell

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| S1 | **scRNA-seq (10x, Smart-seq, Drop-seq)** | 1 | ✅ | [02-scrnaseq.md](methods/02-scrnaseq.md) |
| S2 | **scATAC-seq & Multiome** (combined) | 2 | ✅ | [11-scatac-multiome.md](methods/11-scatac-multiome.md) |
| S3 | (see S2) | – | – | – |
| S4 | **CITE-seq (RNA + surface protein)** | 2 | ✅ | [19-cite-seq.md](methods/19-cite-seq.md) |
| S5 | **Trajectory / pseudotime** | 2 | ✅ | [20-trajectory-analysis.md](methods/20-trajectory-analysis.md) |
| S6 | **Cell-cell interaction inference** | 2 | ✅ | [21-cell-cell-interaction.md](methods/21-cell-cell-interaction.md) |
| S7 | TCR/BCR + scRNA integration (see [33-tcr-bcr-repertoire.md](methods/33-tcr-bcr-repertoire.md)) | 2 | ✅ | – |
| S8 | **Perturb-seq / CRISPR screen scRNA** | 3 | ✅ | [66-perturb-seq.md](methods/66-perturb-seq.md) |
| S9 | **Lineage tracing (LARRY, CoSpar)** | 3 | ✅ | [67-lineage-tracing.md](methods/67-lineage-tracing.md) |
| S10 | **Single-cell methylation (sc-WGBS)** | 3 | ✅ | [68-sc-methylation.md](methods/68-sc-methylation.md) |
| S11 | **Reference mapping / atlas integration** | 2 | ✅ | [38-atlas-reference-mapping.md](methods/38-atlas-reference-mapping.md) |

## ④ Spatial Transcriptomics

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| SP1 | **10x Visium / Visium HD** | 2 | ✅ | [22-visium-spatial.md](methods/22-visium-spatial.md) |
| SP2 | **Stereo-seq (BGI)** | 3 | ✅ | [69-stereo-seq.md](methods/69-stereo-seq.md) |
| SP3 | **MERFISH / seqFISH** | 3 | ✅ | [70-merfish-imaging.md](methods/70-merfish-imaging.md) |
| SP4 | **Xenium / CosMx imaging** | 3 | ✅ | [71-xenium-cosmx.md](methods/71-xenium-cosmx.md) |
| SP5 | **Spatial deconvolution (cell-type mapping)** | 2 | ✅ | [39-spatial-deconvolution.md](methods/39-spatial-deconvolution.md) |
| SP6 | **Niche / spatial domain detection** | 3 | ✅ | [88-niche-domain-detection.md](methods/88-niche-domain-detection.md) |

## ⑤ Epigenomics (Chromatin, Methylation)

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| E1 | **ATAC-seq** | 1 | ✅ | [03-atacseq.md](methods/03-atacseq.md) |
| E2 | **ChIP-seq** | 1 | ✅ | [04-chipseq.md](methods/04-chipseq.md) |
| E3 | **DNA methylation (WGBS/RRBS/EPIC)** | 1 | ✅ | [10-methylation.md](methods/10-methylation.md) |
| E4 | **CUT&RUN / CUT&Tag** | 2 | ✅ | [14-cut-and-run.md](methods/14-cut-and-run.md) |
| E5 | **Hi-C / Micro-C (3D genome)** | 2 | ✅ | [40-hic-3d-genome.md](methods/40-hic-3d-genome.md) |
| E6 | **HiChIP / ChIA-PET** | 3 | ✅ | [72-hichip-chiapet.md](methods/72-hichip-chiapet.md) |
| E7 | **DamID / TaDa** | 3 | ✅ | [73-damid-tada.md](methods/73-damid-tada.md) |
| E8 | **TF motif analysis** | 2 | ✅ | [41-tf-motif.md](methods/41-tf-motif.md) |
| E9 | **ATAC footprinting (TOBIAS)** | 3 | ✅ | [74-atac-footprinting.md](methods/74-atac-footprinting.md) |
| E10 | **Chromatin state segmentation** | 2 | ✅ | [42-chromhmm-state.md](methods/42-chromhmm-state.md) |

## ⑥ Proteomics

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| P1 | **DDA shotgun MS** | 2 | ✅ | [23-dda-proteomics.md](methods/23-dda-proteomics.md) |
| P2 | **DIA-MS (SWATH, scanning DIA)** | 2 | ✅ | [24-dia-proteomics.md](methods/24-dia-proteomics.md) |
| P3 | **PTM analysis (phospho, ubiq, acetyl)** | 2 | ✅ | [43-ptm-proteomics.md](methods/43-ptm-proteomics.md) |
| P4 | **TMT / iTRAQ (multiplexed quant)** | 2 | ✅ | [44-tmt-multiplex.md](methods/44-tmt-multiplex.md) |
| P5 | **Affinity / pull-down MS (AP-MS / TurboID)** | 2 | ✅ | [45-affinity-pulldown.md](methods/45-affinity-pulldown.md) |
| P6 | **Cross-linking MS (XL-MS)** | 3 | ✅ | [75-xlms-crosslinking.md](methods/75-xlms-crosslinking.md) |
| P7 | **Top-down proteomics** | 3 | ✅ | [76-top-down-proteomics.md](methods/76-top-down-proteomics.md) |
| P8 | **Antibody arrays (Olink, SomaScan)** | 2 | ✅ | [46-olink-antibody-array.md](methods/46-olink-antibody-array.md) |
| P9 | Proteomics DE & pathway (covered in P1/P2) | 2 | ✅ | – |

## ⑦ Metabolomics

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| M1 | **Untargeted LC-MS metabolomics** | 2 | ✅ | [25-lcms-metabolomics.md](methods/25-lcms-metabolomics.md) |
| M2 | **Targeted LC-MS (MRM)** | 2 | ✅ | [47-targeted-metabolomics.md](methods/47-targeted-metabolomics.md) |
| M3 | **NMR metabolomics** | 3 | ✅ | [77-nmr-metabolomics.md](methods/77-nmr-metabolomics.md) |
| M4 | **Lipidomics** | 2 | ✅ | [48-lipidomics.md](methods/48-lipidomics.md) |
| M5 | **Stable isotope tracing (¹³C MFA)** | 3 | ✅ | [78-isotope-tracing.md](methods/78-isotope-tracing.md) |
| M6 | Metabolic pathway analysis (covered in M1) | 2 | ✅ | – |

## ⑧ Microbiome / Metagenomics

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| MB1 | **16S rRNA amplicon** | 1 | ✅ | [05-16s-microbiome.md](methods/05-16s-microbiome.md) |
| MB2 | **ITS fungal amplicon** | 2 | ✅ | [49-its-fungal.md](methods/49-its-fungal.md) |
| MB3 | **Shotgun metagenomics + MAGs** (combined) | 2 | ✅ | [15-shotgun-metagenomics.md](methods/15-shotgun-metagenomics.md) |
| MB4 | (see MB3 — function via HUMAnN3) | – | – | – |
| MB5 | (see MB3 — MAGs via metaSPAdes/CheckM2/GTDB-Tk) | – | – | – |
| MB6 | **Strain-level analysis** | 3 | ✅ | [79-strain-level-microbiome.md](methods/79-strain-level-microbiome.md) |
| MB7 | **Metatranscriptomics** | 3 | ✅ | [80-metatranscriptomics.md](methods/80-metatranscriptomics.md) |
| MB8 | **Antibiotic resistance gene (ARG) profiling** | 2 | ✅ | [50-arg-profiling.md](methods/50-arg-profiling.md) |
| MB9 | **Virome (DNA/RNA viruses)** | 2 | ✅ | [51-virome.md](methods/51-virome.md) |

## ⑨ Multi-omics Integration

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| MO1 | **MOFA+ (factor analysis)** | 2 | ✅ | [52-mofa-multiomics.md](methods/52-mofa-multiomics.md) |
| MO2 | **DIABLO (sparse PLS-DA)** | 2 | ✅ | [53-diablo-multiomics.md](methods/53-diablo-multiomics.md) |
| MO3 | **SNF + Bayesian integration** | 3 | ✅ | [81-snf-bayesian-integration.md](methods/81-snf-bayesian-integration.md) |
| MO4 | (see MO3) | 3 | ✅ | – |
| MO5 | **Deep learning multi-omics (totalVI/CPA)** | 3 | ✅ | [82-deep-multiomics.md](methods/82-deep-multiomics.md) |

## ⑩ Functional / Network / Drug

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| F1 | **GSEA / pathway enrichment (GO, KEGG, Reactome, Hallmark)** | 1 | ✅ | [09-gsea-pathway.md](methods/09-gsea-pathway.md) |
| F2 | **WGCNA co-expression network** | 2 | ✅ | [26-wgcna.md](methods/26-wgcna.md) |
| F3 | **TF regulon (DoRothEA, SCENIC, Lisa)** | 2 | ✅ | [27-tf-regulon.md](methods/27-tf-regulon.md) |
| F4 | **Drug-gene + repurposing** (combined) | 2 | ✅ | [54-drug-gene-repurposing.md](methods/54-drug-gene-repurposing.md) |
| F5 | (see F4) | 2 | ✅ | – |
| F6 | **PPI network (STRING, BioGRID)** | 2 | ✅ | [55-ppi-network.md](methods/55-ppi-network.md) |
| F7 | **Cancer driver gene detection** | 2 | ✅ | [56-cancer-driver.md](methods/56-cancer-driver.md) |
| F8 | **Survival / clinical association** | 2 | ✅ | [57-survival-clinical.md](methods/57-survival-clinical.md) |
| F9 | **Bulk cell-type deconvolution** | 2 | ✅ | [58-cell-type-deconvolution.md](methods/58-cell-type-deconvolution.md) |

## ⑪ Structural Biology / Computational

| # | 메소드 | Tier | Status | 1-pager |
|---|--------|------|--------|---------|
| ST1 | **AlphaFold 2/3 structure prediction** | 2 | ✅ | [30-alphafold.md](methods/30-alphafold.md) |
| ST2 | **ESMFold / OmegaFold** | 3 | ✅ | [83-esmfold-omegafold.md](methods/83-esmfold-omegafold.md) |
| ST3 | **Protein-protein docking (HADDOCK)** | 3 | ✅ | [84-protein-protein-docking.md](methods/84-protein-protein-docking.md) |
| ST4 | **Protein-ligand docking (Vina/DiffDock)** | 3 | ✅ | [85-protein-ligand-docking.md](methods/85-protein-ligand-docking.md) |
| ST5 | **Molecular dynamics (GROMACS)** | 3 | ✅ | [86-molecular-dynamics.md](methods/86-molecular-dynamics.md) |
| ST6 | **Cryo-EM single particle (RELION/cryoSPARC)** | 3 | ✅ | [87-cryoem-particle.md](methods/87-cryoem-particle.md) |

---

## 진행 통계 (2026-05-19)

| Tier | 메소드 수 | 1-pager 완료 | Docker 데모 |
|------|----------|-------------|------------|
| 1 | 10 | ✅ **10/10** | 1/10 (RNA-seq DE) |
| 2 | 48 | ✅ **48/48** | 0 |
| 3 | 30 | ✅ **30/30** | 0 |
| **합계** | **88** | ✅ **88/88** | **1** |

🎉 **Methods Atlas v1.0 — 모든 메소드 1-pager 완성**

다음 단계: Tier 1+2 Docker 재현 데모 (각 메소드 `make demo` 1-command 재현 가능 증명)

## 우선순위 (다음 단계)

**Tier 2 1-pager 우선 작성 (~ 6주)**
다음 가장 자주 의뢰될 메소드 — 5개 가속:
1. scATAC-seq + multiome
2. CNV detection (somatic + germline)
3. Long-read SV
4. CUT&RUN/CUT&Tag (low-input ChIP)
5. Shotgun metagenomics + MAGs

**Tier 1 재현 데모 (~ 4주)**
각 Tier 1 메소드에 대해 Docker + 공개 데이터 + `make demo` 1-command 재현 데모 구축. 우선:
1. scRNA-seq (10k PBMC)
2. ATAC-seq (ENCODE GM12878)
3. ChIP-seq (CTCF GM12878)
4. 16S (QIIME 2 Moving Pictures)
