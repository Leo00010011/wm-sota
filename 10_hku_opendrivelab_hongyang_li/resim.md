---
citation_count: 54
citation_source: "Semantic Scholar, 2026-10-15 (value from existing note; live re-check rate-limited)"
venue: "NeurIPS 2025 (Spotlight)"
date: 2025-06-11
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# ReSim: Reliable World Simulation for Autonomous Driving

- **Authors:** Jiazhi Yang, Kashyap Chitta, Shenyuan Gao, Long Chen, Yuqian Shao, Xiaosong Jia, **Hongyang Li**, Andreas Geiger, Xiangyu Yue†, Li Chen†
  - †"Corresponding authors with equal advising": Xiangyu Yue (CUHK) and Li Chen (HKU / OpenDriveLab).
  - Primary contact: Jiazhi Yang.
- **Group role:** Hongyang Li is a co-author (7th of 10). The senior authors, Li Chen and Xiangyu Yue, are not PIs in the summary file. The paper is kept here as an OpenDriveLab project (opendrivelab.com/ReSim, github.com/OpenDriveLab/ReSim), following the 07_stanford_wu precedent for co-authored papers without a summary-PI senior author.
- **Date published:** 2025-12 (NeurIPS 2025). arXiv v1 2025-06-11, v2 2026-04-28.
- **Venue:** NeurIPS 2025, Spotlight (arXiv comment and README).
- **Links:**
  - arXiv: https://arxiv.org/abs/2506.09981
  - Project: https://opendrivelab.com/ReSim
  - Code: https://github.com/OpenDriveLab/ReSim
- **Number of citations:** 54 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
ReSim is a front-camera driving world model meant to simulate hazardous or non-expert ego behavior as well as expert driving. Such behavior is nearly absent from real driving logs, which limits earlier models such as Vista for policy evaluation. ReSim addresses this in three ways:

- **Training data:** it adds CARLA data from two agents, PDM-Lite expert runs and random-steering/speed exploration, to OpenDV web video and NAVSIM real logs.
- **Model:** it adapts CogVideoX into a trajectory-conditioned video world model.
- **Reward:** a Video2Reward head on frozen DINOv2 is trained to predict the CARLA infraction score from video. It scores the futures that ReSim simulates for candidate trajectories, which enables reward-guided policy selection.

## 1. Weights availability
- **Released 2026-05-05:** pretrained world-model weights for the OpenDV + NAVSIM variant (expert actions only, no CARLA data).
- **Not yet released:** the full OpenDV + NAVSIM + CARLA model (expert and non-expert), which is still a TODO in the README. The paper's headline non-expert results therefore cannot yet be reproduced from released weights.

## 2. Code
- https://github.com/OpenDriveLab/ReSim, Apache-2.0, about 192 stars.
- Contents: training and inference scripts built on SAT (SwissArmyTransformer) and CogVideoX checkpoints, using Python 3.10, PyTorch 2.4 and DeepSpeed-style distributed training.

## 3. Datasets
Each sample is 4.9 s at 10 Hz: 9 context frames and 40 target frames.

| Source | Samples | Content |
|---|---|---|
| OpenDV-YouTube | 4M clips | About 1,700 h of web driving video; no actions |
| NAVSIM navtrain | 85K | Real logs with ego trajectories |
| CARLA (Bench2Drive routes) | 88K | Expert (PDM-Lite) and random non-expert behavior |

- **Video2Reward:** trained on 35K CARLA samples.
- **Evaluation:**
  - nuScenes validation (zero-shot) for fidelity;
  - Waymo validation (540 samples, zero-shot) for controllability;
  - NAVSIM navtest for planning and policy selection;
  - CARLA and NAVSIM pairs for reward correlation.

## 4. Evaluation and downstream tasks
**Fidelity on nuScenes validation without action condition (Table 2):**

| Model | Zero-shot | FID ↓ | FVD ↓ |
|---|---|---|---|
| DriveGAN | no | 27.8 | 390.8 |
| DriveDreamer | no | 14.9 | 340.8 |
| DriveDreamer-2 | no | 25.0 | 105.1 |
| WoVoGen | no | 27.6 | 417.7 |
| Drive-WM | no | 15.8 | 122.7 |
| GenAD | no | 15.4 | 184.0 |
| GEM | no | 10.5 | 158.5 |
| Vista | no | 6.9 | 89.4 |
| **ReSim** | **yes** | **5.2** | **50.4** |

The abstract's "up to 44% higher visual fidelity" is the FVD reduction against Vista (89.4 → 50.4). FID improves about 25%. ReSim never saw nuScenes in training.

