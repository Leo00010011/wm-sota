---
citation_count: 132
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICCV 2025"
date: 2025-03-23
research_group: "Columbia (Yunzhu Li)"
---

# PhysTwin: Physics-Informed Reconstruction and Simulation of Deformable Objects from Videos

- **Authors:** Hanxiao Jiang, Hao-Yu Hsu, Kaifeng Zhang, Hsin-Ni Yu, Shenlong Wang, **Yunzhu Li**
- **Group role:** Senior (last) author. The affiliations are Columbia and UIUC; Shenlong Wang (UIUC) is second-to-last.
- **Date published:** 2025-10 (ICCV 2025, Honolulu, 19 to 23 October 2025). arXiv v1 2025-03-23.
- **Venue:** IEEE/CVF International Conference on Computer Vision (ICCV) 2025, DOI 10.1109/ICCV51701.2025.00678 (from Semantic Scholar). The project page and PI's homepage list ICCV 2025, and an abridged version appeared at an ICRA 2025 workshop. Oral or poster status not verified.
- **Links:** arXiv https://arxiv.org/abs/2503.17973 · IEEE https://doi.org/10.1109/ICCV51701.2025.00678 · Project https://jianghanxiao.github.io/phystwin-web/ · Code https://github.com/Jianghanxiao/PhysTwin · Data https://huggingface.co/datasets/Jianghanxiao/PhysTwin
- **Number of citations:** 127 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
PhysTwin builds an interactive, physics-structured world model, a "physical digital twin", of a deformable object from three RGB-D videos of a single interaction. It combines three parts:
- A dense spring-mass simulator with learned topology, per-spring stiffness, collision and damping parameters, and control springs that connect tracked hand points to the object.
- A full shape completed by the TRELLIS image-to-3D generative prior.
- Gaussian splats moved by linear blend skinning for rendering.

Parameters are fitted with a sparse-to-dense scheme: first zero-order CMA-ES on topology and homogeneous parameters, then gradients on dense stiffness. The fit uses Chamfer, CoTracker3-based 3D tracking and rendering losses. The twin runs in real time (implemented in Warp) and supports action-conditioned prediction, teleoperation preview and model-based robot planning.

## 1. Weights availability
- Per-object optimised results are released rather than a general network. Hugging Face dataset repo https://huggingface.co/datasets/Jianghanxiao/PhysTwin holds:
  - `data.zip`: raw and processed data.
  - `experiments_optimization.zip`: first-stage zero-order results.
  - `experiments.zip`: second-stage results.
  - `gaussian_output.zip`: static Gaussians.
  - `additional_data.zip`: extra cloth demos.
- The README explains how to load these into the interactive playground.
- License: MIT (repo LICENSE, copyright 2025 Hanxiao Jiang). No separate license is stated for the data.

## 2. Code
- https://github.com/Jianghanxiao/PhysTwin, MIT license. Contains:
  - Data processing, which downloads the pretrained perception models with `env_install/download_pretrained_models.sh`.
  - Training (`script_train.py`), Gaussian training, evaluation scripts, the interactive playground and Docker scripts.
- Actively maintained. README news entries run to 2026-08-24, including:
  - Self-collision speed-ups (2025-10).
  - Real2Sim-Eval robot support (2025-11).
  - Deform360 data (2026-07).
  - The batched simulator Boba, listed as accepted to ECCV 2026.

## 3. Datasets
- **Own dataset (Sec. 5.1):** 22 scenarios recorded with three RealSense D455 RGB-D cameras.
  - Objects: ropes, stuffed animals, cloth and delivery packages. Interactions: lifting, stretching, pushing and squeezing with one or two hands, in clips of 1–10 s.
  - Each video is split 7:3 into train and test frames, with 9 manually annotated tracking points per video.
  - A generalisation set contains interaction pairs on the same object.
  - Public on Hugging Face (link above).

