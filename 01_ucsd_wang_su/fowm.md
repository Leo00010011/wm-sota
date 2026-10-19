---
citation_count: 50
citation_source: "Semantic Scholar, 2026-10-13 (carried over; 2026-10-15 re-check returned HTTP 429)"
venue: "CoRL 2023"
date: 2023-10-24
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Finetuning Offline World Models in the Real World

- **Authors:** Yunhai Feng, Nicklas Hansen, Ziyan Xiong, Chandramouli Rajagopalan, **Xiaolong Wang** (first three equal contribution)
- **Group role:** senior author (Wang); Hansen is co-first author
- **Date published:** 2023-11 (CoRL 2023, Atlanta, 6–9 Nov 2023; PMLR vol. 229 published 2023-12-02). arXiv 2023-10-24.
- **Venue:** CoRL 2023 (PMLR 229:425–445), Oral (according to the lab and project pages; oral status not checked on the CoRL site)
- **Links:** arXiv https://arxiv.org/abs/2310.16029 · Proceedings https://proceedings.mlr.press/v229/feng23a.html · OpenReview https://openreview.net/forum?id=JkFeyEC6VXV · Project https://yunhaifeng.com/FOWM · Code https://github.com/yunhaif/fowm
- **Number of citations:** 50 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
FOWM pretrains a TD-MPC latent world model offline on real-robot data and then finetunes it online with very few trials (≤20). Two changes make this work. Value learning uses only in-sample actions (IQL-style expectile regression plus AWR for the policy). At test time, the MPPI planner subtracts a penalty proportional to the epistemic uncertainty of a Q-ensemble, so it avoids actions the model cannot predict. Offline and online data are sampled in balanced proportions.

## 1. Weights availability
Not released. The README and project page provide datasets and code but no pretrained checkpoints. Checked: GitHub README, project page.

## 2. Code
- https://github.com/yunhaif/fowm, MIT license. Built on the TD-MPC code.
- Contains offline-to-online training (`src/train_off2on.py`) for the simulated xArm tasks and a dataset download script.
- Last push 2023-10-25 (no later maintenance).
- The real-robot control stack is not described as included.

## 3. Datasets
- **Simulation:**
  - D4RL Hopper (medium, medium-replay) and AntMaze (medium-play, medium-diverse).
  - xArm push/pick (84×84 pixels), each with a medium and a medium-replay set of 40k transitions / 800 trajectories.
  - Quadruped Walk.
- **Real robot:** xArm 7 with 224×224 RGB from a RealSense camera plus proprioception. Offline sets: reach 120 trajectories, pick 200, kitchen 216. Sparse rewards come from hand-designed success detectors.
- The released sim and real datasets are on Google Drive (links in the README); the authors created them.

## 4. Evaluation and downstream tasks
Metrics: success rate (xArm, real) and normalized return (D4RL, quadruped), before and after online finetuning.
- **Simulation (Table 3, 5 seeds), average before → after finetuning:**

  | Method | Before | After |
  |---|---|---|
  | Ours | 46.0 | 84.7 |
  | IQL | 37.7 | 48.7 |
  | TD-MPC offline→online | 11.8 | 55.1 |
  | TD-MPC + data (online only) | – | 48.3 |
  | TD-MPC from scratch | – | 17.4 |

  Examples: AntMaze medium-diverse 75→89; Hopper medium 49.6→100.7.
- **Real robot (Table 1, 18 trials × 2 seeds):** best on reach, pick and kitchen. Reach: 72±6% with zero online trials, vs 50±18% for naive offline-to-online TD-MPC.
- **Transfer to unseen real tasks (Table 2):** pick with unseen distractors goes from 22% to 67% in 20 trials; kitchen with a distractor from 0% to 67%. Transfer fails (0%) for an unseen object colour.
- **Simulation transfer:** 9 xArm transfer tasks (Fig. 4); better than TD-MPC from scratch.
- **Ablations (Fig. 5, Table 4, Fig. 6):**
  - Every component helps.
  - Diverse exploratory data matters more than quantity: Base data gives 0% zero-shot, Diverse gives 33–72%.
  - λ > 0 helps; very large λ hurts online learning.

## 5. Architecture and techniques
- Backbone: TD-MPC (encoder, latent dynamics, reward, Q, policy; deterministic MLPs and a conv encoder for pixels).
- Training: latent consistency, reward and TD losses, with TD targets from a V-function trained by expectile regression (in-sample) and an AWR policy loss.
- Q-ensemble of 5. The planner return subtracts λ·std(Q-ensemble) at every step (Eq. 4).
- Offline and online buffers are sampled 50/50.

## 6. Conclusions and future work
- **Authors:**
  - Uncertainty-regularized planning enables few-shot offline-to-online finetuning of world models on a real robot. The authors say this is the first MBRL offline-to-online work on real robots.
  - Limitations: strong dependence on offline data quality and quantity; sparse rewards only; the best λ varies by task (automating it is future work); transfer needs structural similarity between tasks.
- **Own observations:**
  - The real tasks are simple: three base tasks, a single robot and a fixed camera.
  - Success detectors are hand-designed.
  - The real-world evaluation is small (18 trials × 2 seeds).
  - No checkpoints, which limits reuse.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2310.16029 (main text, Tables 1–4, Figs. 4–6); PMLR v229 listing; GitHub README and API.
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified:
  - Oral status (taken from the authors' pages).
  - Some per-trial values in Table 1, which were garbled in text extraction; only clearly readable numbers are reported.
