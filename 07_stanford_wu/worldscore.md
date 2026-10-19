---
citation_count: 154
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429)"
venue: "ICCV 2025"
date: 2025-04-01
research_group: "Stanford (Jiajun Wu)"
---

# WorldScore: A Unified Evaluation Benchmark for World Generation

- **Authors:** Haoyi Duan*, Hong-Xing Yu*, Sirui Chen, Li Fei-Fei, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author; first authors are Wu's group members
- **Date published:** 2025-10 (ICCV 2025 conference, October 2025; the repo README reports acceptance in 06/2025; arXiv v1 2025-04-01, v2 2025-11-29)
- **Venue:** ICCV 2025, proceedings pp. 27713–27724 (DOI 10.1109/ICCV51701.2025.02573). Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2504.00983
  - CVF: https://openaccess.thecvf.com/content/ICCV2025/html/Duan_WorldScore_A_Unified_Evaluation_Benchmark_for_World_Generation_ICCV_2025_paper.html
  - Project: https://haoyi-duan.github.io/WorldScore/
  - Code: https://github.com/haoyi-duan/WorldScore
- **Number of citations:** 154 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
WorldScore is a benchmark, not a model. It puts 3D, 4D, image-to-video and text-to-video "world generators" on a common scale. It does this by splitting world generation into a sequence of next-scene generation tasks. Each task is specified by:
- the current scene (image and prompt),
- a next-scene prompt,
- a layout (a camera trajectory plus a camera-motion text).

Static and dynamic worlds are evaluated separately. Scores cover controllability, quality and dynamics.

## 1. Weights availability
- Not applicable: no model is trained.
- The evaluation pipeline downloads public metric checkpoints listed in the README: GroundingDINO, SAM ViT-H, SAM 2.1 and DROID-SLAM models (`models.zip` from Dropbox), among others.
- The dataset is on Hugging Face: https://huggingface.co/datasets/Howieeeee/WorldScore
- The leaderboard is a Hugging Face Space: https://huggingface.co/spaces/Howieeeee/WorldScore_Leaderboard

## 2. Code
- https://github.com/haoyi-duan/WorldScore, MIT License (LICENSE file, "Copyright (c) 2025 Haoyi Duan").
- It contains the evaluation code, wrappers for many world generators (`world_generators/`) and dataset download scripts.
- Updates per the README:
  - 06/2025: Voyager results added.
  - 11/2025: WonderJourney/WonderWorld evaluation code added.
- Last commit: 2026-07-23 (commits Atom feed). The repo is actively maintained.

## 3. Datasets
- **WorldScore dataset:** 3,000 test examples (Sec. 3.2), public on Hugging Face.
  - 2,000 static-world examples: 10 categories (5 indoor, 5 outdoor), 100 photorealistic images each plus a stylized counterpart in 1 of 7 styles. 20% are "large worlds" with 3 next scenes.
  - 1,000 dynamic-world examples: 5 motion types (articulated, deformable, fluid, rigid, multi-motion), 100 Unsplash images each plus a stylized counterpart, with motion masks.
- **Sources:** open scene datasets and Unsplash. Captions and categories come from GPT-4o; next-scene prompts come from an LLM. Layouts use 8 cinematic camera moves.

## 4. Evaluation and downstream tasks
- **Metrics:** 10 metrics, each normalized to 0–100.
  - Controllability: camera error (geometric mean of rotation and translation error), object detection success, CLIP content alignment.
  - Quality: 3D consistency (DROID-SLAM reprojection), photometric consistency (optical-flow AEPE), style consistency (Gram matrices), subjective quality (CLIP-IQA+ plus CLIP Aesthetic, selected with a 400-person human study).
  - Dynamics: motion accuracy (flow inside vs. outside the mask), motion magnitude, motion smoothness (frame-interpolation reference).
  - Aggregate scores: WorldScore-Static and WorldScore-Dynamic.
- **Models evaluated (Table 2):** 20 models:
  - 2 closed-source image-to-video (I2V): Gen-3, Hailuo.
  - 11 open-source video models.
  - 6 3D scene generators.
  - 1 4D model (4D-fy).
- **Key numbers (Table 2):**

  | Model | WorldScore-Static | WorldScore-Dynamic |
  |---|---|---|
  | WonderWorld | 72.69 (best) | 50.88 (no dynamics) |
  | LucidDreamer | 70.40 | 49.28 |
  | CogVideoX-I2V (best video model) | 62.15 | 59.12 (best) |
  | Gen-3 | 60.71 | 57.58 |
  | Hailuo | 57.55 | 56.36 |
  | 4D-fy | 27.98 | 32.10 |

  Best video-model camera controllability is 40.22 (CogVideoX-T2V), below every 3D and 4D model.
- **Findings (Sec. 4.1):**
  - 3D models lead on static worlds.
  - Video models lack camera control and are weak on long sequences and outdoor scenes.
  - There is a trade-off between motion magnitude and smoothness, and magnitude correlates weakly with motion accuracy.
  - Text-to-video (T2V) models are easier to steer than I2V models.
- **Downstream tasks:** none. This is an evaluation benchmark.

## 5. Architecture and techniques
- **Benchmark design:** next-scene generation specified as a triplet (C, N, L). Model-specific pre-processing `w_proc` converts the specification for each model type. All outputs are standardized as videos.
- **Metric tools:** DROID-SLAM (camera and 3D consistency), GroundingDINO (objects), CLIPScore, optical flow (photometric consistency and motion), a frame-interpolation model (smoothness), CLIP-IQA+ and CLIP Aesthetic.
- **Normalization and validation:** linear normalization using empirical bounds. Metrics are validated against human judgements (Supp. D).

## 6. Conclusions and future work
- **Authors' conclusions:**
  - 3D models excel at static worlds but struggle to add dynamics.
  - Video models struggle with controllability, long sequences and outdoor scenes.
  - Future directions: bridge 3D and 4D representations, add more robust controllability, and build architectures for extended scene sequences.
- **Own observations:**
  - "World generation" here is mostly camera-conditioned exploration plus scripted in-place motion. The benchmark does not test action-conditioned physical interaction (e.g., forces or robot actions), so it does not evaluate world models for control.
  - 3D models get 0 on all dynamics metrics, which pulls their Dynamic score down by construction.
  - Several metrics rely on learned proxies such as CLIP, DROID-SLAM and optical flow.

## 7. Sources and verification
- arXiv PDF v2: https://arxiv.org/pdf/2504.00983. Read Secs. 1–5, Tables 1–2 and Figures 1–7.
- CVF open-access page and ICCV virtual poster page (https://iccv.thecvf.com/virtual/2025/poster/169), found via web search.
- Repo README and LICENSE: https://github.com/haoyi-duan/WorldScore
- Semantic Scholar API: `paper/arXiv:2504.00983`
- Not verified: exact presentation type (poster or highlight).
