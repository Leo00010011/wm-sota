---
citation_count: null
citation_source: "null; Semantic Scholar API returned HTTP 429 on 2026-10-15, unverified (earlier note said 26)"
venue: "CoRL 2021"
date: 2021-09-21
research_group: "Stanford / Toronto (policy evaluation)"
---

# Example-Driven Model-Based Reinforcement Learning for Solving Long-Horizon Visuomotor Tasks (EMBER)

- **Authors:** Bohan Wu, Suraj Nair, Li Fei-Fei†, **Chelsea Finn**† (†equal advising; Stanford University)
- **Group role:** Senior (last, co-advising) author (Finn); Fei-Fei Li is co-advisor but not a PI of any summary group
- **Date published:** 2021-11 (CoRL 2021, London, venue date); arXiv 2021-09-21
- **Venue:** CoRL 2021
- **Links:** arXiv https://arxiv.org/abs/2109.10312 · project page https://sites.google.com/view/embr-site
- **Number of citations:** 26 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
EMBER learns a task-agnostic latent world model (GHVAE encoder plus latent dynamics) from all robot data, and per-skill success classifiers (from 100-200 example images) and Q-functions. Actions are chosen by MPC: roll the latent dynamics for L steps and score the end state with the Q-function. The learned skills and their classifier-based pre/post-conditions are sequenced by a PDDL planner to solve long-horizon real-robot tasks.

## 1. Weights availability
Not released. The project page contains only videos (checked 2026-10-15); no checkpoints found.

## 2. Code
No code found. Checked project page and GitHub search.

## 3. Datasets
- Real-robot data collected by EMBER itself on a Franka Panda with five 64x64 RGB cameras (one wrist): 300-400 trajectories per skill (about 16 h per skill), 14 skills; 100-200 positive example images per skill (345 min total to collect). Not released.
- Train/validation/test objects are disjoint; test objects are novel.

## 4. Evaluation and downstream tasks
- Primitive skills (Table 1, 20 trials each): EMBER >= 90% on all 14 skills (e.g. insert marker 100%, grasp from drawer in clutter 90%); BEE (classifier-based MPC) 25-40% on high-precision grasps; Qt-Opt-style variant 70-80%.
- Long-horizon tasks (Table 2): Organize Desk and Cabinet, Organize Markers, Rearrange Objects (up to 12 skills): EMBER 85% each; EMBER w/o dynamics 65-75%; Qt-Opt variant 55-65%; BEE 0%; EMBER w/o classifier replanning 50-55%.
- Ablation: stage-specific rewards beat skill-level rewards by 10-30% on high-precision skills.
- Downstream task: model-based RL and symbolic task planning on a real robot.

## 5. Architecture and techniques
- Encoder/decoder: greedy hierarchical VAE (GHVAE) with current image as context.
- Latent dynamics model f_T trained jointly with the VAE by multi-step L2 latent prediction (Eq. 2).
- Rewards: binary image classifiers in latent space (Eq. 3); Q-learning in latent space with sampled-max targets (Eq. 4).
- Planner: sampling-based MPC over latent rollouts with Q-value terminal cost (Eq. 5); high level: PDDL STRIPS planner.

## 6. Conclusions and future work
- Authors: EMBER reaches 85% on three long-horizon tasks with novel objects. Limits: narrow task scope, considerable supervision (example images, stage definitions), limited generalization across scenes, object-specific skill definitions.
- Own observations: the evaluation uses one physical scene (desk with drawers and cabinet); baselines are trained on EMBER's own collected data, which the authors acknowledge favors them in exploration but not in data distribution.

## 7. Sources and verification
- Paper: arXiv 2109.10312v2 PDF (footer "5th Conference on Robot Learning (CoRL 2021)"), main text read.
- Venue: PDF footer, Semantic Scholar venue, Finn CV.
- Citations: Semantic Scholar API, 2026-10-15.
- Method name: the arXiv title uses "Example-Driven Model-Based RL"; the paper calls the method EMBER.
