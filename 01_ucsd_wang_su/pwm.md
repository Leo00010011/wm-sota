---
citation_count: 24
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICLR 2025"
date: 2024-07-02
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# PWM: Policy Learning with Multi-Task World Models

- **Authors:** Ignat Georgiev, Varun Giridhar, Nicklas Hansen, Animesh Garg
- **Group role:** third author (Nicklas Hansen, UCSD, of the Wang/Su group). Neither PI is an author. The senior author is Animesh Garg (Georgia Tech), who is not a PI of any group in the summary. The paper is kept here because it builds directly on the group's TD-MPC2 world models and data, and Hansen is its only author from a summary group.
- **Date published:** 2025-04 (ICLR 2025, Singapore, 24–28 April 2025). arXiv v1 2024-07-02; v3 2025-02-24.
- **Venue:** ICLR 2025, poster ("Accept (Poster)" in the ICLR 2025 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2407.02466 · OpenReview https://openreview.net/forum?id=hOELrZfg0J · Project https://imgeorgiev.com/pwm · Code https://github.com/imgeorgiev/PWM · Models and data https://huggingface.co/imgeorgiev/pwm
- **Number of citations:** 24 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
PWM treats a pretrained, well-regularized TD-MPC2-style world model as a differentiable simulator. For each task it learns a policy with first-order gradients, backpropagating through imagined rollouts with an actor-critic and TD(λ), instead of planning online. The authors show that smoother (SimNorm-regularized) world models produce better optimization landscapes than accurate ones, and even than ground-truth contact dynamics. Policies are extracted in under 10 minutes per task from the 48M multitask world models on the MT30/MT80 benchmarks.

## 1. Weights availability
- Released on Hugging Face at https://huggingface.co/imgeorgiev/pwm:
  - `dflex/pretrained` world models for the single-task dflex experiments;
  - `multitask/` pretrained multitask world models for MT30/MT80.
- Load: `python train_dflex.py env=dflex_ant alg=pwm general.checkpoint=path/to/model`, or `train_multitask.py ... general.checkpoint=...` (README).
- License on the Hugging Face page not checked; the repo is MIT.

## 2. Code
- https://github.com/imgeorgiev/PWM, MIT license (GitHub API).
- Contains world-model pretraining (single-task dflex, and multitask via the TD-MPC2 repo with `horizon=16, rho=0.99`) and per-task policy training and evaluation.
- Last push 2025-08-04.

## 3. Datasets
- **Single-task:** dflex differentiable simulator (Hopper, Ant, Anymal, Humanoid, SNU Humanoid with a 152-D action space). Data released at `huggingface.co/imgeorgiev/pwm/tree/main/dflex/data`.
- **Multitask:** the TD-MPC2 MT30 (DMControl) and MT80 (+50 Meta-World) datasets, public at `huggingface.co/datasets/nicklashansen/tdmpc2`. The paper describes them as 120k / 40k trajectories per DMControl / Meta-World task, from 3 seeds of TD-MPC2.

## 4. Evaluation and downstream tasks
Metrics: PPO-normalized reward (single-task) and normalized score (multitask); 50% IQM with 95% CI (Agarwal et al.); 10 seeds.
- **Single-task, high-dimensional locomotion (Fig. 5):** higher reward than SHAC (which uses ground-truth differentiable dynamics), TD-MPC2, DreamerV3, PPO and SAC. PWM does not scale well to the highest-dimensional task (SNU Humanoid).
- **Multitask MT30/MT80 (Fig. 6):**
  - Higher reward than TD-MPC2 without online planning; the abstract says up to 27% higher.
  - Much lower inference time than TD-MPC2.
  - Multitask PWM roughly matches single-task SAC and DreamerV3 experts on MT30.
  - Per-task policy training takes 9.3 minutes on an RTX 6000.
- **Ablations (Figs. 7–8):**
  - With stiffer contact, SHAC's reward drops 48% while PWM stays about 17% above PPO.
  - Smaller policy batch sizes are better.
  - More regularized world models have higher prediction loss but give better policies (inverse correlation).
  - PWM's policy learning is more sample-efficient than TD-MPC2's but needs a better-trained world model.
- **Toy analyses (Figs. 2–3):** a ball-wall example shows SimNorm models have a smaller optimality gap; a double pendulum shows learned models give higher gradient signal-to-noise ratio (ESNR) than the true dynamics.

## 5. Architecture and techniques
- World model: TD-MPC2 encoder, latent dynamics and reward, conditioned on a task embedding. MLPs with LayerNorm and Mish, SimNorm on the encoder and dynamics outputs. Trained autoregressively with H=16 latent consistency loss (stop-gradient) and cross-entropy discrete reward regression (Eq. 10). The multitask models have 48M parameters.
- Policy: stochastic actor trained by first-order gradients through H-step latent rollouts with value bootstrapping (Eq. 6), plus an ensemble of 3 critics trained with TD(λ) (Eqs. 7–9). No planning at inference.

## 6. Conclusions and future work
- **Authors:**
  - World models for gradient-based policy learning should favour smoothness and a small optimality gap over accuracy.
  - PWM beats methods that use ground-truth dynamics and scales to 80 tasks.
  - Limitations: needs substantial pre-existing data; needs re-training per task; autoregressive TD-MPC2 world models are hard to train at scale.
- **Own observations:**
  - The multitask results reuse TD-MPC2's expert-generated offline data and models, so this is policy extraction on top of an existing world model, not a new world-model architecture.
  - Simulation only.
  - The single-task comparisons use world models pretrained on offline data, a setup that differs from the online baselines.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2407.02466 v3 (main text, Figs. 1–8, Algorithm 1); GitHub README and API.
- ICLR 2025 acceptance confirmed in the ICLR 2025 virtual-site data.
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: exact per-task numbers (reported only as aggregate figures in the main text); Hugging Face license tag.