**Controllability on Waymo (zero-shot, Table 1):** trajectory difference measured by an inverse dynamics model.

| Model | Action-free ↓ | Expert action ↓ |
|---|---|---|
| Ground-truth future | 0.58 | 0.58 |
| Vista | 5.68 | 1.89 |
| ReSim without simulated data | 1.47 | 1.18 |
| ReSim | 1.13 | 0.86 |

- The errors are 80% and 54% lower than Vista's; this is the abstract's ">50%" controllability claim.
- **Non-expert actions:** judged by human preference only (Fig. 4). ReSim was preferred in about 54% of votes for visual realism and 57% for trajectory following, against Vista and the variant without simulated data. These percentages were read from the figure.

**Planning on NAVSIM navtest (Table 4):** ReSim plus an inverse dynamics model, using front-view video only, with no ego status and no extra annotations.

| Method | PDMS ↑ |
|---|---|
| Visual-odometry planner | 78.4 |
| DrivingGPT | 82.4 |
| UniAD | 83.4 |
| Transfuser | 84.0 |
| LAW | 84.6 |
| **ReSim + inverse dynamics model** | **86.6** |
| Ground-truth future + inverse dynamics model (oracle) | 90.8 |

The abstract's "+2% planning" is +2.0 PDMS over LAW.

**Reward-guided policy selection (Table 3, NAVSIM subset):** candidates are Transfuser (47.7) and LTF (47.2). Selection results:

| Selection method | PDMS ↑ |
|---|---|
| Average ensemble | 66.8 |
| Vista-based selection | 59.2 |
| ReSim without simulated data | 69.7 |
| **ReSim + Video2Reward** | **74.1** |
| Oracle | 94.2 |

The abstract says "+25%" and the table caption "+26%": both are absolute PDMS gains over the candidate policies.

**Reward correlation (Fig. 7):** Video2Reward distinguishes expert from non-expert trajectories better than the alternatives on both CARLA and NAVSIM, using 250 comparative pairs.

**Ablations:**
- Unbalanced noise sampling and the dynamics consistency loss: qualitative only (Figs. 9–10).
- Removing the CARLA data hurts controllability, planning and selection (Tables 1, 3 and 4).

## 5. Architecture and techniques
- **Backbone:** CogVideoX, a 2B DiT with a T5 text encoder and a 3D causal VAE.
  - History frames are injected by replacing their noisy latents with clean ones (the Vista approach).
  - A text command plus future ego waypoints (4 s at 2 Hz) pass through a lightweight trajectory encoder (2 attention blocks and a linear head) into the DiT input.
  - Following DiT practice, history latents are corrupted with conditioning-augmentation noise for robust multi-round rollouts.
- **Losses:** diffusion loss plus a dynamics consistency loss on latent motion over intervals up to K = 4, with λ = 0.1.
- **Unbalanced noise sampling:** the share of timesteps drawn from [500, 1000] rises from 1/2 to 2/3.
- **Three-stage curriculum:**
  1. OpenDV only, with text and history conditioning.
  2. Joint OpenDV + NAVSIM + CARLA at 256×448: LoRA plus the trajectory encoder, with the backbone frozen. NAVSIM trajectories are dropped with p = 0.5; CARLA trajectories are never dropped.
  3. Full fine-tuning at 512×896.

  The final model generates 4 s of 10 Hz video.
- **Compute:** all stages on 40 A100 GPUs, about 14 days in total.
- **Video2Reward:** frozen DINOv2 with a light head, input 224×224, 20 epochs.

## 6. Conclusions and future work
- **Authors:** adding non-expert simulator data makes a driving world model reliable across expert and hazardous actions, and video-based reward closes the loop to planning and policy selection. Limitations: inference cost from iterative denoising, and agents have not yet been trained inside ReSim. Future work: efficiency, RL agents inside the world model, and fair closed-loop planning benchmarks.
- **Own observation:**
  - Non-expert controllability is supported only by a human preference study.
  - The NAVSIM planning gain over LAW is small (+2 PDMS), and NAVSIM is non-reactive.
  - Policy selection is tested with only two candidate policies on a subset.
  - The CARLA-trained weights behind the headline results are not released.
  - The group's later robotics paper RISE carries the same simulator-plus-value recipe through to actual RL in imagination.

## 7. Sources and verification
- **Read:** full arXiv v2 PDF (pdftotext, plain and layout mode, main text and appendix B), GitHub README and API metadata, Semantic Scholar API.
- **Not verified:** the NeurIPS 2025 virtual page (the Spotlight status rests on the arXiv comment and README), and the exact human-study percentages, which were read from the figure.
