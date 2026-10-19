---
citation_count: 31
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; re-check on 2026-10-15 hit HTTP 429)"
venue: "CoRL 2025"
date: 2025-03-15
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Diffusion Dynamics Models with Generative State Estimation for Cloth Manipulation (UniClothDiff)

- **Authors:** Tongxuan Tian, Haoyang Li, Bo Ai, Xiaodi Yuan, Zhiao Huang, **Hao Su** (Tian and Li equal contribution)
- **Group role:** senior/last author (Hao Su; UCSD and Hillbot)
- **Date published:** 2025-09 (CoRL 2025, Seoul, 27–30 Sep 2025; PMLR vol. 305 published 2025-10-07). arXiv v1 2025-03-15; v2 2025-08-29.
- **Venue:** CoRL 2025 (PMLR 305). Presentation type not verified.
- **Links:** arXiv https://arxiv.org/abs/2503.11999 · Proceedings https://proceedings.mlr.press/v305/tian25c.html · Project https://uniclothdiff.github.io · Code https://github.com/Tongxuan259/UniClothDiff
- **Number of citations:** 31 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
UniClothDiff is an action-conditioned world (dynamics) model for cloth, used in a perception-plus-planning system.
- **Diffusion Perception Model (DPM):** a transformer diffusion model that reconstructs the full cloth mesh from partial multi-view RGB-D point clouds.
- **Diffusion Dynamics Model (DDM):** a second transformer diffusion model that predicts the next 5 mesh states from 3 history states and a robot action.
- **Planning:** DDM is used inside MPPI model-predictive control for real-robot cloth folding and unfolding.

Long-horizon prediction error is roughly an order of magnitude lower than GNN dynamics models.

## 1. Weights availability
Not released. The project page and README list no checkpoints. Checked: project page, GitHub README.

## 2. Code
- https://github.com/Tongxuan259/UniClothDiff, MIT license. Last push 2025-11-29.
- Contains template-mesh tokenization, dataset schemas, and training scripts for dynamics and state estimation (uses a customized diffusers fork).
- Not included: the simulator used for data collection ("not yet public", tied to the ManiSkill roadmap) and the MPC/real-robot stack (not described in the README).

## 3. Datasets
- **Training:** about 500K simulated cloth-interaction transitions collected in SAPIEN (cloths, T-shirts, long-sleeve shirts). Not released.
- **Evaluation:**
  - simulation;
  - a real robot with 4 RGB-D cameras, parallel gripper, plus a dexterous-hand transfer demo.

## 4. Evaluation and downstream tasks
Metrics: MSE, Chamfer distance and EMD for state estimation and dynamics; success rate for real-world folding/unfolding (10 trials per scenario, success = EMD below a threshold).
- **State estimation (Table 1):** DPM beats GarmentNets, MEDOR, TRTM and a non-diffusion Transformer. Example, T-shirt in simulation, MSE (×10⁻¹): DPM 2.76 vs TRTM 6.30 vs Transformer 9.12.
- **Dynamics (Fig. 3):** DDM has the lowest MSE at every horizon, with little error accumulation. Baselines: GNN, Transformer (MSE-trained), and an analytical simulator on estimated states. The analytical simulator degrades quickly under perception noise.
- **Real-world planning (Table 2):** success out of 10 trials, by occlusion type:

  | Garment | Occlusion | DDM (ours) | GNN |
  |---|---|---|---|
  | Cloth | self | 9 | 6 |
  | Cloth | external | 8 | 4 |
  | Cloth | combined | 6 | 3 |
  | T-shirt | self | 9 | 1 |
  | T-shirt | external | 7 | 2 |
  | T-shirt | combined | 6 | 2 |
  | Long-sleeve | combined | 4 | 0 |

- **Module ablation (Table 3):** DDM+DPM reaches 9/10 (cloth) and 8/10 (T-shirt); every other dynamics/perception combination does worse.
- **Cross-embodiment (Fig. 6):** transfer from a parallel gripper to a dexterous hand is shown qualitatively.

## 5. Architecture and techniques
- Backbone: ViT-style transformer, DDPM diffusion over mesh vertex positions.
- Tokenization: Voronoi patches of the template mesh (farthest-point-sampling centres), each embedded with PointNet.
- Conditioning: AdaLN plus cross-attention on point-cloud patch embeddings (perception) or on Fourier-embedded delta end-effector actions plus history (dynamics, which adds temporal attention).
- Decoding: distance-weighted interpolation plus an MLP to per-vertex noise.
- Planner: MPPI with a weighted MSE + Chamfer cost and an action-smoothness term, informed action sampling, and probabilistic grasp-point selection.
- Parameter count not stated in the main text.

## 6. Conclusions and future work
- **Authors:**
  - Transformer diffusion models give accurate cloth state estimation and long-horizon dynamics and enable strong real-world cloth manipulation.
  - Limitations: high training cost; only cloth tested (rigid contact-rich tasks are future work); no explicit uncertainty estimates.
- **Own observations:**
  - Each real-world condition has only 10 trials.
  - The dynamics model operates on a known template mesh per garment category, so it does not handle arbitrary unseen garment topologies.
  - Data and simulator are not public, which limits reproduction.
  - Funded by Hillbot, where Su is CTO (disclosed in the paper).

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2503.11999 v2 (main text, Tables 1–3, Figs. 1–6); PMLR v305 listing; project page; GitHub README and API.
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: CoRL presentation type and parameter count.
- **Scope note:** this is a learned, action-conditioned state-prediction (dynamics) model used for planning, which fits the "world models that predict future states" scope. It is mesh-based rather than pixel- or latent-based.
