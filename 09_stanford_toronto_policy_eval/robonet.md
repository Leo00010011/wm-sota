---
citation_count: 465
citation_source: "Semantic Scholar, 2026-10-15 (carried over from file; live re-check returned HTTP 429)"
venue: "CoRL 2019"
date: 2019-10-24
research_group: "Stanford / Toronto (policy evaluation)"
---

# RoboNet: Large-Scale Multi-Robot Learning

- **Authors:** Sudeep Dasari, Frederik Ebert, Stephen Tian, Suraj Nair, Bernadette Bucher, Karl Schmeckpeper, Siddharth Singh, Sergey Levine, **Chelsea Finn** (UC Berkeley, Stanford University, University of Pennsylvania, CMU)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2019-10/11 (CoRL 2019, Osaka, venue date); arXiv 2019-10-24
- **Venue:** CoRL 2019
- **Links:** arXiv https://arxiv.org/abs/1910.11215 · project page http://www.robonet.wiki/ · code https://github.com/SudeepDasari/RoboNet · visual foresight code https://github.com/SudeepDasari/visual_foresight
- **Number of citations:** 465 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
RoboNet is an open dataset of about 162k robot interaction trajectories (about 15M frames) from 7 robot platforms, with a shared end-effector action space, built to pre-train action-conditioned video prediction models (world models) for visual foresight. The paper shows that pre-training on RoboNet and fine-tuning with a few hundred trajectories transfers to new viewpoints and new robots, and that video prediction models underfit at this scale.

## 1. Weights availability
Not found. The RoboNet README covers dataset download (wiki) and training code; no pretrained video-prediction checkpoints were found in the README (checked 2026-10-15).

## 2. Code
- https://github.com/SudeepDasari/RoboNet, MIT license, 180 stars, last push 2023-03-24: dataset loading, training of inverse models and video prediction models.
- https://github.com/SudeepDasari/visual_foresight, MIT license, 146 stars, last push 2022-11-22: visual foresight (planning/control) code.

## 3. Datasets
- RoboNet: Sawyer 68k, Baxter 18k, WidowX 5k, Franka 7.9k, Kuka 1.8k, Fetch 5k (from prior work), Google robot 56k (from prior work) trajectories (Table 1); 113 camera configurations, 4 institutions. Public under CC BY 4.0, download instructions on the project wiki.
- Evaluation: object relocation tasks with novel objects on Sawyer, Kuka, Franka, Baxter.

## 4. Evaluation and downstream tasks
- Zero-shot viewpoint generalization (Table 2, final distance to goal): multi-view model 9.0 cm (seen view) and 16.2 cm (held-out view) vs single-view 14.8 / 23.2 cm; new background 14.4 cm.
- Few-shot transfer to new robots (Tables 3-5): Kuka 40% success after RoboNet pre-training + 400 trajectories vs 10% from scratch with 400 and 30% with 1800; Franka 40% vs 20% (400) and 35% (8000); Baxter 58% (RoboNet) and 83% (Sawyer-only pre-training) vs 33% from scratch.
- Capacity: 200M vs 500M parameter models give 0.104 vs 0.0847 L1 per-pixel test error, evidence of underfitting.
- Inverse model reaching (Table 6): 4/5, 5/5 (unseen view), 4/5 on Franka.
- Downstream tasks: visual MPC object relocation, inverse-model reaching.

## 5. Architecture and techniques
- World model: deterministic variant of SAVP action-conditioned video prediction (transformation/flow-based, predicts pixel motion), 5M-75M parameters in control experiments.
- Planner: visual foresight, CEM-style MPC with designated-pixel goals.
- Inverse model: one-step action regression from current and goal images.

## 6. Conclusions and future work
- Authors: RoboNet pre-training enables fast adaptation to new robots and views; current video models underfit and work best on similar subsets. Limits: simple pushing and pick-and-place tasks, random collection policy. Future: higher-capacity models, data from better policies, automatic subset selection, broader contributions.
- Own observations: success-rate comparisons use 10-20 trials per condition and different operators per experiment, so cross-experiment comparisons are not meaningful (the authors say so).

## 7. Sources and verification
- Paper: arXiv 1910.11215v2 PDF (comment "accepted at CoRL 2019"), main text read.
- Code: GitHub API and README (checked 2026-10-15).
- Citations: Semantic Scholar API, 2026-10-15.
- Scope note: primarily a dataset paper, included because its central use and experiments are world-model (video prediction) pre-training for planning.
