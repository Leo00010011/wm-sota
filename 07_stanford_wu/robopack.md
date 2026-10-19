---
citation_count: 48
citation_source: "Semantic Scholar, 2026-10-15"
venue: "RSS 2024"
date: 2024-07-01
research_group: "Stanford (Jiajun Wu)"
---

# RoboPack: Learning Tactile-Informed Dynamics Models for Dense Packing

- **Authors:** Bo Ai*, Stephen Tian*, Haochen Shi, Yixuan Wang, Cheston Tan, Yunzhu Li, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author. Yunzhu Li (PI of summary group 8, then at UIUC) is second-to-last author.
- **Date published:** 2024-07 (RSS 2024, Delft, July 2024; arXiv 2024-07-01)
- **Venue:** Robotics: Science and Systems (RSS) 2024, paper p130
- **Links:**
  - arXiv: https://arxiv.org/abs/2407.01418
  - Proceedings: https://www.roboticsproceedings.org/rss20/p130.pdf (URL taken from the repo README citation)
  - Project page: https://robo-pack.github.io/
  - Code: https://github.com/BoAi01/robopack
- **Number of citations:** 46 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
RoboPack learns an action-conditioned, particle-based world model from real visuo-tactile robot data, and a robot uses it for planning (MPC).

- **Representation.** The scene is a set of tracked 3D keypoint particles. Particles on the Soft-Bubble tactile gripper carry tactile embeddings.
- **State estimator.** A recurrent GNN state estimator (GNN + LSTM) infers particle positions and per-object latent "physics vectors" from the history of touch. It needs only the first visual frame.
- **Dynamics model.** A GNN dynamics model predicts future particle states from the action, conditioned on those latents.
- **Planning.** MPPI uses the model to plan non-prehensile pushing and dense packing.

## 1. Weights availability
- **Checkpoints in the repo:** the `trained_models/` directory (`dec_18`, `dec7_version32`) at https://github.com/BoAi01/robopack/tree/main/trained_models.
- **Tactile autoencoder:** the README shows how to test a pretrained one (`pretrained_ae/...ckpt`).
- **Not confirmed:** which checkpoints correspond to the paper's final models.
- **License:** none declared (no LICENSE file).

## 2. Code
- **Repository:** https://github.com/BoAi01/robopack. Last commit 2025-03-21. No LICENSE file (raw `LICENSE` returns 404).
- **Contents:**
  - Perception: calibration and 3D point tracking.
  - Tactile autoencoder training.
  - Dynamics/estimator training.
  - MPC planning scripts for pushing and packing.
  - Teleoperation data collection.
- **Sample data:** a small sample dataset (Google Drive link in the README).
- **Builds on:** the RoboCraft and RoboCook codebases.

## 3. Datasets
- **Own real-world teleoperated "random interaction" data:**
  - Box pushing: about 15 min per weight configuration, about 12,000 frames in total.
  - Dense packing: about 20 min with 5 objects, about 6,000 frames.
- **Size and availability:** about 30 min of data per task on average. Only a small sample is public (Google Drive). The full dataset was not found.
- **Hardware:** Franka Panda, 4× RealSense D415 cameras, 2 Soft-Bubble sensors (each with a D405 camera inside).

## 4. Evaluation and downstream tasks
- **Long-horizon dynamics prediction (Table I).**
  - Box pushing: RoboPack is best on MSE, EMD and CD. MSE×1e-3 appears to be 1.48 for RoboPack, against 1.75 (no tactile), 2.11 (RoboCook + tactile) and 2.65 (physics sim). This column is a reconstruction from the extracted PDF text, so treat it as approximate.
  - Dense packing: MSE×1e-3 0.070 ± 0.005 against 0.088 ± 0.006 (no tactile). EMD and CD differences are not statistically significant.
- **Latent physics analysis (Fig. 7).** A linear classifier on the latents identifies box type with accuracy rising from 0.26 (t = 0) to 0.93 (t = 144). The latents generalize to histories 4× longer than in training.
- **Real-world non-prehensile box pushing (Table II, 4 boxes × 5 trials).**

  | Method | Success | Final MSE | # pushes |
  |---|---|---|---|
  | RoboPack | 16/20 | 0.0156 | 5.3 |
  | Physics-based simulator + system ID | 10/20 | 0.0230 | 7.3 |
  | RoboPack (no tactile) | 8/20 | 0.0317 | 7.85 |
  | RoboCook + tactile | 6/20 | 0.0480 | 8.4 |

  RoboPack also reaches 4/5 on the unseen box 4.
- **Real-world dense packing (Table III).**
  - Seen objects: 12/15, against 10/15 without tactile.
  - Unseen objects: 6/15, against 5/15 without tactile.
- **Downstream tasks:** model-based planning (MPC) for box pushing and dense packing.

## 5. Architecture and techniques
- **Perception.**
  - 3D keypoint tracking extended from D3Fields: DINOv2 features, Grounded-SAM masks, a distance-to-surface term, motion regularization and a mask-consistency term, with per-object rigid transforms plus shear for deformables.
  - Tactile: a point-wise autoencoder over shear and force signals.
- **State estimator g.**
  - GNN message passing over a radius graph, with node and edge encoder MLPs.
  - An LSTM per object updates a 16-dimensional latent physics vector ξ (initialized from N(0, 0.1²)).
  - It runs autoregressively over T history steps.
- **Dynamics predictor f.**
  - Same GNN, without tactile input, conditioned on the frozen ξ.
  - Predicts next particle positions under robot actions.
- **Training.** End-to-end. The MSE loss on tracked particles is enabled by point correspondences, instead of EMD or Chamfer. The latent ξ receives no explicit supervision.
- **Planning.** MPPI over macro-actions (parameterized pushes). The cost is MSE to the goal point cloud for pushing, or a custom packing cost (Eq. 10). The first K steps are executed, then the system re-estimates and re-plans.

## 6. Conclusions and future work
- **Authors' conclusions.** Tactile histories let the model adapt online to unknown physical properties and improve both prediction and planning. The perception, estimation and dynamics modules are task-agnostic; the cost and planner are task-specific.
- **Authors' future work.**
  - Handle higher-fidelity particles to capture fine deformation.
  - Use alternative (gradient-based) trajectory optimization with the differentiable model.
  - Add physics priors.
- **Own observations.**
  - The evaluation is small: 20 pushing trials and 15 packing trials per setting.
  - Action spaces are hand-engineered for each task.
  - The model relies on multi-camera tracking at training time.
  - Data is small and robot-specific, so generalization beyond the two tasks is not shown.

## 7. Sources and verification
- **Sources read:**
  - arXiv PDF https://arxiv.org/pdf/2407.01418: Secs. I–VI, Tables I–III, Figs. 7–9.
  - Repo README and file listing: https://github.com/BoAi01/robopack
  - Project page: https://robo-pack.github.io/
  - Semantic Scholar API (`paper/arXiv:2407.01418`): venue RSS.
- **Not verified:**
  - Exact Table I column mapping for box pushing (PDF layout extraction mixed the columns).
  - Exact RSS session dates.
