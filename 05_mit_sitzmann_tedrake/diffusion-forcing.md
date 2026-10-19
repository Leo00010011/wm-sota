---
citation_count: 779
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live recheck rate-limited, HTTP 429)"
venue: "NeurIPS 2024"
date: 2024-07-01
research_group: "MIT (Vincent Sitzmann, Russ Tedrake)"
---

# Diffusion Forcing: Next-token Prediction Meets Full-Sequence Diffusion

- **Authors:** Boyuan Chen, Diego Martí Monsó, Yilun Du, Max Simchowitz, **Russ Tedrake**, **Vincent Sitzmann**
- **Group role:** Senior author (Sitzmann last author; Tedrake co-author); first author Boyuan Chen is a student in the group (MIT CSAIL)
- **Date published:** 2024-12 (NeurIPS 2024 conference, Vancouver, December 2024); arXiv v1 2024-07-01
- **Venue:** NeurIPS 2024 (Advances in Neural Information Processing Systems 37), Main Conference Track, poster
- **Links:** arXiv https://arxiv.org/abs/2407.01392 · Proceedings https://proceedings.neurips.cc/paper_files/paper/2024/hash/2aee1c4159e48407d68fe16ae8e6e49e-Abstract.html · NeurIPS poster page https://neurips.cc/virtual/2024/poster/93029 · Project https://boyuan.space/diffusion-forcing · Code https://github.com/buoyancy99/diffusion-forcing
- **Number of citations:** 779 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Diffusion Forcing (DF) trains a diffusion model to denoise a sequence of tokens where every token has its own, independent noise level ("noise as partial masking"). Implemented causally (Causal Diffusion Forcing, an RNN latent-state model), it acts as a sequence world model that rolls out video past the training horizon without diverging, and, when tokens are `[action, reward, next observation]`, works as both a policy and a model-based planner with long-horizon reward guidance (Monte Carlo Guidance).

## 1. Weights availability
- Released, via the code repository: a Google Drive archive `quickstart_atten.tar.gz` linked from the README (bundles a mini dataset plus checkpoints `outputs/minecraft.ckpt`, `outputs/maze2d_medium_x.ckpt`, `outputs/maze2d_large_x.ckpt`).
- Load: `python -m main +name=sample_minecraft_pretrained load=outputs/minecraft.ckpt experiment.tasks=[validation]` (from README).
- Caveat: these checkpoints are for the repo's `main` branch, a re-implementation with temporal attention instead of the paper's RNN. I found no released checkpoint of the original RNN models (`paper` branch).
- License: MIT (repo LICENSE, "Copyright (c) 2024 Boyuan Chen").

## 2. Code
- https://github.com/buoyancy99/diffusion-forcing, MIT license, about 1.3k stars, last push 2026-07-06 (GitHub API, checked 2026-10-13).
- Branches: `main` (transformer/temporal-attention re-implementation, recommended) and `paper` (original RNN code to reproduce the paper).
- Contains training and evaluation/sampling code for Minecraft and DMLab video and for D4RL Maze2D planning (needs MuJoCo 210 and d4rl extras). The README points to the transformer follow-up (DFoT, `kwsong0113/diffusion-forcing-transformer`) for video. I did not find the real-robot or time-series pipelines described in the README.

## 3. Datasets
- **Minecraft navigation** (from TECO, Yan et al. 2023): first-person random walks, 128×128, 72-frame subsequences; actions discarded; only about 10% of subsequences used for training (App. F.1). Public, downloaded automatically from TECO.
- **DMLab navigation** (TECO): 3D maze random walks, 64×64, 48-frame subsequences; about 10% used (App. F.1). Public.
- **D4RL Maze2D** (U-Maze, Medium, Large) plus a multi-goal variant; offline random-walk datasets (App. F.2). Public.
- **Real robot fruit swapping:** 150 VR-teleoperated Franka demonstrations, 500 to 600 frames each, two cameras, 6-DoF actions (App. F.3). Collected by the authors; no public release found.
- **Time series:** GluonTS Exchange, Solar, Electricity, Traffic, Taxi, Wikipedia (App. E.1, Table 3). Public.

