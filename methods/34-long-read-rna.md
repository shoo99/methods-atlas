# Long-read RNA-seq (Iso-Seq / ONT Direct cDNA / Direct RNA)

**Category**: Transcriptomics
**Tier**: 2
**Status**: 📝 1-pager

## Overview

Long-read sequencing 으로 full-length transcript 1분자 단위 측정. Short-read의 isoform 모호성 해결, novel transcript 발견, RNA modification 측정 (direct RNA), allele-specific isoform.

**누가 의뢰**: cancer fusion 정확한 구조, neuro disease isoform, plant/insect biological discovery, novel isoform atlas, m6A epitranscriptomics.

## Input

- **PacBio Iso-Seq** (CCS): full-length cDNA, 평균 1-5 kb
- **ONT direct cDNA / direct RNA**: native RNA modification + transcript
- **Coverage**: 1-3M CCS / sample (Iso-Seq); 5-10M reads (ONT)

## Pipeline

```
[PacBio Iso-Seq]
Raw → ccs → primer/polyA removal (lima/IsoSeq3 refine) → cluster → polished isoforms
  → SQANTI3 QC + classification → TALON or FLAIR (transcript ID)
  → annotation (vs GENCODE) → quantification (kallisto / Salmon long-read)

[ONT direct RNA/cDNA]
POD5 → basecaller (dorado) → minimap2 (-x splice)
  → FLAIR / StringTie2 long → isoform discovery
  → NanoCount / Bambu — quantification
  → m6A: m6Anet / xPore (direct RNA only)
```

| Tool | Version | Purpose |
|------|---------|---------|
| IsoSeq3 / ccs | 4.0 | PacBio Iso-Seq processing |
| SQANTI3 | 5.3 | isoform QC |
| TALON | 6.0 | transcript annotation |
| **FLAIR** | 2.1 | full-length isoform ID |
| StringTie2 | 2.2 | reference-guided |
| Bambu | R/Bioc 3.6 | LR quantification |
| NanoCount | 1.1 | ONT quant |
| dorado | 0.9 | ONT basecaller |
| m6Anet | 2.1 | m6A from direct RNA |
| xPore | 2.1 | RNA modification |

## Output

- Full-length isoform GTF, transcript-level counts, RNA modification calls, isoform switching

## Demo / Time

- PacBio Iso-Seq public WTC11 reference (LRGASP benchmark)
- ~6 h per sample

## Limitations

- ❌ Coverage 비용 — Illumina 대비 비쌈
- ❌ ONT direct RNA basecalling error rate 높음 (5-10% per base)
- ⚠ Short read quantification 보완 권장 (hybrid)
- ⚠ Polyadenylation site 정확도 변동

## References

- Tang AD, et al. Full-length transcript characterization of SF3B1 mutation in chronic lymphocytic leukemia reveals downregulation of retained introns (FLAIR). *Nat Commun* 2020.
- Pardo-Palacios FJ, et al. Systematic assessment of long-read RNA-seq methods for transcript identification and quantification (LRGASP). *Nat Methods* 2024.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
