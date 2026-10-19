---
citation_count: 143
citation_source: "Semantic Scholar, 2026-10-15"
venue: "RSS 2022"
date: 2022-05-05
research_group: "Stanford (Jiajun Wu)"
---

# RoboCraft: Learning to See, Simulate, and Shape Elasto-Plastic Objects with Graph Networks

- **Authors:** Haochen Shi*, Huazhe Xu*, Zhiao Huang, Yunzhu Li, **Jiajun Wu**. *Equal contribution, random order.
- **Group role:** senior (last) author. Yunzhu Li, PI of summary group 8 and then at MIT, is second-to-last.
- **Date published:**
  - RSS 2022: conference held June–July 2022; arXiv posted 2022-05-05.
  - IJRR journal version: online 2023-12-18; print 2024-04, vol. 43, issue 4, pp. 533–549 (Crossref).
- **Venue:**
  - Robotics: Science and Systems (RSS) 2022.
  - Extended journal version: *RoboCraft: Learning to see, simulate, and shape elasto-plastic objects in 3D with graph networks*, International Journal of Robotics Research (IJRR) 43(4), 2024. DOI 10.1177/02783649231219020.
- **Links:**
  - arXiv: https://arxiv.org/abs/2205.02909
  - Project page: http://hxu.rocks/robocraft/
  - Code: https://github.com/hshi74/RoboCraft
  - IJRR: https://doi.org/10.1177/02783649231219020
- **Number of citations:** 141 (source: Semantic Scholar API, checked 2026-10-13). Semantic Scholar merges the RSS and IJRR versions under the IJRR DOI.

## Summary
RoboCraft learns a particle-based, action-conditioned dynamics model (a GNN) of plasticine directly from raw multi-view RGB-D data, using only 10 minutes of real robot interaction. The model is trained with distribution losses (EMD and Chamfer), so it needs no particle correspondence between frames. A goal-conditioned MPC planner then uses it: random shooting followed by L-BFGS gradient refinement, to pinch the plasticine into target shapes such as letters. It is the root of the Wu-lab particle world-model line (RoboCraft → RoboCook → RoboPack).

## 1. Weights availability
Not released. The README gives instructions to train the dynamics model and then point `control.sh` at "the checkpoint you just trained". No pretrained checkpoint link appears in the README or on the project page.

## 2. Code
- **Repository:** https://github.com/hshi74/RoboCraft
  - Contents: simulation data generation (PlasticineLab/Taichi-based simulator), particle sampling, GNN dataset building, dynamics training and MPC planning.
  - The README points to a `dev` branch for the real-world version and to the RoboCook repo.
  - No LICENSE file (raw `LICENSE` returns 404).
  - Last commit: 2026-08-23 (commits Atom feed).
- Training and evaluation code are both included.

## 3. Datasets
- **Real data:** 6,000 frames (10 min), 50 episodes of 120 frames, 3 random grips per episode. Recorded on a Franka Panda with 4 RealSense D415 cameras and blue Play-Doh. I found no public release of the real data.
- **Simulation data:** generated with PlasticineLab-style tasks. The paper used 50 videos per task, and the scripts regenerate them.
- **Benchmark shapes:** 26 letters plus 5 other shapes in simulation; 7 letters plus 5 shapes on the real robot.

## 4. Evaluation and downstream tasks
- **Particle sampling (Table I, simulation):** the crop-based sampler beats the patch-based one. CD 0.0374 vs 0.0384; EMD 0.0308 vs 0.0317.
- **Dynamics prediction (Table II, 12,000 frames):** the mixed loss (0.9 EMD + 0.1 CD) gives CD 0.0327, EMD 0.0231 and Hausdorff 0.0790. Pure CD loss gives HD 0.0851; pure EMD loss gives HD 0.1068. Fig. 5 shows the error growing but bounded over 120 steps.
- **Planning (Fig. 7):** gradient-based planning with L-BFGS beats CEM, random shooting and model-based SAC (shown as a figure only).
- **Tool selection (Table III):** CD 0.0408 vs 0.0409 and EMD 0.0337 vs 0.0345, a marginal gain.
- **Real robot:**
  - Qualitative shaping of unseen letters (Figs. 10–11).
  - Better than an MPM simulator with hand-set parameters and than a GNN trained in simulation (Fig. 12, qualitative).
  - Generalizes to new initial shapes and to modeling foam (Fig. 13).
- **Against amateur humans (Table IV, simulation):** RoboCraft CD 0.0359 / EMD 0.0340; humans CD 0.0655 / EMD 0.0661.
- **Downstream task:** model-based shaping and control (MPC).

## 5. Architecture and techniques
- **Perception:**
  1. RANSAC removes the table plane.
  2. Poisson surface reconstruction.
  3. Particles are sampled inside the mesh using its SDF.
  4. Refinements: a physics prior (patch missing points from the previous frame) and a shape prior (subtract the finger SDFs).
  5. Voxel and farthest-point downsampling.
- **Dynamics:**
  - Interaction-network-style GNN over object and gripper particles, with edges within d = 0.05.
  - 3-layer MLP encoders (150 units), a propagator and a motion predictor; multi-step message passing.
  - Trained 100 epochs with Adam, learning rate 1e-4, batch size 4.
- **Loss:** 0.9·EMD + 0.1·CD between predicted and observed particle sets. No correspondence is needed.
- **Action space:** each grip is parameterized as {x, y, z, r_z, d}.
- **MPC:** fixed look-ahead; one grip is executed at a time; visual feedback only between grips.
- **IJRR extension:** the title adds "in 3D". I did not read the journal version in detail.

## 6. Conclusions and future work
- **Authors' conclusions:**
  - Presented as the first model-based framework that shapes elasto-plastic objects into complex unseen targets in simulation and on a real robot, from 10 minutes of exploration.
  - It outperforms amateur humans on distance metrics.
  - Future directions: harder tasks, complex action spaces, mixed motion modes and multiple tools. These were later followed up in RoboCook.
- **Own observations:**
  - Real-robot results are mostly qualitative.
  - Distance metrics disagree with human recognizability, which the authors acknowledge.
  - A single tool and a single material family were tested.
  - I did not check the IJRR version's additional experiments.

## 7. Sources and verification
- **Read:** arXiv PDF v1 https://arxiv.org/pdf/2205.02909, Secs. I–VI and Tables I–IV.
- **Checked:**
  - Crossref API for DOI 10.1177/02783649231219020 (IJRR metadata).
  - Semantic Scholar API `paper/arXiv:2205.02909`.
  - Repo README https://github.com/hshi74/RoboCraft.
  - Wu's homepage, which lists both RSS 2022 and IJRR 2024.
- **Not verified:** the contents of the IJRR journal version. The number in Table I's "Crop-Based" row is taken from the extracted table ordering.
