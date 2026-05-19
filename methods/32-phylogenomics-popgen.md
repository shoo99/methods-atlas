# Phylogenomics & Population Genomics

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

종 간 (phylogenomics) 또는 인구 내 (popgen) 게놈 차이를 분석. 진화 관계, 자연선택, 인구 구조 추론, demographic history, introgression 검출.

**누가 의뢰**: 종 진화/계통, 작물·가축 육종 popgen, 보전유전학, 임상 microbe transmission 추적, 고생물 ancient DNA.

## Input

- **Phylogenomics**: multi-species genomes/transcriptomes (FASTA, GFF)
- **Population**: VCF per individual, ≥20 samples per population
- **Outgroup** 필수 (rooted tree)
- **Ascertainment-aware genotyping** (low-coverage popgen)

## Pipeline

```
[Phylogenomics]
Orthologs (OrthoFinder/BUSCO) → MSA (MAFFT) → trim (trimAl)
  → ML tree (IQ-TREE2, RAxML-NG) → species tree (ASTRAL)
  → divergence dating (BEAST2, MCMCTree)
  → gene tree-species tree reconciliation (TreeRecs)

[PopGen]
VCF → filter (MAF, HWE, missingness)
  → ADMIXTURE / fastSTRUCTURE — ancestry
  → PCA (smartPCA)
  → FST, π, Tajima's D, iHS (selscan, scikit-allel)
  → PSMC / SMC++ / dadi — demographic inference
  → Treemix / qpAdm — introgression
```

| Tool | Version | Purpose |
|------|---------|---------|
| OrthoFinder | 3.0 | ortholog inference |
| MAFFT | 7.5 | MSA |
| IQ-TREE2 | 2.3 | ML tree |
| RAxML-NG | 1.2 | alternative ML |
| ASTRAL-IV | 5.7 | species tree |
| BEAST2 | 2.7 | Bayesian dating |
| ADMIXTURE | 1.3 | ancestry |
| ANGSD | 0.94 | low-coverage popgen |
| PSMC / SMC++ | – | demographics |
| selscan | 2.0 | selection scan |
| Treemix / qpAdm | – | admixture graphs |
| scikit-allel | 1.3 | python popgen |

## Output

- Trees (Newick), MSA, FST/π plots, PCA, ancestry barplot, demographic history, selection scans

## Time

- Phylogenomics 50 species: ~3 days
- PopGen 100 ind WGS: ~12 h core analyses

## Limitations

- ❌ Long-branch attraction (LBA) — site selection 영향
- ⚠ Low-coverage popgen은 genotype likelihood (ANGSD) 권장 — hard calls 부정확
- ⚠ Reference bias — non-reference allele 검출 불리
- ⚠ Recombination 필요시 ARG-based (Relate, tsinfer)

## References

- Minh BQ, et al. IQ-TREE 2. *Mol Biol Evol* 2020.
- Korneliussen TS, et al. ANGSD: Analysis of Next Generation Sequencing Data. *BMC Bioinformatics* 2014.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
