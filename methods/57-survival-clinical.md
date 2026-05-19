# Survival / Clinical Association Analysis

**Category**: Functional / Clinical
**Tier**: 2
**Status**: 📝 1-pager

## Overview

유전자 발현 / 변이 / methylation / proteomics 신호와 **임상 결과** (OS, PFS, DSS, response) 의 연관 분석. Kaplan-Meier, Cox PH, time-dependent ROC, ML-based prognostic models.

**누가 의뢰**: 임상 cohort biomarker validation, drug response prediction, prognostic signature 검증.

## Input

- **Expression / variant / clinical matrix**: feature × sample
- **Clinical**: OS time, OS event, age, sex, stage, treatment, response
- **Sample**: ≥50 events 권장 (Cox 모델 검정력)
- **Validation cohort**: independent split

## Pipeline

```
Clinical + omics
  → univariate: log-rank (KM), Cox (HR, p)
  → multivariable Cox (adjusted for confounders)
  → high/low stratification (median, optimal cutoff via maxstat)
  → time-dependent ROC, C-index
  → ML prognostic: glmnet (Cox elastic net), randomForestSRC, DeepSurv
  → Calibration + decision curve analysis
  → Validation in independent cohort
  → Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **survival / survminer** | R 3.7 / 0.5 | KM, Cox, viz |
| **lifelines** | Python 0.30 | – |
| **glmnet** | R 4.1 | penalized Cox |
| **randomForestSRC** | R 3.3 | RSF |
| **DeepSurv / PyCox** | 0.3 | deep survival |
| **maxstat / cutpointr** | R | optimal cutpoint |
| **rms** | R 6.8 | Calibration |
| **pec** | R 2024 | prediction error curves |

## Output

- KM curves, Cox HR forest plot, time-ROC, C-index, prognostic model + validation, nomogram

## Demo / Time

- TCGA TPM + clinical → Cox PH per gene
- ~30 min – 2 h

## Limitations

- ❌ Censoring + competing risks — informative censoring 가정 검증
- ❌ Optimal cutpoint over-fitting — independent validation 필수
- ⚠ Cohort heterogeneity (stage, treatment) — stratification 필요
- ⚠ Proportional hazards 가정 위반 시 time-dependent Cox 권장

## References

- Therneau TM. A Package for Survival Analysis in R. *R Foundation* 2024.
- Katzman JL, et al. DeepSurv: personalized treatment recommender system using a Cox proportional hazards deep neural network. *BMC Med Res Methodol* 2018.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
