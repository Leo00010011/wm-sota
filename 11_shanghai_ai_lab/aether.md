---
citation_count: 105
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICCV 2025"
date: 2025-03-24
research_group: "Shanghai AI Lab (Physical Intelligence Team)"
---

# Aether: Geometric-Aware Unified World Modeling

- **Authors:** Aether Team, Haoyi Zhu, Yifan Wang, Jianjun Zhou, Wenzheng Chang, Yang Zhou, Zizun Li, Junyi Chen, **Chunhua Shen**, **Jiangmiao Pang**, **Tong He** (Shanghai AI Laboratory, with USTC, SII, SJTU, Zhejiang University, Fudan University)
- **Group role:** Shanghai AI Lab is the senior affiliation. Tong He (last and corresponding author) then led Shanghai AI Lab's World Model team; Pang is co-corresponding. Chunhua Shen is a contributor to the Physical Intelligence Team's 2026 reports. Zizun Li and Jianjun Zhou later appear on InternW0-Δ (2609.31394), so this team's people moved into the Physical Intelligence Team. The paper predates the team name.
- **Date published:** 2025-10 (ICCV 2025, October 2025; arXiv v1 2025-03-24, v3 2025-07-28)
- **Venue:** ICCV 2025 (Proceedings of the IEEE/CVF ICCV, pp. 8535-8546). It also won the Outstanding Paper Award at the ICCV 2025 RIWM workshop (per the repo README).
- **Links:** arXiv https://arxiv.org/abs/2503.18945 · CVF https://openaccess.thecvf.com/content/ICCV2025/html/Zhu_Aether_Geometric-Aware_Unified_World_Modeling_ICCV_2025_paper.html · Project https://aether-world.github.io · Code https://github.com/InternRobotics/Aether (formerly OpenRobotLab/Aether) · Weights https://huggingface.co/AetherWorldModel/AetherV1
- **Number of citations:** 105 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
Aether fine-tunes the CogVideoX-5b-I2V video diffusion model to jointly generate RGB video, depth (disparity) and camera raymaps. One model then does three things: 4D dynamic reconstruction, action-conditioned video prediction (with camera trajectories as the action space), and goal-conditioned visual planning. It is trained only on synthetic game data and transfers zero-shot to real video.

## 1. Weights availability
- Released: Hugging Face `AetherWorldModel/AetherV1` (diffusers / CogVideoX format, safetensors, MIT license, not gated; last modified 2025-03-30).
- Loading: `python scripts/demo.py --task reconstruction|prediction|planning ...` downloads the checkpoint. A Gradio demo is included (`scripts/demo_gradio.py`; tested on one A100 80GB) and is hosted at https://huggingface.co/spaces/AmberHeart/AetherV1.

## 2. Code
- https://github.com/InternRobotics/Aether, MIT license, 613 stars, last commit 2025-10-26 (GitHub API, checked 2026-10-15).
- Includes inference for all three tasks, a `camera_pose_to_raymap` utility for custom camera actions, and evaluation scripts for video depth and camera pose (`evaluation/`). The README does not mention training code or the data-annotation pipeline.

## 3. Datasets
- Training: synthetic RGB-D video from AAA games (Cyberpunk 2077 and Horizon 5, following DA-V and TheMatrix). About 12.5M raw frames, about 8.9M after camera annotation and filtering.
- Camera annotation pipeline: dynamic masks with Grounded SAM 2; slicing with SIFT, dynamic-area and RAFT checks; coarse calibration with DroidCalib; refinement with CoTracker3, SIFT/SuperPoint and bundle adjustment (Ceres).
- No real-world training data. The annotated training data does not appear to be released (not found in the repo or on Hugging Face).

## 4. Evaluation and downstream tasks
- Video depth (zero-shot; Abs Rel, lower is better / δ<1.25): Sintel 0.314 / 60.4 vs DepthCrafter 0.590 / 55.5 and MonST3R-GA 0.378 / 55.8. KITTI 0.054 / 97.7 vs DepthCrafter 0.124 / 86.5; CUT3R 0.118 / 88.1. BONN 0.308 / 60.2 (weaker on indoor scenes).
- Camera pose (ATE / RPE-trans / RPE-rot): Sintel 0.189 / 0.054 / 0.694, the best ATE and RPE-trans among feed-forward methods. Weaker than CUT3R on TUM-dynamics ATE (0.092 vs 0.046) and ScanNet ATE (0.176 vs 0.099).
- Video prediction (VBench weighted average): action-free 80.04 vs CogVideoX 78.51. Action-conditioned 80.71 vs CogVideoX 79.92 (CogVideoX gets GPT-4o motion text as a proxy for the action).
- Action-conditioned navigation (with vs without the depth branch): PSNR 19.70 vs 18.97, SSIM 0.5545 vs 0.5353, LPIPS 0.2659 vs 0.3074.
- Visual path planning (VBench): 80.67 vs 79.59 without depth.
- No closed-loop robot control or RL experiment.

## 5. Architecture and techniques
- Generative paradigm: latent video diffusion (DiT), initialized from CogVideoX-5b-I2V. Colour, depth and raymap latents are concatenated along channels, with zero-initialized projection layers for depth and raymaps.
- Depth: clipped, converted to square-root disparity, normalized per clip, replicated to 3 channels and encoded with the video VAE.
- Actions: camera extrinsics and intrinsics as a 6-channel raymap (ray directions and origins; signed log transform on translation), downsampled 8x, with 4 frames stacked per latent step.
- The task is chosen by which modalities are given as conditions (reconstruction: full video; prediction: first frame plus optional raymap; planning: first and goal frames). Text conditioning is replaced by an empty embedding.
- Reconstruction uses 4 denoising steps, with a Kalman filter to smooth camera trajectories.
- Compute: 80 A100-80GB GPUs for about two weeks, effective batch 320, two stages (stage 2 about 1/4 of the stage-1 steps). Clip lengths are 17-41 frames.

## 6. Conclusions and future work
- Authors: geometry-aware joint modelling lets a video generator reconstruct, predict and plan, and synthetic-only training transfers to real data zero-shot. Stated limitations: camera pose is less accurate (raymaps fit poorly with the video prior), indoor reconstruction lags outdoor, and it fails in highly dynamic scenes without language prompts. Future work: new action representations, co-training with real data, keeping language prompting.
- Own observation: "actions" are camera motions only. There is no agent or robot action space, so it is a navigation/camera world model, not an embodied control model. The VBench gains over CogVideoX are small (about 1-1.5 points), and planning is evaluated against an ablation, not against other planners.

## 7. Sources and verification
- https://arxiv.org/html/2503.18945v3 (full paper, read through a summarizing fetch)
- CVF open-access page (venue, page numbers); ICCV 2025 listing checked directly
- GitHub README and API; Hugging Face model API; Tong He's homepage (https://tonghe90.github.io/) for corresponding-author and team role
- Not verified: total training steps (not stated in the paper); whether training code was released later in another repo.
