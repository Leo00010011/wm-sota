---
citation_count: 18
citation_source: "Semantic Scholar, 2026-10-13 (carried over from note; not re-verified, API rate-limited)"
venue: "NeurIPS 2026"
date: 2026-05-20
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# stable-worldmodel: A Platform for Reproducible World Modeling Research and Evaluation

- **Authors:** Lucas Maes, Quentin Le Lidec, Luiz Facury, Nassim Massaudi, Ayush Chaurasia, Francesco Capuano, Richard Gao, Taj Gillin, Dan Haramati, Damien Scieur, **Yann LeCun**, **Randall Balestriero**
- **Group role:** Senior (last) author: Balestriero; LeCun co-author. Affiliations (paper header): Mila & Université de Montréal, NYU, UFMG, LanceDB, University of Oxford, Brown University, independent researcher. Code is hosted by Balestriero's galilai-group.
- **Date published:** 2026-09 (NeurIPS 2026 acceptance; author notification date 2026-09-24; conference in December 2026). arXiv: 2605.21800 (2026-05-20); earlier tiny-paper version "stable-worldmodel-v1" arXiv 2602.08968 (February 2026, ICLR 2026 World Models workshop).
- **Venue:** NeurIPS 2026, Evaluations and Datasets Track, poster ("Accept (poster)", Paris Poster Session 3, per the NeurIPS 2026 accepted-papers data).
- **Links:** arXiv https://arxiv.org/abs/2605.21800 · NeurIPS page https://neurips.cc/virtual/2026/poster/139680 · OpenReview https://openreview.net/forum?id=ApzyaGdg38 · Code https://github.com/galilai-group/stable-worldmodel · Docs https://galilai-group.github.io/stable-worldmodel/
- **Number of citations:** 18 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
stable-worldmodel (swm) is an open-source platform, not a new model: it standardizes data collection, storage and loading (Lance format), provides tested baselines (DINO-WM, PLDM, LeWorldModel, TD-MPC2, GCBC, GCIQL/GCIVL) and planning solvers (CEM, iCEM, MPPI, GD, projected GD, GRASP), and adds environments with controllable visual, geometric and physical "factors of variation" for zero-shot robustness tests. A Push-T case study shows current latent world models are brittle under mild distribution shifts.

## 1. Weights availability
- The platform paper does not release new model weights. Datasets and checkpoints used by swm are stored under `$STABLEWM_HOME`; the README benchmarks use the Push-T dataset https://huggingface.co/datasets/galilai-group/lewm-pusht and reference implementations of LeWM (`scripts/train/lewm.py`) and DINO-WM (`scripts/train/prejepa.py`).
- LeWM checkpoints on Hugging Face (e.g. https://huggingface.co/quentinll/lewm-pusht, `lewm-cube`, `lewm-tworooms`, `lewm-reacher`, uploaded by a co-author) can be used with it; the link between these repos and the paper's Table 1 numbers was not verified.

## 2. Code
- https://github.com/galilai-group/stable-worldmodel, MIT license, last push 2026-10-02, about 2.2k stars (GitHub API, 2026-10-13).
- pip package `stable-worldmodel` (extras `[data]`, `[all]`, `[lerobot]`), a `swm` CLI, training scripts, solvers, environments, benchmarks (`scripts/benchmark/compare_h5_lance.py`) and a Colab for training from HF object storage.

## 3. Datasets
- Environment families: classic control (CartPole), 2D (Push-T, Two-Room), MuJoCo, OGBench 3D manipulation (e.g. Cube), Atari, Craftax (open-world, partial observability); each with factors of variation (color, size, shape, background, lighting, textures, occlusion distractors, mass, friction, gravity).
- Data formats: Lance (default), HDF5, MP4, LeRobot (adapter). Push-T case study uses the DINO-WM expert dataset.

## 4. Evaluation and downstream tasks
- Data loading (Fig. 3, Push-T): Lance 4,815 samples/s local and 3,184 samples/s streaming from S3, vs HDF5 1,416 local / 9 from S3 without caching, MP4 1,331.
- In-distribution planning success (Table 1), Push-T / OGBench-Cube: LeWM 94 / 72, DINO-WM 92 / 86, PLDM 78 / 62, GCBC 75 / 84, TD-MPC2 (offline) 12 / 4. The authors say solver implementations reproduce DINO-WM and PLDM reported rates; TD-MPC2 was checked online on DMC against SAC (Table 5).
- Distribution shift (Fig. 4, 256 trajectories per setting): trajectory-level prediction MSE overlaps for successful and failed plans, so prediction error is a poor predictor of planning success.
- Factors of variation (Fig. 5 table, Push-T success %, LeWM / PLDM / DINO-WM): none 50.8 / 50.8 / 20.0; agent color 12 / 8 / 18; canvas color 6 / 6 / 10; agent size 22 / 18 / 4; agent shape 26 / 52 / 18. Success decays roughly quadratically with the number of visual distractor squares.

## 5. Architecture and techniques
- Three abstractions: World (Gymnasium-compatible vectorized wrapper with data collection and factor-of-variation control), Policy (random, expert, feed-forward, `MPCPolicy` wrapping any world model + solver), Solver (single-shooting MPC planners that call the model's `get_cost`).
- Baselines implemented: DINO-WM (frozen DINOv2 + ViT predictor), PLDM (JEPA with VICReg-style regularization), LeWM (end-to-end JEPA with SIGReg), TD-MPC2 (decoder-free, reward/value), GCBC, GCIQL, GCIVL.
- Built on PyTorch and Gymnasium; integrates with stable-baselines3 and LeRobot.

## 6. Conclusions and future work
- Authors: swm reduces friction across the world-model pipeline; experiments show world models are far from robust zero-shot generalization even under mild shifts, which calls for advances in visual and physical robustness and long-horizon consistency. Future work: sim-to-real transfer, asynchronous real-time interaction, efficient online training, more scientific domains.
- Own observations: the "None" row in the variation table (50.8% for LeWM) is much lower than the 94% in Table 1, presumably because of a different start/goal sampling protocol; the paper should be read carefully before comparing numbers across tables. The case study covers only Push-T. Being a benchmark from the authors of LeWM, there is a risk of home-baseline advantage in tuning.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2605.21800 (Sections 1–6, Table 1, Fig. 3–5).
- Venue: NeurIPS 2026 accepted-papers JSON lists it under the Evaluations and Datasets Track with decision "Accept (poster)".
- Code: GitHub API and README; Hugging Face API for related datasets/models.
- Citations: Semantic Scholar API (arXiv:2605.21800).
- Not verified: the evaluation-protocol difference behind the 94% vs 50.8% LeWM Push-T numbers.
