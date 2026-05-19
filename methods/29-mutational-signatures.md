# Cancer Mutational Signatures

**Category**: Genomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

종양 게놈의 somatic mutation 패턴을 SBS/DBS/ID/CN signature 으로 분해하여 발암 원인을 추론. UV exposure (SBS7), smoking (SBS4), APOBEC (SBS2/13), MMR deficiency (SBS6/15/21/26), HRD (SBS3) 등.

**누가 의뢰**: 임상 종양 driver/원인 추정, 면역치료 반응 예측 (TMB + MSI), HRD-PARP inhibitor 선정, drug-induced mutagenesis 평가.

## Input

- **Somatic variant calls**: tumor-normal VCF (Mutect2, Strelka2 등)
- **Coverage**: WES 100-200×, WGS 60-90×
- **Sample**: 1-10 sample (single) ~ 100+ cohort
- **Reference signatures**: COSMIC v3.4 (78 SBS, 11 DBS, 18 ID, 24 CN)

## Pipeline

```
Somatic VCF → SigProfilerMatrixGenerator (96-context matrix)
  → SigProfilerExtractor (de novo + COSMIC match)
  → or fitting only: deconstructSigs / MutationalPatterns
  → activity per sample → cohort heatmap
  → clinical correlation (TMB, MSI, HRD)
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **SigProfiler suite** | 1.3 | de novo extraction + COSMIC match |
| **deconstructSigs** | R 1.9 | fitting to known signatures |
| **MutationalPatterns** | R/Bioc 3.16 | comprehensive R package |
| **YAPSA / sigfit** | R | Bayesian fitting |
| **HRDetect** | R | HR deficiency score |
| **MSIsensor2** | 0.2 | MSI status |
| **TMB calculator** | – | tumor mutation burden |

## Output

- `results/`:
  - `signature_activities.tsv` — sample × signature contribution
  - `mutational_context_matrix.tsv` — 96 SBS contexts
  - `cosmic_match.tsv` — top etiology match
  - `tmb_msi.tsv` — TMB (mut/Mb), MSI status
  - `hrd_score.tsv`
- `figures/`:
  - `sbs_96_context.png`
  - `signature_contributions.png` (stacked bar)
  - `cohort_heatmap.png`
  - `tmb_violin.png`
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/mutational-signatures:v1.0.0
docker run --rm -v $PWD:/work replisci/mutational-signatures:v1.0.0 \
  python run_sigprofiler.py vcfs/
```

## Demo Dataset

- TCGA-MELA (UV signature SBS7)
- TCGA-LUAD (smoking SBS4)
- COLO829 melanoma cell line (UV + tobacco mix)

## Time & Resources

| Stage | Wall-clock | CPU | RAM |
|-------|-----------|-----|-----|
| Single sample fitting | ~10 min | 4 cores | 8 GB |
| Cohort de novo extraction (n=100) | ~6 h | 16 cores | 64 GB |

## Limitations

- ❌ **Low mutation count** — < 100 mutations / sample 은 fitting 불안정. cohort 분석 권장
- ❌ **De novo signature 안정성** — small cohort 에서 새 signature 신뢰도 낮음. ≥30 sample 권장
- ⚠ **Signature overlap** — 유사한 context 다수. ID/CN signature는 정확도 낮음
- ⚠ **Tissue specificity** — 어떤 signature가 어떤 조직에서 흔한지 사전 정보 활용 권장
- ⚠ **TMB threshold** — 면역치료 cutoff (10 mut/Mb)는 panel 의존. WES-derived TMB 권장
- ⚠ **HRD score** — single signature(SBS3) 보다 multi-feature score (HRDetect)가 정확

## Quality Checks

- [x] Total somatic mutations per sample > 50
- [x] Tumor purity > 30% (signature 정확도)
- [x] Cosine similarity (extracted vs COSMIC) > 0.85
- [x] Known cause + signature 일치 (UV → SBS7 등)
- [x] MSI sensor + dMMR signature 일치

## References

- Alexandrov LB, et al. The repertoire of mutational signatures in human cancer. *Nature* 2020.
- Islam SMA, et al. Uncovering novel mutational signatures by de novo extraction with SigProfilerExtractor. *Cell Genomics* 2022.
- Rosenthal R, et al. DeconstructSigs: delineating mutational processes in single tumors. *Genome Biology* 2016.
- Davies H, et al. HRDetect: a mutational signature-based predictor of homologous recombination deficiency. *Nat Med* 2017.

**Database**: COSMIC Mutational Signatures v3.4 — https://cancer.sanger.ac.uk/signatures/

---

**Lead**: Replisci
**Last updated**: 2026-05-19
