---
citation_count: 41
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live re-check rate-limited on 2026-10-15)"
venue: "RSS 2026"
date: 2026-03-09
research_group: "Columbia (Yunzhu Li)"
---

# Interactive World Simulator for Robot Policy Training and Evaluation

- **Authors:** Yixuan Wang, Rhythm Syed, Fangyu Wu, Mengchao Zhang, Aykut Onol, Jose Barreiros, Hooshang Nayyeri, Tony Dear, Huan Zhang, **Yunzhu Li**
- **Group role:** Senior (last) author. The affiliations are Columbia, Toyota Research Institute, Amazon and UIUC.
- **Date published:** 2026-07 (RSS 2026, Sydney, 13 to 17 July 2026). arXiv v1 2026-03-09.
- **Venue:** Robotics: Science and Systems (RSS) XXII, 2026, paper 18 in the "World Models & Memory" session (https://roboticsconference.org/program/papers/18/). Proceedings: https://roboticsproceedings.org/rss22/p018.html. The PI's homepage also notes a Best Student Paper Award at the RSS 2026 Workshop on Robot World Models (workshop award, not the main conference).
- **Links:** arXiv https://arxiv.org/abs/2603.08546 · Proceedings https://roboticsproceedings.org/rss22/p018.pdf · Project https://yixuanwang.me/interactive_world_sim · Code https://github.com/WangYixuan12/interactive_world_sim · Checkpoints https://huggingface.co/yixuan1999/interactive-world-sim-checkpoints
- **Number of citations:** 41 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The Interactive World Simulator is an action-conditioned latent video world model trained on about 600 play episodes per task from a bimanual ALOHA robot. It has two stages:
1. A CNN encoder maps 128×128 RGB frames to 2D latents, and a consistency-model decoder reconstructs them.
2. An action-conditioned consistency model, built from 3D-convolution blocks with FiLM and spatiotemporal attention, predicts the next latent frame from a sliding context window. It is trained with noise injected into the context for robustness.

It runs interactively at 15 FPS on one RTX 4090 and stays stable for more than 10 minutes. It is used to:
- Teleoperate demonstrations entirely inside the model, for imitation learning.
- Evaluate policies, with success that correlates with the real world.

## 1. Weights availability
- Released. `bash scripts/download_checkpoints.sh` downloads 7 checkpoints (each with `checkpoints/best.ckpt` and `.hydra/config.yaml`) via gdown. They are mirrored on Hugging Face: https://huggingface.co/yixuan1999/interactive-world-sim-checkpoints.
- Data:
  - A mini dataset of a few episodes per task: https://huggingface.co/datasets/yixuan1999/interactive-world-sim-min-data.
  - A full ALOHA training dataset via `scripts/download_full_data.sh`.
- License: the repository LICENSE is MIT, with copyright "2024 Boyuan Chen". This suggests the codebase derives from Boyuan Chen's (MIT) Diffusion Forcing/DFoT code template; I infer that only from the copyright line. HF licenses were not checked.

## 2. Code
- https://github.com/WangYixuan12/interactive_world_sim, MIT license. Contains:
  - Stage 1 autoencoder training and Stage 2 dynamics training.
  - Real ALOHA data collection, MuJoCo simulated data collection, and in-world-model data collection on ALOHA.
- Uses uv with CUDA 12.6 and Weights & Biases logging. Last-commit date not verified.

## 3. Datasets
- **Simulation:** MuJoCo T-pushing, 10,000 scripted random-interaction episodes.
- **Real:** about 600 play episodes × 200 steps for each of six tasks: mug grasping, rope routing, rope collecting, T pushing, box packing and pile sweeping. Each task takes about 6 hours of collection by one person (Sec. IV-A).
- Resolution is 128×128. Action spaces are partly constrained, e.g. planar motion for T pushing.
- Public: mini and full datasets (see above).

## 4. Evaluation and downstream tasks
- **Long-horizon video prediction** over 192 steps (19.2 s), averaged over all 7 tasks (Table I):

  | Model | MSE ↓ | LPIPS ↓ | FID ↓ | PSNR ↑ | SSIM ↑ | UIQI ↑ | FVD ↓ |
  |---|---|---|---|---|---|---|---|
  | **This model** | **0.005** | **0.051** | **63.5** | **25.82** | **0.831** | **0.960** | **243.2** |
  | Dreamer4 | 0.012 | 0.163 | 240.0 | 20.81 | 0.693 | 0.919 | 1747.3 |
  | Cosmos (fine-tuned) | 0.019 | 0.224 | 200.7 | 18.91 | 0.647 | 0.883 | 799.3 |
  | UVA | 0.023 | 0.272 | 142.6 | 17.87 | 0.650 | 0.884 | 2213.3 |
  | DINO-WM | 0.028 | 0.270 | 200.8 | 17.79 | 0.652 | 0.875 | 1752.6 |

- **Policy training on world-model data** (100 episodes per mixture; Fig. 5):
  - Policies: DP, ACT, π0, π0.5. Tasks: real T pushing, pile sweeping, mug grasping, rope routing, and sim T pushing.
  - Average task score with 100% world-model data vs 100% real data:
    - DP: 87.9% vs 90.3%.
    - ACT: 76.2% vs 73.6%.
    - π0.5: rises from 73.1% to 88.8% as the real-data share grows.
- **Data scaling (Fig. 6):** ACT and DP trained on 5–100 world-model episodes improve at the same rate as when trained on MuJoCo data (MuJoCo T-pushing, 100 evaluation trials).
- **Policy-evaluation correlation (Fig. 7):** 20 shared initial configurations per task, with checkpoints of DP, ACT, π0 and π0.5. Pearson r values of 0.9908, 0.8869, 0.8553 (T pushing) and 0.8455 (rope routing) are reported. I could not reliably attribute 0.9908 and 0.8869 between mug grasping and pile sweeping from the extracted figure text. The simulator is slightly optimistic on all tasks except T pushing.
- **Speed and size:** 15 FPS on an RTX 4090, stable for more than 10 minutes. The mug-grasping model is 176 MB. Training takes about 6 h (Stage 1) plus 12 h (Stage 2) on one H200.
- **Downstream tasks:** action-conditioned video prediction, in-model teleoperated data generation for imitation learning, and policy evaluation and checkpoint selection.

## 5. Architecture and techniques
- **Autoencoder:** a CNN encoder to a C×H′×W′ latent. The decoder is a consistency model trained in the style of a Consistency Trajectory Model: it maps a higher-noise to a lower-noise image conditioned on z (Eqs. 1–3).
- **Dynamics:** an action-conditioned consistency model F_ψ:
  - Full noise is applied only to the last latent frame, and a weighted regression loss is used (Eqs. 4–5).
  - Architecture: 3D convolutions, FiLM action and time embeddings, and spatial plus temporal attention.
  - Small noise is injected into context frames for long-horizon robustness.
- **Inference:** an autoregressive sliding window, with few denoising steps.
- **Paradigm:** latent consistency model (diffusion family) world model, trained per task. Total parameter count not stated; checkpoint size about 176 MB.

## 6. Conclusions and future work
- **Authors:** the simulator gives long-horizon, stable, physically consistent interaction at interactive speed. World-model data is as good as real data for imitation learning, and simulated policy evaluation correlates strongly with the real world.
- **Authors' future work:** more diverse environments and harder tasks, and a study of how world-model performance scales with data and compute.
- **My own observations:**
  - There is one model per task at 128×128 resolution, with constrained action spaces in some tasks.
  - Real-world evaluations use 10 trials per data-mixture point and 20 configurations for correlation, so confidence intervals are wide.
  - Unlike the group's particle and physics models, this is a purely 2D video world model, which marks a shift in the lab's line in 2026.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2603.08546 (v1, full main text read).
- RSS 2026 program and proceedings entries, via web search: https://roboticsconference.org/program/papers/18/ and https://roboticsproceedings.org/rss22/p018.html.
- README and LICENSE: raw.githubusercontent.com (WangYixuan12/interactive_world_sim).
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - The per-task attribution of two correlation values.
  - The per-task supplementary table.
  - Hugging Face licenses.
  - Repository last update.