## 4. Evaluation and downstream tasks
- **Baselines:** Spring-Gaus, augmented with control, and GS-Dynamics, strengthened with the same CoTracker3 3D tracker. Metrics are one-way Chamfer distance and track error (3D), and IoU, PSNR, SSIM and LPIPS (2D, centre view).
- **Table 1, PhysTwin vs GS-Dynamics vs Spring-Gaus:**

  | Metric | Reconstruction & resimulation | Future prediction |
  |---|---|---|
  | CD ↓ | 0.005 vs 0.014 vs 0.041 | 0.012 vs 0.041 vs 0.062 |
  | Track error ↓ | 0.009 vs 0.022 vs 0.050 | 0.022 vs 0.070 vs 0.094 |
  | IoU % ↑ | 84.4 vs 72.1 vs 57.6 | 72.5 vs 49.8 vs 46.4 |
  | PSNR ↑ | 28.21 vs 26.26 vs 23.45 | 25.62 vs 22.54 vs 22.49 |
  | SSIM ↑ | 0.945 vs 0.940 vs 0.928 | 0.941 vs 0.924 vs 0.924 |
  | LPIPS ↓ | 0.034 vs 0.052 vs 0.102 | 0.055 vs 0.097 vs 0.113 |

  Spring-Gaus metrics are reported only for its successful (non-crashing) cases.
- **Generalisation to unseen interactions, PhysTwin vs GS-Dynamics (Table 2):**

  | Metric | PhysTwin | GS-Dynamics |
  |---|---|---|
  | CD ↓ | 0.013 | 0.029 |
  | Track error ↓ | 0.018 | 0.038 |
  | IoU % ↑ | 72.18 | 63.4 |
  | PSNR ↑ | 26.20 | 25.05 |
  | SSIM ↑ | 0.938 | 0.934 |
  | LPIPS ↓ | 0.047 | 0.067 |

- **Applications (Sec. 5.3, Fig. 5):** qualitative only:
  - Real-time interactive simulation with keyboard or robot teleoperation.
  - Model-based robot planning, e.g. lifting a rope into a target configuration.
  - No quantitative planning results in the paper.
- **Downstream tasks:** resimulation, future prediction, generalisation to new actions, interactive simulation and robot planning. Later reused for policy evaluation in Real2Sim-Eval (ICRA 2026).

## 5. Architecture and techniques
- **Dynamics:** a spring-mass model with spring and dashpot forces, drag damping, and impulse-based collision (object–collider and self-collision). It uses explicit Euler integration in a custom differentiable simulator built on NVIDIA Warp.
- **Actions:** control points from Grounded-SAM2 hand masks, CoTracker3 tracks and farthest-point sampling, attached to the object with springs.
- **Geometry:** a TRELLIS mesh from a super-resolved masked image. It is registered with SuperGlue, PnP, scale and translation optimisation, ARAP and ray-casting.
- **Optimisation:**
  - Stage 1: CMA-ES on non-differentiable topology (radius, maximum neighbours) and homogeneous physical parameters.
  - Stage 2: gradient descent on dense spring stiffness and collision parameters.
  - Losses: one-way Chamfer plus 3D tracking error.
- **Appearance:** isotropic static Gaussians optimised on the first frame (L1 + D-SSIM), then deformed with LBS.
- **Paradigm:** physics-based simulation with system identification. There is no neural dynamics network, and no parameter count applies.

## 6. Conclusions and future work
- **Authors:** PhysTwin reconstructs simulatable twins from sparse videos and beats prior work at resimulation, prediction and generalisation. It supports fast simulation for robotics.
- **Authors' future work (Supp. D):**
  - Even sparser input, down to a single RGB video, for in-the-wild use.
  - Several interaction types to better estimate intrinsic properties.
  - Real-to-sim transfer for RL with less domain randomisation.
  - More model-based planning.
- **My own observations:**
  - Each twin is optimised per object and per interaction, so there is no learned prior across objects; PGND and Real2Sim-Eval address parts of this.
  - The spring-mass model limits fidelity for plastic or granular materials.
  - The robot-planning claim is qualitative only in this paper.
  - The setup needs three calibrated depth cameras.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2503.17973 (v1, main text and supplementary sections read).
- README and LICENSE: raw.githubusercontent.com (Jianghanxiao/PhysTwin).
- Project page: https://jianghanxiao.github.io/phystwin-web/
- Citations and DOI: Semantic Scholar batch API.
- **Not verified:**
  - Presentation type at ICCV.
  - The license terms of the Hugging Face data.
