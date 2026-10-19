---
citation_count: 90
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; re-check on 2026-10-15 returned HTTP 429)"
venue: "RSS 2024"
date: 2024-07-10
research_group: "Columbia (Yunzhu Li)"
---

# AdaptiGraph: Material-Adaptive Graph-Based Neural Dynamics for Robotic Manipulation

- **Authors:** Kaifeng Zhang\*, Baoyu Li\*, Kris Hauser, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author. The affiliations are UIUC and Columbia.
- **Date published:** 2024-07 (RSS 2024, Delft, 15 to 19 July 2024). arXiv v1 2024-07-10.
- **Venue:** Robotics: Science and Systems (RSS) XX, 2024, paper p010 (https://roboticsproceedings.org/rss20/p010.html, confirmed in the proceedings index). The work also won the Best Abstract Award at the ICRA 2024 RMDO workshop (project page).
- **Links:** arXiv https://arxiv.org/abs/2407.07889 · Project https://robopil.github.io/adaptigraph/ · Code https://github.com/Boey-li/AdaptiGraph
- **Number of citations:** 90 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
AdaptiGraph is a graph-based neural dynamics (particle) world model conditioned on material type and a continuous physical-property variable φ. Examples of φ are rope or cloth stiffness, granule size, and a box's center of pressure. When the robot meets an unseen object, it performs a few curiosity-driven interactions and estimates φ by inverse optimisation of prediction error (Bayesian optimisation or CMA-ES). It then plans with MPPI-based MPC on the adapted model. A single model covers ropes, granular piles, rigid boxes and cloth.

## 1. Weights availability
- Released. The README links model checkpoints for each material on Google Drive (https://drive.google.com/drive/folders/1HR39B7PXbYkM2w3XcA3feTCRCZH1q7UG), to be placed in `./log/{data_name}/checkpoints`.
- License: MIT (repository LICENSE, copyright 2024 Baoyu Li).

## 2. Code
- https://github.com/Boey-li/AdaptiGraph, MIT license. Contains:
  - Simulation data generation with PyFleX (`sim/data_gen/data_gen.py`).
  - Preprocessing, training, and rollout and evaluation scripts.
  - Optional real-world planning, which uses GroundingDINO+SAM weights via gdown.
- Last-commit date not verified (GitHub API rate-limited).

## 3. Datasets
- **Training (simulation):** NVIDIA FleX for rope, granular and cloth, and Pymunk for boxes. 1,000 episodes per material, each with 5 random push or pull interactions and randomised physical properties (Sec. IV-B).
  - The released sample is 100 episodes (98 train, 2 validation). The full data was not released because of its size; generation code is provided.
- **Real-world tests (Fig. 3):** 22 objects in total, recorded with 4 calibrated RealSense D455 cameras (1280×720, 15 Hz) and an xArm 6:
  - 9 ropes.
  - 9 granular materials.
  - 5 cloths.
  - 2 boxes with movable weights.

## 4. Evaluation and downstream tasks
- **Forward prediction (simulation, 200 test objects per material, Fig. 5):** lower and more consistent error than GNN (a per-material model without conditioning) and "Ours w/o Adaptation" (mean φ). Rigid-box prediction is near perfect. Values are given only as box plots.
- **Physical property estimation on real objects (Fig. 6), 10 interactions each:**
  - Box center of pressure: predicted close to ground truth for all 8 configurations, converging after about 5 interactions.
  - Rope stiffness: the ranking matches human perception, and the estimate extrapolates beyond the training range.
  - Granularity: the ranking matches grain size (granola smallest, toy blocks largest).
  - Cloth: the modal fabric is softest, and a plastic sheet not seen in training is estimated as very stiff.
- **Closed-loop MPC on real objects (Figs. 7–8, maximum 10 steps, 5 repeats):**
  - Tasks: box pushing, rope straightening, granular gathering and cloth relocating.
  - The method reaches lower error with fewer steps and has higher success across error thresholds than the model without adaptation.
  - Example final Chamfer distances from Fig. 7: box 0.01 vs 0.80; granular 0.008 vs 0.06. I read these from the figure labels, and the pairing is my interpretation.
- **Appendix A (not read in detail):** comparisons with fine-tuning the GNN and with adapting a physics simulator.
- **Downstream tasks:** dynamics prediction, online system identification and model-based manipulation planning (4 tasks).

## 5. Architecture and techniques
- **State:** a particle graph from farthest-point sampling of fused point clouds. GroundingDINO and SAM segment the scene, and edges are added within a radius. The robot end-effector is represented as particles.
- **Model:** a GBND model in the style of propagation networks:
  - Vertex and edge encoders.
  - L message-passing steps.
  - A vertex decoder that predicts the next particle positions.
- **Inputs:** vertex features carry 3-step position history, object or robot flag, φ and material M. Translation equivariance comes from feeding relative positions to the edge encoder.
- **Training:** MSE over K = 3-step rollouts with backpropagation through time.
- **Adaptation:** actions are chosen by MPPI to maximise predicted displacement (curiosity). φ is then fitted to the observed outcomes by minimising Chamfer distance, with BO for 1-D φ and CMA-ES for multi-D φ. The search range extends 20% beyond the training range.
- **Planner:** MPPI-based MPC with task cost plus penalty terms.
- **Size:** parameter count not stated.

## 6. Conclusions and future work
- **Authors:** AdaptiGraph simulates several deformable materials and adapts to varying physical properties at deployment.
- **Authors' future work:** cover more materials, cover more physical properties per material, and model heterogeneous multi-material interactions with a material-conditioned heterogeneous graph.
- **My own observations:**
  - The material type must be known in advance.
  - Training is entirely in simulation, so the sim-to-real gap is mitigated only by matching tools and by the particle abstraction.
  - Quantitative results are given mainly as plots.
  - The system relies on four calibrated depth cameras.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2407.07889 (v1, main text read; appendix not read in detail).
- README and LICENSE: raw.githubusercontent.com (Boey-li/AdaptiGraph).
- Project page: https://robopil.github.io/adaptigraph/
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - Exact plotted values.
  - Repository last update.
