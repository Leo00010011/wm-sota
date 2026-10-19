---
citation_count: 44
citation_source: "Semantic Scholar, 2026-10-15"
venue: "RSS 2025"
date: 2025-06-18
research_group: "Columbia (Yunzhu Li)"
---

# Particle-Grid Neural Dynamics for Learning Deformable Object Models from RGB-D Videos (PGND)

- **Authors:** Kaifeng Zhang, Baoyu Li, Kris Hauser, **Yunzhu Li**
- **Group role:** Senior (last) author. The affiliations are Columbia and UIUC.
- **Date published:** 2025-06 (RSS 2025, Los Angeles, 21 to 25 June 2025). arXiv v1 2025-06-18 (v2 2025-11-06).
- **Venue:** Robotics: Science and Systems (RSS) XXI, 2025, paper p036 (https://roboticsproceedings.org/rss21/p036.html, confirmed in the proceedings index). Presentation type not found.
- **Links:** arXiv https://arxiv.org/abs/2506.15680 · Project https://kywind.github.io/pgnd · Code https://github.com/kywind/pgnd · Checkpoints https://huggingface.co/kaifz/pgnd-checkpoints · Data https://huggingface.co/datasets/kaifz/pgnd-dataset · Demo https://huggingface.co/spaces/kaifz/pgnd
- **Number of citations:** 42 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
PGND is a learned 3D world model for deformable objects, trained only on sparse-view RGB-D recordings of robots interacting with them. Each prediction step works as follows:
1. A PointNet encodes dense tracked particles: position and velocity history.
2. An MLP neural velocity field, queried with locality-pooled features, predicts velocities on a fixed Eulerian grid.
3. Grid velocity editing imposes the grasp and ground constraints. For pushing, robot particles are added instead.
4. MPM-style B-spline grid-to-particle transfer interpolates velocities back to the particles.
5. Euler integration moves the particles.

Combined with 3D Gaussian Splatting via LBS, it produces 3D action-conditioned video. It is also used for MPPI-based MPC.

## 1. Weights availability
- Released. A versioned Hugging Face release (tag `release-20260831`) is in the README:
  - Checkpoints: https://huggingface.co/kaifz/pgnd-checkpoints, downloaded with `hf download kaifz/pgnd-checkpoints ...`.
  - Dataset: https://huggingface.co/datasets/kaifz/pgnd-dataset, with per-category download and checksum verification.
- Interactive demo on Hugging Face Spaces.
- License: the repo LICENSE contains MIT-style permission text, but its copyright line reads "2011-2024 GitHub Inc.", which looks like a copied template. Licensing of the HF artefacts was not checked.

## 2. Code
- https://github.com/kywind/pgnd. Contains:
  - Training and evaluation scripts.
  - The custom-data processing pipeline (2025-07).
  - Rendering with Gaussian Splatting.
- Initial release 2025-06-18; script and data fixes 2026-01-05; versioned HF dataset 2026-08-31 (README news).

## 3. Datasets
- **Own real-world data:** a bimanual xArm (7-DoF arms) with 4 RGB-D cameras and random teleoperated interactions. Particles come from SAM-2 masks plus CoTracker tracks, fused in 3D.
- **Training duration (App.):** cloth 20.3 min, rope 21.7, plush toy 3.8, box 10.1, paper bag 6.7, bread 4.8.
- **Test:** 3-second episodes; 40 each for cloth and rope, 20 for the other categories.
- **Category-level split:**
  - Ropes: 6 instances, 2 unseen (cotton and utility rope).
  - Cloths: 8 instances, 2 unseen (flannel blanket and foam sheet).
- Public on Hugging Face (above).

## 4. Evaluation and downstream tasks
- **Dynamics prediction over 3 s, mean distance error in metres** (Table I, lower is better). The "Particle" baseline is the model without the grid. CD and EMD follow the same ordering.

  | Category | PGND | Particle | GBND | MPM |
  |---|---|---|---|---|
  | Cloth | 0.045 | 0.059 | 0.077 | 0.176 |
  | Rope | 0.039 | 0.061 | 0.062 | 0.138 |
  | Plush | 0.043 | 0.060 | 0.078 | 0.163 |
  | Box | 0.022 | 0.025 | 0.045 | — |
  | Bag | 0.016 | 0.021 | 0.030 | 0.226 |
  | Bread | 0.020 | 0.038 | 0.031 | 0.034 |

- **3D action-conditioned video prediction** (Table II; J-score/IoU, F-score, LPIPS). PGND has the best LPIPS in every category, for example cloth 0.099 vs GBND 0.120 and MPM 0.141. J and F are best in most categories, but not for box or bag, where the authors attribute baseline gains to the baselines barely moving particles and to spiky Gaussians.
- **Sparse views (Fig. 4):** with 1–4 input views on cloth and bag, error is lower than GBND and degrades less as views are removed.
- **Category-level generalisation (Fig. 5):** lower error than GBND on seen and unseen rope and cloth instances. GBND degrades sharply on unseen cloth.
- **Planning (Figs. 8–9):** 10 trials each of cloth lifting, box closing, rope manipulation and plush relocating. Terminal Chamfer distance is lower and success is higher than GBND. Example final CDs: cloth 0.013 vs 0.036, box 0.021 vs 0.047.
- **Downstream tasks:** dynamics prediction, 3D action-conditioned video prediction, simulation from phone-scanned Gaussian scenes, and MPC planning on a real bimanual robot.

## 5. Architecture and techniques
- **Encoder and field:** a PointNet point encoder feeds a neural velocity-field MLP. The MLP takes the sinusoidal positional encoding of each grid point plus the average-pooled features of particles within r = 0.2 m.
- **Grid:** 50 or 100 cells per axis, δ = 1–2 cm.
- **Grid-to-particle transfer:** B-spline interpolation with forward Euler, using history h = 2.
- **Action conditioning:**
  - Grasping uses grid velocity editing: grid points near the grasp take the gripper's rigid velocity, and ground contact uses projection plus friction.
  - Non-prehensile pushing (box) uses robot particles.
- **Training:** MSE over K = 5-step rollouts.
- **Rendering:** 3DGS deformed by LBS.
- **Planner:** MPPI-based MPC with a Chamfer cost.
- **Paradigm:** deterministic, hybrid Lagrangian–Eulerian neural simulator. Parameter count not stated.

## 6. Conclusions and future work
- **Authors:** the hybrid particle-grid representation beats graph-based neural dynamics in accuracy and density. It gives a learned digital twin with appearance and improves planning.
- **Authors' limitations:**
  - The number of particles is fixed, so appearing or disappearing particles cannot be handled. They suggest re-observing or modelling visibility.
  - Physical properties are inferred only implicitly; a parameter-conditioned model in the style of AdaptiGraph could help.
  - The method depends on SAM, CoTracker and 3DGS, so perception failures propagate.
- **My own observations:**
  - Training data is small: minutes per category.
  - Models are per category, not one cross-category world model.
  - Evaluation horizons are 3 s.
  - Planning success is reported as curves, not tables.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2506.15680 (v2, main text, limitations and data appendix read).
- README and LICENSE: raw.githubusercontent.com (kywind/pgnd).
- Project page: https://kywind.github.io/pgnd
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - Hugging Face licenses.
  - The exact license intent given the template copyright line.
