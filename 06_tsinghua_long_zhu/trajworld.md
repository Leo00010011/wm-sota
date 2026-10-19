---
citation_count: 5
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live re-check returned HTTP 429 on 2026-10-15)"
venue: "ICML 2025"
date: 2025-02-03
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# Trajectory World Models for Heterogeneous Environments

- **Authors:** Shaofeng Yin, Jialong Wu, Siqiao Huang, Xingjian Su, Xu He, Jianye Hao, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); co-authors from Huawei Noah's Ark Lab.
- **Date published:** 2025-07 (ICML 2025, Vancouver; PMLR 267, pp. 72462-72484). arXiv v1 2025-02-03, v2 2025-06-09.
- **Venue:** ICML 2025 (42nd International Conference on Machine Learning), poster.
- **Links:** arXiv https://arxiv.org/abs/2502.01366 · PMLR https://proceedings.mlr.press/v267/yin25f.html · poster https://icml.cc/virtual/2025/poster/45359 · code https://github.com/thuml/TrajWorld · dataset https://huggingface.co/datasets/Joseph-Yin-2004/UniTraj
- **Number of citations:** 5 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
TrajWorld is a transformer world model for low-dimensional proprioceptive state and action trajectories. It works across environments whose state and action spaces differ, because it splits each trajectory into scalar "variates" and alternates attention over time and over variates. It is pre-trained on UniTraj (1.3M trajectories from 80 environments), infers the dynamics of a new environment in context, and transfers to unseen environments for transition prediction, off-policy evaluation and MPC.

## 1. Weights availability
Not released as of the check date. The repo's release checklist leaves "Pre-trained TrajWorld Model" unticked, and the fine-tuning command refers to a local `pt_model/trajworld_pt/trajworld_pt.pkl` without a download link. No Hugging Face model was found.

## 2. Code
- https://github.com/thuml/TrajWorld. No license shown on the page. 9 commits, about 21 stars. README news says training code was released 2025-06-05 and the dataset on Hugging Face 2025-08-18, although the checklist items are still unticked.
- Contains pre-training (`scripts/pretrain/pretrain_trajworld.py`), training and fine-tuning (`scripts/training/train_trajworld.py`), baselines (TDM, MLP Ensemble), transition prediction (`pred/`), OPE (`ope/ope_eval.py`) and MPC (`mpc/mpc.py`).

## 3. Datasets
- **UniTraj** (built for the paper): 1,306,666 episodes and 719M steps from 80 environments, merged from ExORL, RL Unplugged, JAT, DB-1, TD-MPC2 data, plus Modular RL buffers the authors collected (Table 1). State dims range 3-78 and action dims 1-21. HalfCheetah, Hopper and Walker2D are held out. Public on Hugging Face (`Joseph-Yin-2004/UniTraj`, MIT, about 541 GB).
- **Downstream:** D4RL HalfCheetah, Hopper and Walker2D (5 datasets each), DOPE policies for OPE, and Gym Pendulum and Walker2D variants plus DMC Cart-k-Pole for zero-shot tests.

## 4. Evaluation and downstream tasks
- **Zero-shot (Fig. 4):** on held-out Pendulum gravity, MSE is 7.2e-4 with history, 2.1e-3 without history, and 1.7e-2 for last-step mirroring. On Walker2D the corresponding values are 2.9e-6, 1.7e-5 and 1.3e1. It also predicts zero-shot on unseen Cart-2-Pole and Cart-3-Pole.
- **Transition prediction (Fig. 1, Fig. 5):** aggregated MAE over 75 D4RL train-test pairs. Pre-training helps TrajWorld (about -91% error per Fig. 1 labels) and TDM. MLP Ensemble transfers negatively. Pre-trained TrajWorld has lower error on 12 of 15 training datasets.
- **Off-policy evaluation (DOPE, Fig. 6):** best mean absolute error and rank correlation among MLP Ensemble, TDM, FQE, DICE, VPM and the previous best, ETM (energy-based transition models). Slightly worse on Regret@1.
- **MPC with proposal policies (Fig. 7):** best returns, with big gains on Hopper and Walker2D and little on HalfCheetah. TDM shows negative transfer here. With random shooting all models perform poorly, though TrajWorld is relatively better.
- **Scaling (Fig. 9):** 1/100, 1/10 and JAT-only subsets all beat training from scratch but trail full UniTraj.
- Most numbers are given in figures, not tables.

## 5. Architecture and techniques
- Each trajectory is flattened into a T x M matrix of scalars (M = state dim + reward + action dim). Each scalar is discretized into B uniform bins (one-hot or Gaussian histogram).
- Learned timestep, variate and prediction embeddings. Transformer blocks alternate causal temporal attention (per variate) with non-causal variate attention (per timestep).
- Trained with next-step categorical cross-entropy. At inference it samples or takes the expectation over bin centres.
- There is no environment ID; the dynamics are inferred from history. MPC uses proposal policies or random shooting (128 candidate trajectories).
- Parameter count is not stated in the main text. The appendix says pre-training and fine-tuning fit on one 24 GB RTX 4090.

## 6. Conclusions and future work
- **Authors:** this is the first demonstration of positive world-model transfer across heterogeneous, complex control environments. Stated limitations are that discretization bounds predictions to a fixed range, that the larger model is harder to calibrate (an issue for uncertainty-aware offline RL), and the extra compute cost. Future work is multimodal world models that combine vision and proprioception.
- **Own observation:** Downstream tests are limited to three classic D4RL locomotion environments. The world model is not used inside a full offline-RL pipeline. Pre-trained weights are not yet public, so reproduction requires pre-training on the 541 GB UniTraj.

## 7. Sources and verification
- Full arXiv PDF v2 (main text and parts of the appendix), PMLR and ICML poster pages, GitHub README, Hugging Face dataset page, Semantic Scholar API.
- Not verified: the repo license (none visible), exact parameter count (Table 4 hyperparameters not extracted cleanly from the PDF), and exact OPE and MPC numbers (figures only).
