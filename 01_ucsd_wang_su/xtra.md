---
citation_count: 19
citation_source: "Semantic Scholar, 2026-10-13"
venue: "ICLR 2023"
date: 2022-10-19
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# On the Feasibility of Cross-Task Transfer with Model-Based Reinforcement Learning

- **Authors:** Yifan Xu, Nicklas Hansen, Zirui Wang, Yung-Chieh Chan, **Hao Su**, Zhuowen Tu (Xu and Hansen equal contribution)
- **Group role:** co-first author (Hansen) and co-author (Su). The last author, Zhuowen Tu (UCSD), is not a PI of any group in the summary, so the paper is kept here.
- **Date published:** 2023-05 (ICLR 2023, Kigali, 1–5 May 2023). arXiv v1 2022-10-19; v2 2023-06-15.
- **Venue:** ICLR 2023, poster ("Accept: poster" in the ICLR 2023 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2210.10763 · OpenReview https://openreview.net/forum?id=KB1sc5pNKFv · Project https://nicklashansen.github.io/xtra · Code https://github.com/mlpc-ucsd/XTRA
- **Number of citations:** 19 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
XTRA asks whether a learned latent world model (EfficientZero/MuZero: representation, latent dynamics, and policy/value/reward heads used with MCTS) can transfer across distinct Atari games.
- **Pretraining:** a multi-game world model is distilled offline from per-game teacher models.
- **Finetuning:** online on the target game while still training on the offline games, with each offline game's weight set by gradient cosine similarity to the target.

It improves EfficientZero's mean and median score on Atari100k by 23% and 25%.

## 1. Weights availability
- Released: pretrained model checkpoints and offline cross-task data in one Google Drive folder, https://drive.google.com/drive/folders/1UcY9nnldmHYUizUi08C-fsM-tHAZRutW (README; about 100 GB in total).
- Usage: unzip into `/data` and `/pretrained_ckpts` in the repo root.
- No separate weights license stated (repo is Apache-2.0).

## 2. Code
- https://github.com/mlpc-ucsd/XTRA, Apache-2.0 license (GitHub API).
- Contains online finetuning scripts for Atari100k with cross-task offline data.
- Last push 2023-04-30.

## 3. Datasets
- **Offline pretraining data (App. F):** trajectories from 12 EfficientZero checkpoints per game (every 10k steps up to 120k), 64 trajectories each. About 1M transitions per game (636k for Assault). Mixed quality, not expert. Released via the Drive link above.
- **Evaluation:** Atari100k (14 games, public ALE).
  - Shooter category: Assault, Carnival, Centipede, DemonAttack, Phoenix.
  - Maze category: Alien, Amidar, BankHeist, MsPacman, WizardOfWor.
  - Diverse targets: Assault, BattleZone, Hero, Krull, Seaquest, pretrained on 8 other games.

## 4. Evaluation and downstream tasks
Metrics: raw game score at 100k steps; mean and median normalized by EfficientZero or by human scores; 5 seeds × 32 episodes.
- **Similar-mechanics transfer (Table 1):** mean improvement over EfficientZero is 1.23× overall (1.36 shooter, 1.11 maze); median 1.25×.
  - Example: DemonAttack 14140.9 vs 11523.0.
  - Without task weights, DemonAttack reaches 51045.9 (+343%), but median performance drops.
  - Zero-shot XTRA beats multi-game BC (normalized mean 0.23 vs 0.05).
- **Diverse transfer (Table 2):** human-normalized mean 1.87 vs 1.29 for EfficientZero; median 0.35 vs 0.33.
- **Ablations (Figs. 6–7):**
  - Task relevance matters: maze→shooter pretraining is about equal to training from scratch.
  - Transferring the representation and dynamics gives most of the gain.
  - A frozen encoder hurts asymptotic performance.
  - The larger encoder (EfficientZero-L) helps XTRA but not EfficientZero.
- **Model-free comparison (App. G):** pretraining helps model-free CURL less consistently than it helps XTRA.

## 5. Architecture and techniques
- Backbone: EfficientZero (MuZero with self-supervised consistency). Components:
  - conv representation network h;
  - latent dynamics g (predicts next latent and reward);
  - prediction head f (policy, value).
- Planning: MCTS in latent space.
- Pretraining: multi-task teacher-student distillation through a modified MuZero Reanalyze.
- Finetuning objective: L = L_target + Σ ηᵢ·L_offline,i, with ηᵢ recomputed periodically from gradient cosine similarity (Eqs. 1–2).
- The default uses a 4× larger representation network (EfficientZero-L); absolute parameter counts are not given in the main text.

## 6. Conclusions and future work
- **Authors:** cross-task transfer of learned world models with model-based RL is feasible when the pretraining tasks are relevant. It is most useful early in training (low-data regime). The paper is framed as a feasibility study meant to encourage further work.
- **Own observations:**
  - Discrete-action Atari only; no continuous control or robotics.
  - Gains vary widely between games, and the diverse-transfer median gain is small.
  - Pretraining data comes from EfficientZero runs on the same benchmark, which may favour related games.
  - Low citation count relative to the group's other work.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2210.10763 v2 (main text, Tables 1–2, Figs. 1, 5–7, App. F); GitHub README and API.
- ICLR 2023 acceptance confirmed in the ICLR 2023 virtual-site data.
- Citation count: Semantic Scholar batch API, 2026-10-13. Semantic Scholar labels the venue "arXiv.org" although the ICLR listing confirms acceptance.
