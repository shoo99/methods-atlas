# AlphaFold Protein Structure Prediction

**Category**: Structural
**Tier**: 2
**Status**: 📝 1-pager

## Overview

AlphaFold2 / **AlphaFold 3** / ColabFold / ESMFold 으로 단백질 (또는 complex, RNA, ligand) 의 3차 구조 *in silico* 예측. CASP14 결정적 돌파 이후 단백질 구조 연구 패러다임 전환. 약물 표적 검증, 변이 효과, complex assembly, *de novo* design 모두 적용.

**누가 의뢰**: drug target structural validation, variant impact (pathogenic missense), protein-protein interaction model, antibody design, enzyme engineering.

## Input

- **Sequence (single protein)**: FASTA
- **Complex**: 여러 sequence (homo/hetero-mer)
- **AlphaFold 3**: protein + RNA/DNA + small molecule (ligand SMILES) — multi-modal
- **Multiple Sequence Alignment**: 자동 또는 사용자 제공 (MMseqs2 / HHblits)
- **Templates**: PDB 자동 검색 또는 비활성화

## Pipeline

```
Sequence
  ├─ AlphaFold 2 (local) — full DB (~2.5 TB)
  ├─ AlphaFold 3 (public server / local) — 2024+
  ├─ ColabFold — MSA via MMseqs2 (5× faster)
  ├─ ESMFold — single-seq, very fast, slightly less accurate
  ↓
Predicted structure (PDB) + per-residue pLDDT + PAE matrix
Confidence assessment → ranking
Downstream:
  ├─ Variant impact: SAVER, EVE, AlphaMissense
  ├─ Docking: ligand (DiffDock, AutoDock), protein-protein (HADDOCK, AlphaFold-Multimer)
  ├─ Visualization: PyMOL, ChimeraX
  ├─ Druggability: P2Rank, FPocket
Report
```

| Tool | Version | Purpose |
|------|---------|---------|
| **AlphaFold 3** | 2024 release | gold standard, multi-modal |
| **AlphaFold 2** | 2.3.2 | open-source, well-validated |
| **ColabFold** | 1.5 | accessible, fast MSA |
| **ESMFold** | 1.0 | single-seq, no MSA |
| **OmegaFold** | 2.0 | alternative single-seq |
| **AlphaFold-Multimer** | 2.3 | hetero/homo complex |
| **AlphaMissense** | – | variant pathogenicity |
| **PyMOL / ChimeraX** | 3.0 | visualization |
| **DiffDock** | 1.1 | ligand docking |
| **HADDOCK 3** | 3.0 | flexible docking |

## Output

- `predictions/`:
  - `ranked_0.pdb` — top model
  - `confidence.json` — pLDDT, PAE
  - `msa_depth.txt`
- `figures/`:
  - `structure_pLDDT.png` — colored by confidence
  - `pae_matrix.png` — domain interactions
  - `multimer_interface.png`
  - `variant_impact_3d.png`
- `analysis/`:
  - `pocket_prediction.tsv` (P2Rank)
  - `interface_residues.tsv` (multimer)
- `report/report.pdf`

## Reproducible Environment

```bash
docker pull replisci/alphafold:v1.0.0
docker run --gpus all --rm -v $PWD:/work -v /databases:/databases:ro \
  replisci/alphafold:v1.0.0 \
  python run_af2.py --seq protein.fasta
```

또는 [LocalColabFold](https://github.com/YoshitakaMo/localcolabfold) (DB 작음).

## Demo Dataset

- 익숙한 단백질: human p53 (UniProt P04637)
- Complex demo: ACE2 + SARS-CoV-2 Spike RBD
- Multimer benchmark: CASP15 targets

## Time & Resources

| Stage | Wall-clock | GPU | RAM | Disk |
|-------|-----------|-----|-----|------|
| 200 aa single, AF2 | ~30 min | A100 | 32 GB | (DB 2.5 TB) |
| 500 aa single, AF2 | ~2 h | A100 | 64 GB | – |
| Multimer (2 × 300 aa) | ~4 h | A100 | 64 GB | – |
| ColabFold (MMseqs2) | ~10 min | A100 | 16 GB | (no large DB) |
| ESMFold | ~5 min | A100 | 16 GB | – |

GPU 필수 (V100/A100/H100). DB가 큰 경우 CPU 시간이 MSA에 더 많이 듦.

## Limitations

- ❌ **Disordered / flexible 영역 부정확** — pLDDT < 70 영역 신뢰도 낮음
- ❌ **Conformational dynamics 미반영** — 단일 conformation 예측. 실제 dynamic은 MD 필요
- ⚠ **Multimer stoichiometry** — 정확한 stoichiometry 사전 지식 필요
- ⚠ **Membrane proteins** — TM 영역 정확도 변동 (학습 데이터 제한)
- ⚠ **Ligand binding** — AF3는 일부 ligand 지원, 정확도 변동. 명확한 docking은 별도 tool
- ⚠ **Mutation effect 작음** — single point mutation 효과는 미세, AlphaMissense 등 specialized 모델 권장
- ⚠ **Experimental validation 권장** — AF는 가설. cryo-EM/X-ray/NMR 검증 단계

## Quality Checks

- [x] Mean pLDDT > 70 (good), > 90 (excellent)
- [x] PAE matrix domain block structure
- [x] MSA depth > 30 sequences
- [x] (Multimer) interface PAE < 5 Å
- [x] Steric clash check (e.g., MolProbity)
- [x] Known structure (if available) RMSD < 2 Å

## References

- Jumper J, et al. Highly accurate protein structure prediction with AlphaFold. *Nature* 2021.
- Abramson J, et al. Accurate structure prediction of biomolecular interactions with AlphaFold 3. *Nature* 2024.
- Mirdita M, et al. ColabFold: making protein folding accessible to all. *Nat Methods* 2022.
- Lin Z, et al. Evolutionary-scale prediction of atomic-level protein structure (ESMFold). *Science* 2023.
- Cheng J, et al. Accurate proteome-wide missense variant effect prediction with AlphaMissense. *Science* 2023.

---

**Lead**: Replisci
**Last updated**: 2026-05-19
