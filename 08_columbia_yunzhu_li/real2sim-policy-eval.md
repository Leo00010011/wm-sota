---
citation_count: 45
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICRA 2026"
date: 2025-11-06
research_group: "Columbia (Yunzhu Li)"
---

# Real-to-Sim Robot Policy Evaluation with Gaussian Splatting Simulation of Soft-Body Interactions

- **Authors:** Kaifeng Zhang\*, Shuo Sha\*, Hanxiao Jiang, Matthew Loper, Hyunjong Song, Guangyan Cai, Zhuo Xu, Xiaochen Hu, Changxi Zheng, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author. The affiliations are Columbia and SceniX Inc.; Zhuo Xu is at Google DeepMind.
- **Date published:** 2026-06 (ICRA 2026, Vienna, held in June 2026; exact dates not verified). arXiv v1 2025-11-06.
- **Venue:** IEEE International Conference on Robotics and Automation (ICRA) 2026, IEEE DOI 10.1109/ICRA57385.2026.11696757 (from Semantic Scholar). The project page and PI's homepage also say ICRA 2026.
- **Links:** arXiv https://arxiv.org/abs/2511.04665 · IEEE https://doi.org/10.1109/ICRA57385.2026.11696757 · Project https://real2sim-eval.github.io/ · Code https://github.com/kywind/real2sim-eval · Data and checkpoints https://huggingface.co/collections/shashuo0104/real-to-sim-policy-eval
- **Number of citations:** 44 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper builds a physics-structured, action-conditioned simulator, which works as a world model for policy evaluation. Dynamics come from PhysTwin spring-mass digital twins identified from human-interaction videos and extended to robot–object contact: friction-based grasping with a force-threshold gripper stop, running in NVIDIA Warp. Observations come from 3D Gaussian Splatting phone scans of the scene, with:
- ICP and RANSAC alignment of the robot to its URDF and per-link kernel assignment.
- A quadratic colour-space mapping to the RealSense cameras, fitted by IRLS.
- LBS deformation.

The simulator is wrapped as a Gym environment. Imitation policies (ACT, Diffusion Policy, π0, SmolVLA) trained only on real data are rolled out in it, and their simulated success rates correlate strongly with real-world success.

## 1. Weights availability
- Released on Hugging Face (README "Assets and Checkpoints"):
  - Gaussian Splatting scans: https://huggingface.co/shashuo0104/gs-scans.
  - PhysTwin checkpoints: e.g. https://huggingface.co/shashuo0104/phystwin-rope and https://huggingface.co/shashuo0104/phystwin-toy, plus a T-block model.
  - Policy checkpoints (ACT, π0, etc.) through the `policy_training` submodule README (https://github.com/shuosha/policy_training).
- License: the repo is MIT (copyright 2025 Kaifeng Zhang). Hugging Face asset licenses were not checked.

## 2. Code
- https://github.com/kywind/real2sim-eval, MIT license. Contains:
  - PhysTwin deployment.
  - The GS simulator.
  - Policy training (submodule) and policy evaluation (`experiments/eval_policy.py`, with multi-GPU distribution).
- Uses uv with CUDA 12.8 and PyTorch 2.7.1. Last-commit date not verified.

## 3. Datasets
- **Demonstrations:** 39–60 successful GELLO teleoperation demonstrations per task on an xArm 7 with a wrist D405 and a fixed D455 (Sec. IV-A).
- **Scenes:** Scaniverse iPhone GS scans of the workspace and of each object.
- **PhysTwin identification:** one human-interaction video per object (plush toy, rope, T-block).
- **Evaluation:** fixed grids of initial configurations, with 20 (toy packing), 27 (rope routing) and 16 (push-T) configurations per checkpoint.
- Released assets are on Hugging Face (above). The demonstration data release was not verified.

## 4. Evaluation and downstream tasks
- **Sim–real correlation of success rates** across many policy checkpoints (Fig. 3): Pearson r = 0.944 (toy packing), 0.901 (rope routing) and 0.915 (push-T).
- **Table I: MMRV (lower is better) and r (higher is better):**

  | Variant | Toy MMRV | Toy r | Rope MMRV | Rope r | Push-T MMRV | Push-T r |
  |---|---|---|---|---|---|---|
  | Full model | 0.076 | 0.944 | 0.174 | 0.901 | 0.108 | 0.915 |
  | Without physics optimisation | 0.241 | 0.694 | 0.248 | 0.832 | 0.100 | 0.905 |
  | Without colour alignment | 0.200 | 0.805 | 0.343 | 0.714 | 0.354 | 0.529 |
  | IsaacLab | — | — | 0.270 | 0.237 | 0.196 | 0.649 |

  - IsaacLab could not stably simulate the plush toy.
  - Column assignment is my reconstruction of the extracted table. It is consistent with the text's IsaacLab values (r = 0.649 push-T, 0.237 rope).
- **Training-curve tracking (Fig. 4):** simulated success follows real success across checkpoints. For example, toy packing with DP peaks at iteration 5,000 in both.
- **Speed:** 5–30 FPS on one GPU; with multi-GPU, evaluation runs "several times faster than real-world execution".
- **Downstream tasks:** policy evaluation, checkpoint selection, and performance-trend prediction for visuomotor imitation policies.

## 5. Architecture and techniques
- **Dynamics:** a PhysTwin dense spring-mass system:
  - Spring threshold d and per-spring stiffness are optimised from video by CMA-ES then gradients.
  - Rigid objects use a large stiffness.
  - A custom Warp engine adds robot–object and object–environment collisions and friction-only grasping.
- **Rendering:** 3DGS. Rigid kernels follow link or object poses, and deformable kernels use LBS. The colour mapping is a degree-2 polynomial in RGB fitted by IRLS.
- **Interface:** Gym-style `env.step()` and `env.render()`.
- **Paradigm:** physics simulation with system identification plus neural rendering. There is no learned dynamics network.

## 6. Conclusions and future work
- **Authors:** combining physics-informed soft-body twins with GS rendering gives strong sim–real correlation (r > 0.9). Physics optimisation and colour alignment both matter.
- **Authors' future work:** scale to more tasks and policies, and generalise the real-to-sim framework to more diverse environments.
- **My own observations:**
  - This is a physics-based simulator rather than a learned world model, so it fits the policy-evaluation category but is borderline for "world model".
  - Evaluation sets are small (16–27 initial states), and there are only three tasks with one robot.
  - Correlation is reported per task over checkpoints, not across tasks.
  - Two authors are affiliated with SceniX Inc., a company.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2511.04665 (v2, full main text read).
- README and LICENSE: raw.githubusercontent.com (kywind/real2sim-eval).
- Project page: https://real2sim-eval.github.io/
- Citations and DOI: Semantic Scholar batch API.
- **Not verified:**
  - Exact ICRA 2026 dates and presentation type.
  - Hugging Face licenses.
  - The Table I column assignment (reconstructed from garbled text).
