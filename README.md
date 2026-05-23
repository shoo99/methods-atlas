# Replisci Methods Atlas

> 바이오인포매틱스 오믹스 분석 메소드 카탈로그 — 88개 메소드 1-pager (CC BY-SA 4.0 + MIT)

🌐 **Live site**: <https://shoo99.github.io/methods-atlas/>
🏠 **Replisci**: <https://replisci-landing-v2.vercel.app>

**목적**
- Replisci가 제공 가능한 분석 메소드의 **공식 카탈로그**
- 의뢰자가 "어떤 분석이 가능한가"를 자가 확인할 수 있는 self-service 자료
- 재현성(reproducibility) wedge를 증명하는 포트폴리오

**구조**
- `docs/methods/` — 88개 메소드 1-pager (입력·파이프라인·산출·Docker·데모·한계)
- `docs/categories.md` — 전체 카테고리·메소드 인벤토리 (11 카테고리)
- `docs/index.md` — 사이트 홈 페이지
- `_template.md` — 신규 1-pager 작성 템플릿
- `mkdocs.yml` — MkDocs Material 설정
- `.github/workflows/deploy.yml` — Pages 자동 배포

---

## v1.0 — 88/88 메소드 모두 1-pager 완료 🎉

- **Tier 1** (10) — 임상·wet-lab 80% 커버. RNA-seq DE는 Docker 재현 데모 완료
- **Tier 2** (48) — 자주 의뢰되는 specialized 메소드
- **Tier 3** (30) — long-tail / 고난도 / 신기술

전체 인벤토리: [docs/categories.md](docs/categories.md)

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

## 로컬에서 사이트 빌드

```bash
python3 -m venv .venv
.venv/bin/pip install mkdocs-material
.venv/bin/mkdocs serve  # http://127.0.0.1:8000
```

---

**작성자**: Replisci (shoo99@gmail.com)
**라이선스**: 메소드 카탈로그 자체는 CC BY-SA 4.0, 데모 코드는 MIT ([LICENSE](LICENSE))
**저장소**: <https://github.com/shoo99/methods-atlas>
