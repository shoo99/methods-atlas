# Replisci Methods Atlas

**Open catalog of 88 reproducible bioinformatics analysis methods.**

Built for wet-lab PIs, clinical researchers, and industry R&D teams who need to know — before commissioning analysis — **what's possible, what's not, and how it's done**.

[Browse all 88 methods :material-arrow-right:](categories.md){ .md-button .md-button--primary }
[Visit Replisci :material-launch:](https://replisci-landing-v2.vercel.app){ .md-button }

---

## 11 Categories · 88 1-pagers · 6,700+ lines

Each method documented with a **standard 1-pager**:

| Section | Purpose |
|---|---|
| Overview | One-line definition + who commissions it |
| Input | Data type, format, minimum N, QC requirements |
| Pipeline | Tool stack with version pins + step-by-step flow |
| Output | Deliverables — figures, tables, reports |
| Reproducible env | Docker image + one-line reproduction command |
| Demo dataset | Public GEO/ENCODE accession for validation |
| Time & resources | Wall-clock + CPU/RAM/disk estimates |
| **Limitations** | **Honest limits — when this method fails or is wrong** |
| Quality checks | Automated checks for analysis confidence |
| References | Tool papers + best-practice guidelines |

---

## Why open?

**Reproducibility and honest limitations are core to Replisci.** Before you engage us — or anyone — for omics analysis, you should be able to self-service review:

- Whether your data is suitable for the method
- What the standard pipeline looks like
- What deliverables to expect
- **What the method cannot do (limitations section is mandatory)**
- Time and cost rough order of magnitude

If a vendor cannot answer these in a 1-pager, ask why.

---

## Tier structure

<div class="grid cards" markdown>

-   **Tier 1 — Priority (10)**

    ---

    Cover ~80% of wet-lab and clinical requests. **Bulk RNA-seq DE** has a reproducible end-to-end Docker demo.

    [:octicons-arrow-right-24: Tier 1 methods](methods/01-bulk-rnaseq-de.md)

-   **Tier 2 — Specialized (48)**

    ---

    Frequently requested specialized methods — scATAC, CNV, fusion, HLA, CITE-seq, Visium, DIA-MS, GWAS, WGCNA, TF regulon, AlphaFold, MOFA, and more.

    [:octicons-arrow-right-24: Tier 2 methods](methods/11-scatac-multiome.md)

-   **Tier 3 — Long-tail (30)**

    ---

    Long-tail / high-difficulty / emerging methods — T2T assembly, pangenome, MERFISH, Xenium, cryo-EM, MD, XL-MS, deep multi-omics, and more.

    [:octicons-arrow-right-24: Tier 3 methods](methods/59-tandem-repeat-str.md)

</div>

---

## Categories

<div class="grid cards" markdown>

-   :material-dna: **① Genomics**

    WES/WGS variants · CNV · long-read SV · de novo assembly · T2T · pangenome · STR · HLA · GWAS · PRS · mutational signatures · cancer driver · phylogenomics · TCR/BCR

-   :material-chart-bell-curve: **② Transcriptomics**

    Bulk DE · de novo (Trinity) · isoform/splicing · gene fusion · long-read (Iso-Seq) · small RNA · circRNA · Ribo-seq · RNA editing · structure · nascent · stability

-   :material-cell: **③ Single-cell**

    scRNA · scATAC + multiome · CITE-seq · trajectory · cell-cell interaction · atlas mapping · Perturb-seq · lineage tracing · sc-WGBS

-   :material-map: **④ Spatial**

    Visium · spatial deconvolution · Stereo-seq · MERFISH · Xenium / CosMx · niche detection

-   :material-helix-coil: **⑤ Epigenomics**

    ATAC-seq · ChIP-seq · CUT&RUN/CUT&Tag · methylation (WGBS/EPIC) · Hi-C/Micro-C · HiChIP/ChIA-PET · DamID · ATAC footprinting · TF motif · ChromHMM

-   :material-protein: **⑥ Proteomics**

    DDA · DIA-MS · PTM (phospho/ubiq) · TMT multiplex · AP-MS / TurboID · XL-MS · top-down · Olink / SomaScan

-   :material-atom: **⑦ Metabolomics**

    Untargeted LC-MS · targeted MRM · lipidomics · NMR · ¹³C isotope tracing (flux)

-   :material-bacteria: **⑧ Microbiome**

    16S · ITS fungal · shotgun + MAGs · ARG profiling · virome · strain-level · metatranscriptomics

-   :material-circle-multiple: **⑨ Multi-omics**

    MOFA+ · DIABLO · SNF/Bayesian · deep learning (totalVI/CPA)

-   :material-pulse: **⑩ Functional / Clinical**

    GSEA pathway · WGCNA · TF regulon (SCENIC) · drug-gene + repurposing · PPI · survival/Cox · bulk cell-type deconvolution

-   :material-molecule: **⑪ Structural**

    AlphaFold 2/3 · ESMFold / OmegaFold · P-P docking · P-L docking · MD (GROMACS) · cryo-EM

</div>

---

## License

- **Documentation** (Markdown, prose, tables): [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- **Code** (future Dockerfiles, scripts, workflows): [MIT](https://opensource.org/licenses/MIT)

See [LICENSE](https://github.com/shoo99/methods-atlas/blob/main/LICENSE) for full terms.

---

## Source & contributions

- :material-github: [GitHub repository](https://github.com/shoo99/methods-atlas) — issues, suggestions, pull requests welcome
- :material-mail: shoo99@gmail.com

Maintained by **[Replisci](https://replisci-landing-v2.vercel.app)** — bioinformatics analysis + reproducible delivery for wet-lab, clinical, and industry R&D.
