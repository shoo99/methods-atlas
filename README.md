# Replisci Methods Atlas

> 바이오인포매틱스 오믹스 분석 메소드 카탈로그
> 각 메소드는 **재현 가능한** 1-pager 형식 — Docker/conda 환경 + 공개 데모 데이터 + 예상 재현 시간 포함

**목적**
- Replisci가 제공 가능한 분석 메소드의 **공식 카탈로그**
- 의뢰자가 "어떤 분석이 가능한가"를 자가 확인할 수 있는 self-service 자료
- 재현성(reproducibility) wedge를 증명하는 포트폴리오

**구조**
- `methods/` — 각 메소드 1-pager (입력·파이프라인·산출·Docker·데모·한계)
- `_template.md` — 신규 1-pager 작성 템플릿
- `CATEGORIES.md` — 전체 카테고리·메소드 인벤토리 (Tier 1/2/3 진행 상태)

---

## Tier 1 — 임상·wet-lab 80% 커버 (우선 작성 중)

| # | 메소드 | 카테고리 | 상태 |
|---|--------|---------|------|
| 1 | [Bulk RNA-seq Differential Expression](methods/01-bulk-rnaseq-de.md) | Transcriptomics | ✅ 1-pager (데모 완료) |
| 2 | [Single-cell RNA-seq](methods/02-scrnaseq.md) | Single-cell | 📝 1-pager |
| 3 | [ATAC-seq](methods/03-atacseq.md) | Epigenomics | 📝 1-pager |
| 4 | [ChIP-seq](methods/04-chipseq.md) | Epigenomics | 📝 1-pager |
| 5 | [16S Microbiome](methods/05-16s-microbiome.md) | Microbiome | 📝 1-pager |
| 6 | [WES/WGS Variant Calling](methods/06-wes-wgs-variant.md) | Genomics | 📝 1-pager |
| 7 | [De novo Genome Assembly (long-read)](methods/07-denovo-genome.md) | Genomics | 📝 1-pager |
| 8 | [De novo Transcriptome (Trinity)](methods/08-denovo-transcriptome.md) | Transcriptomics | 📝 1-pager |
| 9 | [GSEA / Pathway Enrichment](methods/09-gsea-pathway.md) | Functional | 📝 1-pager |
| 10 | [DNA Methylation (Bisulfite/EPIC)](methods/10-methylation.md) | Epigenomics | 📝 1-pager |

---

## Tier 2 — 1-pager 완료 (48/48 ✅)

48개 메소드 모두 1-pager 작성됨 (`methods/11-58.md`).
다음 단계: Docker 재현 데모 (각 메소드 + 공개 데이터 + `make demo`).

## Tier 3 — Long-tail (향후)
T2T assembly, pangenome graphs, Stereo-seq, MERFISH/Xenium, cryo-EM, MD simulation, deep multi-omics integration 등.

전체 인벤토리: [CATEGORIES.md](CATEGORIES.md)

---

## 1-pager 표준 구조

각 메소드 문서는 다음 섹션을 포함합니다:

1. **Overview** — 한 줄 정의 + 누가 의뢰하는가
2. **Input** — raw data 종류, 포맷, 최소 N (replicate 등)
3. **Pipeline** — tool stack + version pin + 단계별 흐름
4. **Output** — 산출 figure, table, report 종류
5. **Reproducible env** — Docker image / conda env / Snakemake 명령
6. **Demo dataset** — 공개 accession (GEO / ENA / SRA)
7. **Time & resources** — 예상 wall-clock + CPU/RAM 요구
8. **Limitations** — 정직한 한계 명시 (hallucination 방어)
9. **References** — 도구 논문 + best-practice 가이드

템플릿: [_template.md](_template.md)

---

**작성자**: Replisci (shoo99@gmail.com)
**라이선스**: 메소드 카탈로그 자체는 CC BY-SA 4.0, 데모 코드는 MIT
