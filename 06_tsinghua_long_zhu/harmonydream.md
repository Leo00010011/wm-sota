---
citation_count: 28
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429 on 2026-10-15)"
venue: "ICML 2024"
date: 2023-09-30
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# HarmonyDream: Task Harmonization Inside World Models

- **Authors:** Haoyu Ma, Jialong Wu, Ningya Feng, Chenjun Xiao, Dong Li, Jianye Hao, Jianmin Wang, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); co-authors from Huawei Noah's Ark Lab and Tianjin University.
- **Date published:** 2024-07 (ICML 2024, Vienna; PMLR volume 235). arXiv v1 2023-09-30, v3 2024-06-05.
- **Venue:** ICML 2024 (41st International Conference on Machine Learning), PMLR 235. Track type not verified.
- **Links:** arXiv https://arxiv.org/abs/2310.00344 · code https://github.com/thuml/HarmonyDream
- **Number of citations:** 28 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
HarmonyDream treats world-model learning in model-based RL as multi-task learning with two tasks, observation modelling and reward modelling. The paper shows that in Dreamer the observation loss dominates and causes spurious correlations, while reward-only (implicit) models lack learning signal. It adds lightweight learnable "harmonizers" that rescale the observation, reward and dynamics losses automatically so that neither task dominates.

## 1. Weights availability
Not released. The README and file listing (`assets`, `dreamerv3-jax`, `wmlib-torch`) mention no checkpoints.

## 2. Code
- https://github.com/thuml/HarmonyDream, MIT license, 5 commits, about 57 stars. Date of the last commit not visible.
- `wmlib-torch` holds the PyTorch HarmonyDream, DreamerV2 and DreamerV3 code (an add-on to the ContextWM codebase). `dreamerv3-jax` holds the JAX version used for Atari 100K.
- There are training configs for Meta-World, RLBench, DMC Remastered, Natural-background DMC and MineDojo. Evaluation runs through the training configs.

## 3. Datasets
All online RL in public simulators, with no fixed dataset: Meta-World (6 tasks), RLBench (Push Button, Reach Target), DMC Remastered (3 tasks), Natural-background DMC, Atari 100K (26 games) and MineDojo Hunt Cow. The analysis in Sec. 2.3 uses one offline replay buffer from Meta-World Lever Pull.

## 4. Evaluation and downstream tasks
- Metrics: success rate, episode return and Atari human-normalized score, with mean and 95% CI over 5 runs.
- **Meta-World (Fig. 6a):** above 75% success on Push and above 90% on Assembly, where DreamerV2 fails.
- **RLBench (Fig. 6b):** +28% absolute final success on Push Button and +50% on Reach Target.
- **DMC Remastered (Fig. 7):** large gains over DreamerV2; the dynamics loss converges where DreamerV2's diverges.
- **Generality (Figs. 8-9):** improves DreamerV3 (Meta-World, RLBench, Minecraft Hunt Cow) and DreamerPro, beating DreamerPro's hand-tuned w_r = 1000.
- **Atari 100K:** Harmony DreamerV3 reaches 136.5% mean human-normalized score, which the authors call state of the art among methods without lookahead search (Sec. 4.4).
- **Comparisons (Figs. 10-11):** beats TD-MPC and RePo (implicit or task-centric MBRL), Denoised MDP, and multi-task weighting methods (uncertainty weighting, DWA, NashMTL).
- Headline claim: 10-69% absolute gains on visual robotic tasks.

## 5. Architecture and techniques
- Base world model: DreamerV2 RSSM (also DreamerV3 and DreamerPro), with CNN encoder and decoder and actor-critic learning in imagination.
- Harmonious loss: L = sum_i [ L_i / sigma_i + log(1 + sigma_i) ] over i in {observation, reward, dynamics}, with sigma_i = exp(s_i) learnable. This "rectified" form avoids huge weights on small losses (Eq. 5, Prop. 3.1).
- No architecture change and no extra hyperparameters.

## 6. Conclusions and future work
- **Authors:** domination by either task hurts MBRL sample efficiency, and dynamic balancing fixes it. Gains are marginal on visually clean benchmarks (standard DMC, Crafter), with no degradation. Future work is to explain the method theoretically and to find better ways to measure and balance task contributions than loss scale.
- **Own observation:** The method is a loss-weighting heuristic for reconstruction-based world models, so it is not clearly relevant to newer video or diffusion world models. No weights are released. Most results are curves rather than tables.

## 7. Sources and verification
- Full arXiv PDF v3 (main text), arXiv abstract page, GitHub README, Semantic Scholar API.
- Venue confirmed by the PDF footer ("Proceedings of the 41st ICML, Vienna, PMLR 235, 2024") and the arXiv comment "ICML 2024". The PMLR page URL and presentation type (poster or spotlight) were not found by search.
