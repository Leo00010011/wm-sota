---
citation_count: 161
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check rate-limited, HTTP 429)"
venue: "ICLR 2020"
date: 2019-09-12
research_group: "Stanford / Toronto (policy evaluation)"
---

# Hierarchical Foresight: Self-Supervised Learning of Long-Horizon Tasks via Visual Subgoal Generation (HVF)

- **Authors:** Suraj Nair, **Chelsea Finn** (Stanford University; Google Brain)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2020-04 (ICLR 2020, venue date); arXiv 2019-09-12
- **Venue:** ICLR 2020, poster
- **Links:** arXiv https://arxiv.org/abs/1909.05829 · ICLR https://iclr.cc/virtual/2020/poster/1569 · mlanthology https://mlanthology.org/iclr/2020/nair2020iclr-hierarchical · project page https://sites.google.com/stanford.edu/hvf · code https://github.com/google-research/google-research/tree/master/hierarchical_foresight
- **Number of citations:** 161 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
HVF builds on visual foresight (action-conditioned video prediction plus CEM planning). A (conditional) VAE over images generates candidate subgoal images, and an outer CEM in the VAE latent space picks subgoals that minimize the worst-case planning cost of each segment under the video prediction model. The robot then runs visual MPC from subgoal to subgoal, which helps long-horizon, sparse-cost tasks.

## 1. Weights availability
Not released. No checkpoints found in the google-research directory listing or project page (checked 2026-10-15).

## 2. Code
https://github.com/google-research/google-research/tree/master/hierarchical_foresight (Apache-2.0 for the monorepo). Files: `generate_data.py`, `train_vae.py`, `train_tdm.py`, `train_tap.py`, `meta_cem.py`, `models`, `env`. Training of the SV2P video model is via tensor2tensor per README. Contains data generation, training and planning code.

## 3. Datasets
- Simulated maze navigation and simulated desk manipulation (Franka, MuJoCo, blocks/door/drawer), random-policy data generated with released code.
- BAIR robot pushing dataset (public) for qualitative real-image subgoals.

## 4. Evaluation and downstream tasks
- Maze (Fig. 2, 100 trials): HVF improves over visual foresight without subgoals and over TAP subgoals, especially on "Hard"; oracle bottleneck subgoals still best.
- Desk manipulation (Fig. 4, 100 trials): at least 20% absolute gain over visual foresight, TAP and RIG; Door + 2 Block: about 15% without or with 1 subgoal vs over 42% with 2 subgoals.
- Ablations (Tables 1-3, maze Hard): success by number of subgoals 0/1/2/3/5/10 = 33/47/54/39/2/0%; max vs mean cost 0.54 vs 0.53 (2 subgoals); with 1000 MPC samples 0.35/0.54/0.55 for 0/1/2 subgoals.
- Downstream task: long-horizon goal-image planning.

## 5. Architecture and techniques
- Dynamics model: SV2P stochastic action-conditioned video prediction (Babaeizadeh et al.).
- Generative subgoal model: VAE or CVAE (conditioned on current image), latent size 8.
- Planning: nested CEM (outer over subgoal latents, M = 200, 40 elites; inner visual MPC), objective min-max segment cost (Eq. 1), pixel L2 cost to goal.

## 6. Conclusions and future work
- Authors: HVF finds semantically meaningful subgoals and improves long-horizon success. Limits: heavy computation from nested MPC; video prediction and cost functions are the main bottleneck (oracle subgoals not perfect); better generative models needed. Future: amortize subgoal generation as a learned policy.
- Own observations: real-robot evaluation is only qualitative (BAIR dataset subgoals), not closed-loop control.

## 7. Sources and verification
- Paper: arXiv 1909.05829v1 PDF, main text read.
- Venue: ICLR 2020 virtual poster page and mlanthology (search results).
- Code: GitHub contents API for google-research/hierarchical_foresight (checked 2026-10-15).
- Citations: Semantic Scholar API, 2026-10-15.
