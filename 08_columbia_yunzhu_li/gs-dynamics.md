---
citation_count: 60
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live recheck rate-limited on 2026-10-15)"
venue: "CoRL 2024"
date: 2024-10-24
research_group: "Columbia (Yunzhu Li)"
---

# Dynamic 3D Gaussian Tracking for Graph-Based Neural Dynamics Modeling (GS-Dynamics)

- **Authors:** Mingtong Zhang\*, Kaifeng Zhang\*, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author. The affiliations are UIUC and Columbia.
- **Date published:** 2024-11 (CoRL 2024, Munich, 6 to 9 November 2024). arXiv v1 2024-10-24.
- **Venue:** 8th Conference on Robot Learning (CoRL 2024), PMLR. The paper footer reads "8th Conference on Robot Learning (CoRL 2024), Munich, Germany". Presentation type not found.
- **Links:** arXiv https://arxiv.org/abs/2410.18912 · Project https://gs-dynamics.github.io · Code https://github.com/robo-alex/gs-dynamics · Demo https://huggingface.co/spaces/kaifz/gs-dynamics
- **Number of citations:** 60 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
GS-Dynamics learns an action-conditioned particle world model of deformable objects directly from real multi-view robot-interaction videos. The pipeline has four steps:
1. A modified Dynamic 3D Gaussians optimisation produces dense, long-horizon 3D tracks.
2. A GNN is trained on sparse control particles sampled from the tracks to predict particle motion given robot end-effector actions.
3. Predicted motion is spread to all Gaussians with linear blend skinning. This gives action-conditioned 3D video prediction.
4. The same model is used for MPPI-based MPC planning.

## 1. Weights availability
- Released. Checkpoints are on Google Drive (https://drive.google.com/drive/folders/1N9AbTgCi9_Wd1gNeNljqml_wcFeIu_6_), to be placed as `log/{rope,sloth,...}/checkpoints/latest.pth` (README).
- Interactive demo: https://huggingface.co/spaces/kaifz/gs-dynamics
- License: MIT (repository LICENSE, copyright 2024 by the three authors).

## 2. Code
- https://github.com/robo-alex/gs-dynamics, MIT license. The README covers:
  - Data preparation (GroundingDINO+SAM masks, initial point cloud, metadata).
  - Dyn3DGS tracking (`train_gs.py`).
  - Preprocessing, dynamics training (`train.py --config config/rope.yaml`) and evaluation (`predict.py`).
- Last-commit date not verified (GitHub API rate-limited).

## 3. Datasets
- Real-world data collected by the authors (Sec. 4.1):
  - 8 object instances across rope, cloth and toy animals.
  - 4 RGB-D cameras at the workspace corners, top-down, 15 FPS, with synchronised robot actions.
- Tracking ground truth: 200 hand-labelled 3D frames per episode, for 1 or 2 instances per category and two episodes per object (Table 1).
- Public: the README links Google Drive data folders (episodes for rope, dog and others). Total size not stated.

## 4. Evaluation and downstream tasks
- **3D tracking, 3D median trajectory error in mm (Table 1):**

  | Method | Rope | Cloth | Toy animals |
  |---|---|---|---|
  | This model | 6.90 | 13.14 | 12.83 |
  | SpatialTracker | 38.72 | 53.85 | 42.82 |
  | CoTracker | 46.96 | 55.84 | 51.84 |
  | PIPS++ | 49.37 | 58.10 | 58.45 |
  | Unmodified Dyn3DGS | 62.41 | 69.20 | 66.19 |

  The model also has the best δ_avg and survival rate in 2D and 3D. For example, 3D δ_avg on rope is 89.26 vs 86.56 for SpatialTracker.
- **Action-conditioned 3D video prediction (Table 2).** Baselines are MPM and FleX with CMA-ES-fitted parameters. Values are this model / MPM / FleX:

  | Metric | Cloth | Toy animals | Rope |
  |---|---|---|---|
  | 3D Chamfer ↓ | 0.064 / 0.076 / 0.136 | 0.030 / 0.052 / 0.069 | 0.053 / 0.073 / 0.074 |
  | J&F ↑ | 0.492 / 0.359 / 0.225 | 0.714 / 0.600 / 0.438 | 0.543 / 0.498 / 0.426 |
  | LPIPS ↓ (no FleX) | 0.044 / 0.057 | 0.021 / 0.027 | 0.025 / 0.030 |

  3D EMD shows the same ordering.
- **Model-based planning (Fig. 5, 5 runs each):** rope straightening and toy-doll relocating. Error decreases with planning steps, and success rates are high across thresholds (plots only). MPM and FleX were too slow for real-world planning, so no baseline is given.
- **Downstream tasks:** dense 3D tracking, action-conditioned video prediction and real-robot MPC.

## 5. Architecture and techniques
- **Tracking:** Dyn3DGS, modified so colour, opacity and scale are fixed and only position and rotation are optimised. It uses stronger local-rigidity and isometry losses for the 4-view setup, keeps only high-opacity Gaussians, and segments with GroundingDINO+SAM.
- **Dynamics GNN:**
  - Vertices are farthest-point-sampled control particles plus the end-effector. Edges connect pairs within distance d_e.
  - Vertex input: k = 3 past motions, an object or robot one-hot, and distance to the table.
  - Edge input: relative position and relation type.
  - p = 3 message-passing steps. The output is a per-vertex displacement.
- **Training:** MSE over τ = 5-step recurrent rollouts, plus optional edge-length and rigidity regularisers.
- **Rendering:** per-vertex rotations by Procrustes over neighbours, then LBS with inverse-distance weights to move Gaussian centers and rotations.
- **Planner:** MPPI-based MPC with an MSE-to-target cost.
- **Size:** parameter count not stated.

## 6. Conclusions and future work
- **Authors:** learning graph-based neural dynamics from real videos through 3D Gaussian tracking outperforms simulator-fitting baselines in motion and video prediction and supports real-world planning.
- **Authors' limitations:** real data collection is costly, and perception fails under large occlusions or on textureless objects.
- **Authors' future work:** more efficient data collection and more robust perception.
- **My own observations:**
  - Models are trained per object instance, so cross-object generalisation is not shown. That gap is addressed later by PGND and PhysTwin.
  - The dataset is small (8 instances).
  - Planning results have no baseline.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2410.18912 (v1, full main text read).
- README and LICENSE: raw.githubusercontent.com (robo-alex/gs-dynamics).
- Project page: https://gs-dynamics.github.io
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - The PMLR page (CoRL 2024 volume not fetched).
  - Total data size.
  - Repository last update.
