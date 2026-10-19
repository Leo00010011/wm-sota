---
citation_count: 159
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check on 2026-10-15 returned HTTP 429)"
venue: "CoRL 2021"
date: 2021-07-08
research_group: "Columbia (Yunzhu Li)"
---

# 3D Neural Scene Representations for Visuomotor Control

- **Authors:** **Yunzhu Li**\*, Shuang Li\*, Vincent Sitzmann, Pulkit Agrawal, Antonio Torralba (\* equal contribution)
  - This is the order on the arXiv PDF. The PI's homepage and project page list "Yunzhu Li, Shuang Li, Vincent Sitzmann, Pulkit Agrawal, Antonio Torralba", while the arXiv PDF lists Agrawal before Sitzmann. In both, Torralba is last.
- **Group role:** First author (co-first). The last author is Torralba, who is not a PI in the summary. The MIT agent (05) flagged this paper for this folder. Vincent Sitzmann (05 PI) is a middle author.
- **Date published:** 2021-11 (CoRL 2021, London, 8 to 11 November 2021). arXiv v1 2021-07-08.
- **Venue:** 5th Conference on Robot Learning (CoRL 2021), PMLR. Oral presentation per the arXiv comment and the project page.
- **Links:** arXiv https://arxiv.org/abs/2107.04004 · Project https://3d-representation-learning.github.io/nerf-dy/ · OpenReview listed on the PI's homepage (forum URL not resolved)
- **Number of citations:** 159 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper learns an action-conditioned latent world model on top of a 3D-aware scene representation, sometimes called "NeRF-dy":
- A ResNet-18 encoder maps multi-view images to a unit-norm scene vector.
- A conditional NeRF decoder renders the scene vector from any viewpoint.
- A time-contrastive loss makes the vector viewpoint-invariant.
- An MLP dynamics model predicts the next latent from the current latent and the action.

MPPI planning in this latent space solves fluid and rigid-body manipulation tasks. When the goal image comes from an out-of-distribution viewpoint, auto-decoding (test-time latent optimisation through the renderer) recovers the goal state.

## 1. Weights availability
Not released. The project page lists only the paper, video and BibTeX, with no code or checkpoint link. No GitHub repository was found on the project page, the PI's homepage or the arXiv abstract.

## 2. Code
No code found. I checked:
- The project page https://3d-representation-learning.github.io/nerf-dy/ (no GitHub link).
- The PI's homepage entry, which links [Project] [Paper] [Video] [OpenReview] [BibTex] and no code.
- The arXiv comments.

## 3. Datasets
- **Simulated (NVIDIA FleX, 20 camera views each; Sec. 5):**
  - FluidPour: 1,000 trajectories × 300 steps.
  - FluidShake: 1,000 × 300.
  - RigidStack: 800 × 80.
  - RigidDrop: 1,000 × 50.
- **Real (App. E.2):** four D415 RGB-D cameras recording a human pouring water, with cup pose from an AprilTag. 50 episodes of 15 s at 15 Hz (43,400 frames), split 45 train and 5 test.
- No public dataset release was found.

## 4. Evaluation and downstream tasks
- **Control (FluidPour, FluidShake):**
  - Metrics: Chamfer distance to the goal particles, plus pose errors (cup position and angle for FluidPour; container and cube errors for FluidShake).
  - Goals are tested from seen, interpolated and extrapolated viewpoints.
  - Baselines: TC (time-contrastive only), TC+AE (convolutional decoder, GQN-like), and NeRF (no TC loss and no auto-decoding).
  - Fig. 6: the full model "significantly outperforms all baselines under all testing settings". Values are only in bar charts.
- **PID baseline that matches only the robot state (App. E.3):** fluid Chamfer distance is 0.0482 for the model vs 0.0857 for PID.
- **Cross-view nearest-neighbour retrieval** (App. Table 1, L1 error in time index over 300-frame trajectories):

  | Method | FluidPour | FluidShake |
  |---|---|---|
  | This model | 1.77 | 2.58 |
  | Pixel-space NN | 147.97 | 132.47 |
  | Random | 99.90 | 99.65 |

- **Forward prediction and novel-view synthesis:** shown qualitatively for all four environments (Fig. 7) and on real pouring data (Fig. 11).
- **Downstream tasks:** visuomotor MPC control (fluid pouring and shaking), goal specification from novel viewpoints, open-loop future prediction and novel-view synthesis.

## 5. Architecture and techniques
- **Encoder:** ResNet-18 features (256-D) concatenated with a 16-D flattened camera matrix, passed through an MLP. Features are averaged across views, passed through a second MLP and L2-normalised to give the state s_t.
- **Decoder:** a NeRF-style MLP f_dec(x, d, s_t) → (σ, c) with 8 × 256 ReLU layers (App. Fig. 8), trained with volumetric-rendering L2 loss on two rendered views per iteration.
- **Time-contrastive loss:** a multi-view triplet loss with margin α.
- **Dynamics:** an MLP ŝ_{t+1} = f_dyn(s_t, a_t), trained with an H-step latent L2 rollout loss after the encoder is frozen.
- **Planner:** MPPI with M = 1,000 samples, L = 100 iterations at the first step and 10 afterwards, γ = 50, β = 0.7, horizon T = 80.
- **Auto-decoding:** K = 500 SGD steps on s_goal through the frozen decoder.
- **Training:** Adam with learning rate 5e-4 decayed to 5e-5, batch size 2.
- **Size:** parameter count not stated.

## 6. Conclusions and future work
- **Authors:** viewpoint-invariant, 3D-aware representations learned with a NeRF decoder and time-contrastive learning work well for model-based visuomotor control. Auto-decoding allows goals from out-of-distribution viewpoints.
- **Authors' limitations (App. F):** the model does not generalise to unseen configurations such as more containers. Proposed remedies are more diverse data, or a more structured, compositional representation instead of a single vector.
- **My own observations:**
  - Control is only evaluated in simulation with 20 calibrated training cameras, and the real data is used for prediction only.
  - The single global latent vector limits compositional generalisation.
  - Auto-decoding with 500 steps adds test-time cost.
  - No code was released, which limits reproducibility.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2107.04004 (v2, main text and appendix read).
- Project page: https://3d-representation-learning.github.io/nerf-dy/
- PI's homepage: https://yunzhuli.github.io
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - The PMLR proceedings URL and the OpenReview forum ID (not fetched).
  - Exact values behind Fig. 6.
  - The author-order discrepancy between arXiv and the homepage.
