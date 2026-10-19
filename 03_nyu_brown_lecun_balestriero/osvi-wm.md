---
citation_count: 12
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check on 2026-10-15 returned HTTP 429)"
venue: "NeurIPS 2025"
date: 2025-05-26
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# OSVI-WM: One-Shot Visual Imitation for Unseen Tasks using World-Model-Guided Trajectory Generation

- **Authors:** Raktim Gautam Goswami, Prashanth Krishnamurthy, **Yann LeCun**, Farshad Khorrami
- **Group role:** Co-author (LeCun, third author). Last author is Farshad Khorrami (NYU Tandon), who is not a PI of another group in the summary, so the paper stays in this folder. Affiliations: NYU Tandon, NYU Courant, Meta FAIR.
- **Date published:** 2025-12 (NeurIPS 2025 conference, December 2025). arXiv v1: 2025-05-26; v2: 2025-12-30.
- **Venue:** NeurIPS 2025, poster (OpenReview venue "NeurIPS 2025 poster"; Semantic Scholar venue "Neural Information Processing Systems").
- **Links:** arXiv https://arxiv.org/abs/2505.20425 · OpenReview https://openreview.net/forum?id=eXO6g7BmOA · Code https://github.com/raktimgg/osvi-wm
- **Number of citations:** 12 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
OSVI-WM does one-shot visual imitation of unseen manipulation tasks: from one expert demonstration video and the robot's first observation, a latent action model and a latent world model (causal transformers over ResNet-18 feature maps) recursively roll out future latent states. The predicted latent trajectory is pooled and decoded into five end-effector waypoints (3D position + gripper), executed by an IK waypoint controller with optional re-planning.

## 1. Weights availability
- Released. README: "The pre-trained checkpoints for Meta-World and Pick-and-Place can be downloaded from [drive link]": https://drive.google.com/drive/folders/1ekflx2fGqf8gVPISV1Jckc77SbhsXCRc
- No checkpoints listed for the real-robot setups. No license stated for the checkpoints.

## 2. Code
- https://github.com/raktimgg/osvi-wm, no license file detected by the GitHub API (license field empty), last push 2026-09-05.
- Contains training (`train_metaworld.py`, `train_pp.py` with `accelerate`) and evaluation scripts that plug into the AWDA evaluation framework (https://github.com/MatthewChang/osvi-awda), with configs `metaworld_eval.yaml` and `pick_place_eval.yaml`.

## 3. Datasets
- Meta-World (simulated, public): 50 tasks, 46 train / 4 test (easy: Button-Press-V2, Pick-Place-Wall-V2; hard: Window-Open-V2, Door-Unlock-V2). Generated with the AWDA scripts.
- Pick-and-Place (simulated, from T-OSVI): 16 tasks (4 objects × 4 targets), 14 train / 2 test; expert is a Sawyer arm, agent a Franka.
- Real-world, collected by the authors (not stated as released): Two-Franka-PP, Human-Franka-PP, Human-Franka-Push (4 tasks each, 3 train / 1 test).
- Expert demos downsampled to 10 frames, agent trajectories to 6 frames; every expert demo paired with every agent trajectory of the same task.

## 4. Evaluation and downstream tasks
- Metric: task success rate (100 rollouts per task in simulation; 25 fixed configurations per real setup), plus reach/grasp breakdowns.
- Simulation (Table 1): Pick-and-Place 100% (AWDA 100, T-OSVI 10, DAML 1); Meta-World easy 96, hard 71.5, all 83.8 vs AWDA 74 / 11 / 42.5 (AWDA with extra data 73 / 30 / 51.5).
- Real world (Table 2): Two-Franka-PP 96% (AWDA 88, T-OSVI 0); Human-Franka-PP 92% (AWDA 92); Human-Franka-Push 100% (AWDA 76, T-OSVI 16).
- Ablations: removing the world model gives the lowest Meta-World score, keeping it without L_wm gives limited gains (Fig. 7); re-planning adds about 30 points on hard tasks (Fig. 8); no stop-gradient → Pick-Place 20, Meta-World 76.5; spatial pooling before the WM → 15 / 79.5; single-step instead of multi-step prediction → 100 / 67.3 (Table 3).
- Task-split ablation: Window-Open drops 95% → 80% when Window-Close is removed from training.

## 5. Architecture and techniques
- Encoder: ResNet-18 (shared for expert and agent frames), 512×H×W feature maps before global pooling, trained end-to-end in-domain, no pretraining.
- Action model π: causal transformer over concatenated expert and agent latent states, outputs a latent action. World model W: causal transformer over (agent state, latent action) pairs, predicts the next latent state; applied recursively for M−1 steps.
- Decoding: spatial soft-argmax pooling, sinusoidal temporal embeddings, attention pooling, then a 2-layer MLP for 5 attributed waypoints (x, y, z, gripper); IK controller plus wrist depth camera correction for grasping.
- Losses: soft-DTW on waypoints + α(τ)·L1 latent loss between predicted and stop-gradient encoded future states (L_wm). AdamW, one-cycle LR, batch 128, asymmetric demo image mixup.
- Hardware: single RTX A4000 (sim) / RTX 2080 Ti (real). Parameter count not stated.

## 6. Conclusions and future work
- Authors: world-model-guided latent trajectory generation improves one-shot imitation on unseen tasks in simulation and on real robots. Stated limitations: predicts only 3D position and gripper state (no end-effector orientation, so no screwing or side-grasping); single-task execution only; performance depends on the train/test task split; test tasks come from the same manipulator family as training.
- Own observations: the "world model" is an auxiliary latent predictor inside a behavior-cloning pipeline, not a model used for search or planning; its effect is shown only through ablations. Real-robot test sets are small (one held-out task, 25 trials per setup), and baseline numbers in simulation are copied from AWDA.

## 7. Sources and verification
- Paper: arXiv PDF v2 (Sections 3–5, Tables 1–3, Figs. 7–8), https://arxiv.org/pdf/2505.20425
- Venue: OpenReview search (venue "NeurIPS 2025 poster", forum eXO6g7BmOA); Semantic Scholar venue field.
- Code: GitHub API (README, license field, push date).
- Citations: Semantic Scholar API (arXiv:2505.20425).
- Not verified: the NeurIPS proceedings page URL (not looked up); release status of the real-robot data; license of code and checkpoints (none declared).
