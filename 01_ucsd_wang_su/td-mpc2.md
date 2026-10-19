---
citation_count: 706
citation_source: "Semantic Scholar, 2026-10-13"
venue: "ICLR 2024"
date: 2023-10-25
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# TD-MPC2: Scalable, Robust World Models for Continuous Control

- **Authors:** Nicklas Hansen, **Hao Su**, **Xiaolong Wang** (Su and Wang marked "equal advising")
- **Group role:** first author (Hansen) plus both PIs as senior authors
- **Date published:** 2024-05 (ICLR 2024, Vienna, 7–11 May 2024). arXiv v1 2023-10-25.
- **Venue:** ICLR 2024, Spotlight (listed as "Accept (spotlight)" in the ICLR 2024 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2310.16828 · OpenReview https://openreview.net/forum?id=Oxh5CstDJU · ICLR https://iclr.cc/virtual/2024/poster/18722 · Project https://www.tdmpc2.com · Code https://github.com/nicklashansen/tdmpc2 · Models https://huggingface.co/nicklashansen/tdmpc2 · Data https://huggingface.co/datasets/nicklashansen/tdmpc2
- **Number of citations:** 706 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
TD-MPC2 is a set of robustness and scaling improvements to TD-MPC's implicit (decoder-free) latent world model. Planning uses MPPI in latent space, with a learned terminal value and a policy prior. One hyperparameter set works across 104 online-RL tasks in 4 domains. A single 317M-parameter multitask world model is trained offline on 80 tasks spanning several embodiments and action spaces.

## 1. Weights availability
- Released: 324 checkpoints on Hugging Face at https://huggingface.co/nicklashansen/tdmpc2 (model-card license tag: MIT).
  - 312 single-task checkpoints (104 tasks × 3 seeds, 5M params).
  - 12 multitask checkpoints (mt80/mt70/mt30; 1M, 5M, 19M, 48M and 317M params).
- Load with the repo, e.g. `python evaluate.py task=mt80 model_size=48 checkpoint=/path/to/mt80-48M.pt`. Single-task checkpoints need `model_size=5`.
- Hardware: evaluating up to the 317M model needs a GPU with at least 8 GB (README).

## 2. Code
- https://github.com/nicklashansen/tdmpc2, MIT license. Actively maintained: last push 2026-07-13, about 980 stars.
- Contains training and evaluation for single-task online RL and multitask offline RL, from state or pixels.
- Since April 2025 it also has basic support for Gymnasium MuJoCo/Box2D tasks.

## 3. Datasets
- **Online RL (public simulators):** DMControl (39 tasks, including 11 new ones; plus 10 pixel tasks), Meta-World (50), ManiSkill2 (5, including Pick YCB with 74 objects), MyoSuite (10).
- **Multitask offline datasets, released:**
  - mt80: 545M transitions, 34 GB, 80 tasks (50 Meta-World + 30 DMControl), taken from the replay buffers of 240 single-task agents, so behaviour ranges from random to expert.
  - mt30: 345M transitions, 20 GB.
  - Hosted at https://huggingface.co/datasets/nicklashansen/tdmpc2. No license is stated on the dataset page.

## 4. Evaluation and downstream tasks
Metrics: episode return (DMControl), success rate (Meta-World, ManiSkill2, MyoSuite), and a normalized score (mean of success rates and returns scaled to [0, 100]).
- **Single-task online RL, 104 tasks (Fig. 4):** beats SAC, DreamerV3 and TD-MPC in all 4 domains with one set of hyperparameters. TD-MPC sometimes diverges; DreamerV3 is unstable on Dog and weak on fine manipulation (Figs. 5–6).
- **Visual RL, 10 pixel DMControl tasks (Fig. 10):** comparable to DrQ-v2 and DreamerV3.
- **Multitask scaling, mt80 (Table 1, Fig. 7):** normalized score by model size:

  | Params | Normalized score | GPU-days (RTX 3090) |
  |---|---|---|
  | 1M | 16.0 | 3.7 |
  | 5M | 49.5 | 4.2 |
  | 19M | 57.1 | 5.3 |
  | 48M | 68.0 | 12 |
  | 317M | 70.6 | 33 |

  The score rises roughly linearly in log(params) and does not appear saturated.
- **Few-shot finetuning (Fig. 8):** a 19M model pretrained on 70 tasks and finetuned on 10 held-out tasks scores 47.0 vs 24.0 from scratch at 20k steps (about 2×).
- **Ablations (Fig. 9):** the SimNorm normalization, discrete regression, Q-ensemble and maximum-entropy policy prior each matter. For example, on the 80-task model the score drops from 54.2 to 46.8 with continuous regression.

## 5. Architecture and techniques
- Components (all MLPs with LayerNorm and Mish activations): encoder h, latent dynamics d, reward R, ensemble of 5 Q-functions, and a stochastic max-entropy policy prior p. All are conditioned on a learnable task embedding (ℓ2-norm ≤ 1).
- SimNorm: the latent is projected onto simplices with a grouped softmax.
- Losses: joint-embedding prediction to a stop-gradient target (BYOL-style), plus cross-entropy discrete regression of rewards and values in a log-transformed space. The TD target uses the minimum of 2 subsampled EMA Q-functions.
- Planner: MPPI with a policy-prior warm start.
- Multi-embodiment support: inputs and outputs are zero-padded to the largest dimension and invalid action dimensions are masked.
- Default size is 5M params; the scaled family spans 1M–317M. Batch size is 256 single-task and 1024 multitask; update-to-data ratio is 1.

## 6. Conclusions and future work
- **Authors:**
  - Implicit, control-centric world models scale with model and data size and can be robust across tasks without retuning.
  - Open problems: discrete action spaces, zero-shot use and fast finetuning to new embodiments, combining with VLMs, and reward-free or generalized-reward pretraining.
  - Risks: reward misspecification, unsafe autonomy, and data costs that favour well-resourced groups.
- **Own observations:**
  - The multitask results are offline RL on data from expert TD-MPC2 agents, not online multitask learning (Newt addresses this later).
  - Tasks are identified by a learned embedding (task ID), so new tasks cannot be specified zero-shot.
  - Simulation only, with 3 seeds.

## 7. Sources and verification
- Paper PDF https://arxiv.org/pdf/2310.16828 (main text, Table 1, Figs. 1–10); tdmpc2.com /models and /dataset pages; Hugging Face model card; GitHub README and API.
- ICLR 2024 acceptance (spotlight) confirmed in the ICLR 2024 virtual-site paper data (iclr.cc static JSON).
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: dataset license (not stated on the dataset page).