## 4. Evaluation and downstream tasks
- **Video prediction (Minecraft, DMLab):** qualitative only (Fig. 3, App. E.3). DF rolls out stably to 1000 frames, and the authors report 2000 frames without blow-up; teacher-forcing and causal full-sequence diffusion baselines with the same RNN diverge. No FVD reported.
- **Model-based planning, D4RL Maze2D (Table 1, episode return):** DF 116.7 / 149.4 / 159.0 (U-Maze / Medium / Large), single-task average 141.7, vs Diffuser* 119.5 (with hand-crafted PD controller), Diffuser with its own actions 8.7, IQL 47.0, CQL 7.7, MPPI 16.2. Multi-goal average 146.2 vs Diffuser* 129.4. Without Monte Carlo Guidance: 129.67 single-task.
- **Compositional generation:** a 2D cross-shaped trajectory dataset; with memory DF reproduces the cross, while memoryless MPC-style sampling stitches sub-trajectories into V-shapes (Sec. 4.3, App. E.2). Qualitative.
- **Real-robot imitation learning with memory (Sec. 4.4):** 80% success on fruit swapping, where Diffusion Policy (no memory) fails. With visual distractors or camera occlusion handled as noisy tokens: 76% vs 48% for a next-frame diffusion baseline. The same model also generates the robot video from one frame.
- **Time-series forecasting (Table 2, CRPS-sum, lower is better):** Exchange 0.003, Solar 0.289, Electricity 0.023, Traffic 0.040, Taxi 0.075, Wikipedia 0.085. Competitive with TimeGrad and ScoreGrad (best on Exchange, Traffic and Taxi; 4th on Wikipedia).

## 5. Architecture and techniques
- **Paradigm:** diffusion with per-token independent noise levels. Training loss is standard noise-prediction MSE (Eq. 3.1). Theorem 3.1: it optimizes a reweighted ELBO on the likelihood of all subsequences / noise-level sequences.
- **Backbone (paper):** a convolutional RNN. A diffusion U-Net encodes the noisy frame, a GRU updates a deterministic latent z_t, and a 1-layer ResNet plus conv observation model decodes (App. D). Non-image data uses residual MLPs plus a GRU. Frame stacking is used.
- **Size:** Minecraft 36M, DMLab 24M, maze planner 4.33M parameters. Video models trained on 8×A100, about 12 h to converge (App. D).
- **Sampling:** a 2D scheduling matrix of noise levels over (denoising step × time). Schedules include autoregressive with slight noise on context for stabilization, and "zig-zag" (pyramid) schedules that keep the far future more uncertain.
- **Decision making:** tokens `[a_t, r_t, o_{t+1}]`; receding-horizon planning with reward or goal guidance; Monte Carlo Guidance averages guidance gradients over several sampled futures. Flexible horizon without retraining.
- **Action conditioning:** actions are generated jointly as tokens in planning/IL. In the video experiments actions are discarded, so those are unconditional rollouts.
- A transformer variant is discussed in App. B.1 (it became the DFoT follow-up).

## 6. Conclusions and future work
- **Authors:** DF combines the strengths of next-token prediction (variable length, causality, stable long rollouts) with those of full-sequence diffusion (guidance), and gives large gains in sequential decision making. Stated limitations: the causal implementation is an RNN, so higher-resolution video or more complex distributions need large transformers. Scaling to internet-scale data was not studied. Future work: other domains and larger datasets.
- **Own observations:**
  - Video results are qualitative only, at low resolution (64 to 128 px) and on about 10% of the data, with no FVD or comparison to published video models.
  - Planning is evaluated only on 2D Maze2D.
  - The real-robot result rests on one task with 150 demonstrations.
  - The released code and weights are a transformer re-implementation, not the exact RNN models in the paper.

## 7. Sources and verification
- Paper PDF (arXiv v4): https://arxiv.org/pdf/2407.01392 (read Sec. 1 to 5, App. D, E, F)
- Proceedings: https://proceedings.neurips.cc/paper_files/paper/2024/hash/2aee1c4159e48407d68fe16ae8e6e49e-Abstract.html ; poster page https://neurips.cc/virtual/2024/poster/93029
- Code and README: https://github.com/buoyancy99/diffusion-forcing ; LICENSE https://raw.githubusercontent.com/buoyancy99/diffusion-forcing/main/LICENSE
- Citations: https://api.semanticscholar.org/graph/v1/paper/arXiv:2407.01392
- Not verified:
  - The exact Google Drive URL of the checkpoint archive and its contents (I read the README description, not the archive).
  - Whether the venue track was a spotlight. Only "poster" appears on the virtual page, and I found no spotlight label.
