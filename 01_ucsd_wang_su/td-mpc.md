---
citation_count: 537
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live recheck rate-limited, HTTP 429)"
venue: "ICML 2022"
date: 2022-03-09
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Temporal Difference Learning for Model Predictive Control

- **Authors:** Nicklas Hansen, **Xiaolong Wang**, **Hao Su** (Wang and Su marked as equal contribution on the arXiv PDF; the ICML virtual site and PMLR list the order as Hansen, Su, Wang)
- **Group role:** first author (Hansen, PhD student of the group); both PIs are senior authors
- **Date published:** 2022-07 (ICML 2022, Baltimore, 17–23 July 2022; PMLR vol. 162 published 2022-06-28). arXiv v1 2022-03-09.
- **Venue:** ICML 2022 (PMLR 162). Presentation track (oral/spotlight/poster) not verified.
- **Links:** arXiv https://arxiv.org/abs/2203.04955 · Proceedings https://proceedings.mlr.press/v162/hansen22a.html · Project https://nicklashansen.github.io/td-mpc · Code https://github.com/nicklashansen/tdmpc
- **Number of citations:** 537 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
TD-MPC learns a Task-Oriented Latent Dynamics (TOLD) model: a decoder-free latent world model trained only from reward prediction, TD value learning and a latent consistency loss, never reconstructing observations. At each step, MPPI trajectory optimization runs over short model rollouts (horizon 5) and a learned terminal Q-function bootstraps the return beyond the horizon. The result is sample-efficient continuous control from states or pixels.

## 1. Weights availability
- No pretrained checkpoints released. The README only says weights can be saved during your own training runs (`save_model=True`).
- Per-task results for 23 state-based DMControl tasks are in the repo's `results/` folder.
- Checked: GitHub README, project page.

## 2. Code
- https://github.com/nicklashansen/tdmpc, MIT license (README and GitHub API).
- Includes training and evaluation (`src/train.py`, Hydra configs, task list). Last push 2023-11-25; not archived, but superseded by TD-MPC2.

## 3. Datasets
No fixed dataset. Online RL in simulators (all public):
- DeepMind Control Suite: 15 state-based tasks (6 with sparse reward); 6 Humanoid/Dog tasks; image-based DMControl-100k (6 tasks); 12 image-based "Dreamer benchmark" tasks.
- Meta-World v2: 50 goal-conditioned manipulation tasks, plus MT10 multitask.
- 2 multimodal quadruped locomotion tasks (proprioception plus egocentric camera).
- 92 tasks in total (Section 5).

## 4. Evaluation and downstream tasks
Metrics: episode return (DMControl), success rate (Meta-World), wall-time.
- **State-based DMControl, 15 tasks (Fig. 3):** beats or matches SAC, LOOP, MPC with a ground-truth simulator ("MPC:sim") and ablations. Largest gains on Quadruped and Acrobot.
- **Humanoid/Dog, up to a 38-D action space (Fig. 1):** solves Dog tasks within 1M environment steps. The authors call this the first documented result solving the Dog tasks.
- **Pixels, DMControl-100k (Table 1):** for example Finger Spin 943±59 (DrQ 901, CURL 779), Cup Catch 933±24, Cartpole Swingup 770±70 (EfficientZero 813), Walker Walk 577±208, Cheetah Run 222±88 (DrQ 344).
- **Pixels, 12 Dreamer-benchmark tasks (Fig. 4):** beats CURL and DrQ; competitive with DrQ-v2 and DreamerV2, using one hyperparameter set.
- **Meta-World 50 tasks and MT10 multitask (Fig. 5):** higher success rate than SAC.
- **Multimodal locomotion (Fig. 5):** solves the tasks; a proprioception-only ("blind") agent fails.
- **Compute (Table 2):** solves Walker Walk 16× faster than LOOP, with time-to-solve similar to SAC (0.47 h vs 0.41 h).
- **Planning budget (Fig. 6):** more planning helps on Quadruped Walk. Inference can use about 50% fewer iterations without loss.
- **Transfer, Walk→Run (App. A):** the encoder transfers; the dynamics model is more task-specific.

## 5. Architecture and techniques
- TOLD has five deterministic MLP components: encoder h, latent dynamics d, reward R, Q-function Q, and policy π. Image inputs use a conv encoder with 3 stacked 84×84 frames and ±4 px shift augmentation.
- Loss (Eqs. 7–10) is applied over H-step latent rollouts with temporal weighting λ:
  - reward MSE;
  - TD loss with an EMA target network;
  - latent-state consistency ‖d(z,a) − h_target(s′)‖².
- The policy maximizes Q (DDPG-style) and is used for TD targets and to seed the planner.
- Planner: MPPI with 512 samples (plus 5% from the policy), 6 iterations (8 for Dog, 12 for Humanoid), top-64 elites, horizon 5, warm start, and a constrained σ for exploration.
- Parameter count not given in the main text. The authors note that pixel baselines have up to 15× more parameters.

## 6. Conclusions and future work
- **Authors:** a jointly TD-trained task-oriented latent model plus MPC beats model-free and model-based baselines on diverse continuous control tasks and matches the state of the art in image-based RL. Future work: use the model in other ways (e.g. value expansion), add better exploration (Finger Turn Hard is weaker than SAC/LOOP), and improve the architecture.
- **Own observations:**
  - The model is task-specific because it is trained with rewards: it cannot render or predict observations, so it cannot be inspected visually or reused without rewards.
  - Hyperparameters are tuned per domain in places (planning iterations differ for Dog and Humanoid).
  - Results come from 5 seeds and are simulation-only.

## 7. Sources and verification
- Paper PDF https://arxiv.org/pdf/2203.04955 (main text read: Sections 3–7, Table 1, Table 2, Figs. 1–6); PMLR page; GitHub README and API (license, last push).
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: ICML presentation type (oral vs poster) and total parameter count (not stated in the main text).
