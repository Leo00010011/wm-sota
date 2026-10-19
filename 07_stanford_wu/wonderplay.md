---
citation_count: 56
citation_source: "Semantic Scholar, 2026-10-13 (carried from existing note; live re-check rate-limited on 2026-10-15)"
venue: "ICCV 2025"
date: 2025-05-23
research_group: "Stanford (Jiajun Wu)"
---

# WonderPlay: Dynamic 3D Scene Generation from a Single Image and Actions

- **Authors:** Zizhang Li*, Hong-Xing Yu*, Wei Liu, Yin Yang, Charles Herrmann, Gordon Wetzstein, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author; first authors are Wu's group members at Stanford
- **Date published:** 2025-10 (ICCV 2025 conference, October 2025; arXiv v1 2025-05-23, v2 2025-11-29)
- **Venue:** ICCV 2025, Highlight (per the arXiv comment), proceedings pp. 9080–9090 (DOI 10.1109/ICCV51701.2025.00849)
- **Links:**
  - arXiv: https://arxiv.org/abs/2505.18151
  - CVF: https://openaccess.thecvf.com/content/ICCV2025/html/Li_WonderPlay_Dynamic_3D_Scene_Generation_from_a_Single_Image_and_ICCV_2025_paper.html
  - Project: https://kyleleey.github.io/WonderPlay/
  - Code: https://github.com/kyleleey/WonderPlay
- **Number of citations:** 56 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
WonderPlay is an action-conditioned dynamic 3D world model built from a single image. Given gravity, a wind force field or a 3D point force, it predicts the dynamic 3D scene that results. Its "hybrid generative simulator" works in three steps:
1. Physics solvers (rigid, MPM, PBD) produce coarse 3D dynamics.
2. These condition a video diffusion model through flow and RGB.
3. The generated video is used to refine the 3D dynamics.

It handles rigid bodies, elastic objects, cloth, liquid, smoke and granular material.

## 1. Weights availability
- No new weights were trained. The method combines pre-trained components:
  - Go-with-the-Flow, a flow-conditioned CogVideoX I2V model
  - InstantMesh
  - SAM and RepViT-SAM
  - DUSt3R
  - A monocular depth model
  - The Genesis physics engine
- The README downloads `repvit_sam.pt` from the THU-MIG GitHub release. It notes that 80 GB of GPU memory is required.

## 2. Code
- https://github.com/kyleleey/WonderPlay. It contains a modified Genesis, the WonderPlay pipeline, video-model code and one example scene ("venice"). The pipeline has two stages: reconstruction plus simulation, then video-model refinement.
- No LICENSE file was found (raw `LICENSE` returns 404).
- Last commit: 2026-03-03 (commits Atom feed).
- No training code is needed because nothing is trained.

## 3. Datasets
- No training data.
- Evaluation used 15 curated examples (7 real photos and 8 realistic synthetic images) covering cloth, rigid, elastic, liquid, gas and granular content (Sec. 4).
- Appendix A adds a staged set of 4 scenes of increasing complexity.
- Neither set has been released as a benchmark.

## 4. Evaluation and downstream tasks
- **Baselines:**
  - Physics-based: PhysGen (2D rigid) and PhysGaussian (MPM elastic, run on WonderPlay's reconstruction).
  - Video generation: CogVideoX-I2V (text prompt) and Tora (drag trajectories taken from WonderPlay's simulation).
- **Metrics:** VBench imaging, aesthetic, motion smoothness and consistency, plus the GPT-4o "PhysReal" score.
- **Results (Table 2, 15 scenes):**

  | Method | Imaging | Aesthetic | Motion | Consistency | PhysReal |
  |---|---|---|---|---|---|
  | WonderPlay | 0.695 | 0.610 | 0.995 | 0.217 | 0.700 |
  | CogVideoX | 0.686 | 0.574 | 0.993 | 0.219 | 0.670 |
  | PhysGen | 0.692 | 0.593 | 0.992 | 0.212 | 0.545 |

  WonderPlay is best or second-best on every metric.
- **User study (Table 1, 200 participants, 2AFC):** preferred over the baselines by 71–85% across physics plausibility, motion fidelity and visual quality. Examples:
  - 80.2% for physics plausibility over PhysGaussian.
  - 85.2% for visual quality over PhysGaussian.
- **Ablation (Table S1):** PhysReal falls from 0.700 (full) to 0.670 (w/o RGB), 0.650 (w/o flow) and 0.500 (coarse simulation only).
- **Downstream tasks:** none (no planning, RL or robotics). The task is interactive, action-conditioned generation of dynamic 3D scenes.

## 5. Architecture and techniques
- **Scene representation:**
  - Background: FLAGS surfels, the same representation as WonderWorld.
  - Objects: "topological Gaussian surfels", meaning surfels bound to InstantMesh vertices, plus edges and velocities, registered to the scene with DUSt3R.
- **Materials:**
  - A VLM classifies each object into one of 6 classes (rigid, elastic, cloth, smoke, liquid, granular).
  - The VLM also estimates physical parameters, with optional manual tuning.
- **Solvers (in Genesis):**
  - Shape-matching rigid solver.
  - MPM for elastic, liquid and granular materials.
  - PBD for cloth and smoke.
  - Simulation runs 960 steps, rendering one frame every 20 steps.
- **Generative refinement:** Go-with-the-Flow, a latent video diffusion model (CogVideoX-based), using flow-warped noise from the simulated velocities.
  - RGB conditioning uses SDEdit-style partial denoising from the rendered coarse video.
  - Spatially varying "responsibility" (s1 = 21, s2 = 18 of S = 25 DDIM steps) keeps the background closer to the simulator.
  - Output is 49 frames at 480×720.
- **Closing the loop:** a photometric L1 loss fits object trajectories, appearance and background colour to the generated video.
- **Action conditioning:** explicit 3D forces (gravity, wind field f_w(x,y,z,t), point force f_p(t)).

## 6. Conclusions and future work
- **Authors' conclusions:** the hybrid generative simulator gives better simulation fidelity and visual quality than purely physics-based or purely video-based methods across diverse materials. No explicit limitations or future-work section is given in the paper.
- **Own observations:**
  - Material class and parameters come from a VLM with optional manual adjustment, so results may need per-scene tuning.
  - The method is open-loop and short-horizon (one action sequence). PerpetualWonder (CVPR 2026) targets the long-horizon case.
  - The evaluation is small (15 scenes) and relies on VBench and GPT-4o proxies. There is no ground-truth physics comparison.
  - The 80 GB GPU requirement limits use.

## 7. Sources and verification
- arXiv PDF v2: https://arxiv.org/pdf/2505.18151. Read Secs. 1–5, Tables 1–2, Appendix A–B (Tables S1, S2).
- Venue and pages are confirmed by the CVF open-access page and the ICCV 2025 virtual poster page (https://iccv.thecvf.com/virtual/2025/poster/2645), found via web search. "Highlight" is taken from the arXiv comment.
- Repo README: https://github.com/kyleleey/WonderPlay. Semantic Scholar API: `paper/arXiv:2505.18151`.
- Not verified: runtime per scene (not reported in the paper).
