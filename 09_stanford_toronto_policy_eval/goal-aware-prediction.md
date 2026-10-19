---
citation_count: null
citation_source: null
venue: "ICML 2020"
date: 2020-07-14
research_group: "Stanford / Toronto (policy evaluation)"
---

# Goal-Aware Prediction: Learning to Model What Matters (GAP)

- **Authors:** Suraj Nair, Silvio Savarese, **Chelsea Finn** (Stanford University)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2020-07 (ICML 2020, venue date); arXiv 2020-07-12
- **Venue:** ICML 2020 (PMLR vol. 119)
- **Links:** arXiv https://arxiv.org/abs/2007.07170 · project page https://sites.google.com/stanford.edu/gap · code https://github.com/suraj-nair-1/goal_aware_prediction
- **Number of citations:** 76 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
GAP is a goal-conditioned latent dynamics model: it encodes the current state and goal image together and reconstructs the goal-state residual (s_g - s_t) instead of the full next image, trained with hindsight goal relabelling on self-supervised random data. This shifts model capacity to task-relevant parts of the scene, lowering error on good trajectories and improving latent MPC (CEM) planning.

## 1. Weights availability
Released in the code repository: `pretrained_gap_models/gap_block.pth` and `gap_door.pth` (about 38 MB each), loaded with `python eval_gap.py --models pretrained_gap_models/gap_block.pth ...` (README). No license file.

## 2. Code
https://github.com/suraj-nair-1/goal_aware_prediction, 20 stars, last push 2020-07-15, no license reported by GitHub API (checked 2026-10-15). Contains data collection, training (`train_gap.py`), evaluation with planning (`eval_gap.py`) and the simulated environments (`gap_envs`).

## 3. Datasets
- Simulated Sawyer tabletop (built on Meta-World): 2,000 random episodes of 50 steps (100,000 frames), 64x64 RGB; collected by the authors with released collection code.
- Real-robot data for prediction error: BAIR Robot Pushing dataset and RoboNet (public).

## 4. Evaluation and downstream tasks
- Model error analysis (Fig. 3): GAP has the highest error averaged over 1,000 random action sequences but the lowest error on the 10 best sequences.
- Planning (Fig. 6, 200 trials per task): GAP beats a standard latent dynamics model, an inverse-model latent space and RIG on four tasks (one-block push, two-block push, door close, door open), with 10-20% absolute gains on 3 of 4 tasks.
- Ablations (Fig. 7): goal conditioning and residual prediction both matter except on Task 3.
- Real data (Fig. 9): SVG + GAP has lower prediction error than SVG on goal-reaching trajectories in BAIR and RoboNet.
- Theory: Theorem 3.1 bounds the allowed cost error per action sequence for epsilon-optimal planning; checked on a 2-D navigation task (Fig. 1).
- Downstream task: goal-conditioned visual planning.

## 5. Architecture and techniques
- Encoder f_enc(z | s_t, s_g) and decoder f_dec(s_g - s_t | z) are CNNs; latent dynamics f_dyn(z_{t+1} | z_t, a_t) is fully connected.
- Multi-step prediction with horizon curriculum (H grows by 1 every 50k iterations, about 300k iterations total).
- Planner: latent MPC with CEM (1000 samples, 10 elites, horizon 15, 3 iterations), cost ||z - z_g||^2.
- Real-data variant: GAP objective plugged into action-conditioned SVG.

## 6. Conclusions and future work
- Authors: GAP lowers error in task-relevant states, improves planning and scales to real cluttered scenes. Limits: may fail with dynamic distractors (lighting, moving objects); goal sampling strategy unexplored.
- Own observations: success rates are given only in bar charts; control results are simulation-only.

## 7. Sources and verification
- Paper: arXiv 2007.07170v2 PDF ("Proceedings of the 37th ICML, PMLR 119"), main text read.
- Code and weights: GitHub API and README (checked 2026-10-15).
- Citations: Semantic Scholar API, 2026-10-15.
- Exact success-rate numbers: not stated in text (figures only).
