---
citation_count: null
citation_source: null
venue: "ICML 2025"
date: 2025-03-03
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Multi-Stage Manipulation with Demonstration-Augmented Reward, Policy, and World Model Learning (DEMO³)

- **Authors:** Adrià López Escoriza, Nicklas Hansen, Stone Tao, Tongzhou Mu, **Hao Su**
- **Group role:** senior/last author (Hao Su); Hansen is second author
- **Date published:** 2025-07 (ICML 2025, Vancouver, July 2025; PMLR vol. 267). arXiv v1 2025-03-03; v2 2025-06-23.
- **Venue:** ICML 2025, poster ("Accept (poster)" in the ICML 2025 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2503.01837 · OpenReview https://openreview.net/forum?id=Bv7LUUYOiq · ICML https://icml.cc/virtual/2025/poster/46087 · Project https://adrialopezescoriza.github.io/demo3 · Code https://github.com/adrialopezescoriza/demo3
- **Number of citations:** 15 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
DEMO³ extends TD-MPC2's decoder-free latent world model for long-horizon, sparse-reward visual manipulation, using a few demonstrations for three purposes at once:
- **Reward:** per-stage discriminators on the world-model latent are learned online. They turn sparse stage indicators into a dense reward, r + β·tanh(δ_k(z)) with β ≤ 1/3, which the world model's reward head learns and the planner uses.
- **Policy:** behaviour-cloning pretraining, with an annealed handover from the BC policy to MPPI planning.
- **World model:** 50% demonstration oversampling during training.

## 1. Weights availability
- DEMO³ checkpoints: not found. The README shows how to evaluate checkpoints but links no DEMO³ weights.
- Pretrained TD-MPC2 checkpoints for some ManiSkill tasks (used to generate demonstrations) are in a Google Drive folder: https://drive.google.com/drive/folders/1SQukMBzW2D-aeS_7X_7GbGGaEoqEGKeu
- Checked: GitHub README and project page.

## 2. Code
- https://github.com/adrialopezescoriza/demo3. The README states MIT; the GitHub API reports no detected license.
- Contains training and evaluation for DEMO³, MoDem and TD-MPC2, plus 20+ multi-stage tasks; supports state and pixel observations. Built on the TD-MPC2 code.
- Last push 2025-07-29.

## 3. Datasets
16 sparse-reward, multi-stage visual tasks (RGB plus proprioception) in public simulators (Table 1):

| Domain | Tasks | Demos per task | Interaction budget | Stages |
|---|---|---|---|---|
| ManiSkill3 manipulation | 5 | 5–100 | 500k | 3 |
| Meta-World | 5 | 5 | 500k | 2 |
| Robosuite | 4 | 5–25 | 100k | 1 |
| ManiSkill3 humanoids | 2 | 5 | 100k | 3 |

- Demonstrations come from trained TD-MPC2 agents (per the README).
- The paper says all demonstrations are open-sourced, but no explicit download link was found in the README.

## 4. Evaluation and downstream tasks
Metrics: success rate vs interaction steps; steps to reach 30% success; wall-time; 5 seeds.
- **Overall (Figs. 1, 5):** on average 40% better data efficiency than baselines (TD-MPC2, MoDem, LaNE, BC), and 70% better on the hardest tasks.
  - ManiSkill3 manipulation: about 75% success at 500k steps, 50% better than the second-best baseline.
  - Also strongest on ManiSkill humanoids.
- **Hardest tasks (Fig. 6):** DEMO³ is the only method that reliably solves Peg Insertion and Stack Cube within budget. It roughly matches LaNE on Robosuite (LaNE uses a pretrained encoder). TD-MPC2 alone fails with sparse rewards.
- **Wall-time per 100k steps on Robosuite (Table 2):** DEMO³ 5.19 h, TD-MPC2 4.84 h, MoDem 8.37 h, LaNE 20.40 h.
- **Ablations (Figs. 7–9):**
  - BC pretraining and oversampling give most of the gain; the learned reward matters on long-horizon tasks and with few demos.
  - Best in the lowest-demonstration regime.
  - With only 2 stages, performance is near that of hand-designed dense rewards.

## 5. Architecture and techniques
- Backbone: TD-MPC2 (encoder, latent dynamics, reward, Q-ensemble, policy prior; SimNorm; MPPI planning) from RGB plus proprioception.
- Added: N+1 stage discriminators δ_k on the latent z, trained with BCE against labels for the maximum stage reached (Eqs. 2–3). The discriminator loss also trains the encoder (Eq. 4: L_R + L_Q + L_h + L_δ).
- Training: phase 1 is BC pretraining with early stopping; phase 2 is interactive learning with annealing α from π_BC to planning (Algorithm 1) and 50% demo oversampling.
- Reward learning builds on DrS (Mu et al., 2024), also from Su's lab.

## 6. Conclusions and future work
- **Authors:**
  - Jointly learning reward, policy and world model from a few demos gives about 40% better performance and up to 4× faster convergence on the hardest tasks.
  - Future work: robustness to other demonstration sources (e.g. teleoperation), real-robot deployment (experiments are simulation-only), and smarter demo sampling than a fixed 50%.
- **Own observations:**
  - Requires stage indicators from the simulator, which is privileged task structure.
  - Demonstrations come from RL agents, not humans.
  - Gains over MoDem are partly due to the TD-MPC2 backbone.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2503.01837 v2 (main text, Tables 1–2, Figs. 1, 5–9, Algorithm 1); GitHub README and API.
- ICML 2025 acceptance confirmed in the ICML 2025 virtual-site data; the PDF header reads "Proceedings of the 42nd ICML, PMLR 267".
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Not found:
  - The PMLR page URL (not retrieved; OpenReview and the ICML virtual page are linked instead).
  - DEMO³ weights.
  - Exact per-baseline values in the Fig. 1 bar chart (garbled in text extraction).
