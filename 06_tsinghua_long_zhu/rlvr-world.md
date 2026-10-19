---
citation_count: 52
citation_source: "Semantic Scholar, 2026-10-13 (carried over from note; re-check returned HTTP 429 on 2026-10-15)"
venue: "NeurIPS 2025"
date: 2025-05-20
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# RLVR-World: Training World Models with Reinforcement Learning

- **Authors:** Jialong Wu, Shaofeng Yin, Ningya Feng, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); all authors are Tsinghua (THUML).
- **Date published:** 2025-12 (NeurIPS 2025 conference). arXiv v1 2025-05-20, camera-ready v2 2025-10-25. The repo README dates acceptance to 2025-09-18.
- **Venue:** NeurIPS 2025 (39th Conference on Neural Information Processing Systems), Main Conference, poster.
- **Links:** arXiv https://arxiv.org/abs/2505.13934 · proceedings https://papers.nips.cc/paper_files/paper/2025/hash/b63a24a1832bd14fa945c71f535c0095-Abstract-Conference.html · poster https://neurips.cc/virtual/2025/poster/116433 · project https://thuml.github.io/RLVR-World · code https://github.com/thuml/RLVR-World
- **Number of citations:** 52 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
RLVR-World post-trains autoregressive world models with reinforcement learning from verifiable rewards (GRPO). The reward is the prediction metric itself (accuracy, F1, or L1 + LPIPS on decoded frames), measured against the ground-truth next state. The authors apply it to language world models (text-game and web-page state prediction) and to iVideoGPT-style video world models on robot manipulation, and show better prediction plus downstream gains in web-agent MPC and real-to-sim policy evaluation.

## 1. Weights availability
- Released on Hugging Face (collection https://huggingface.co/collections/thuml/rlvr-world-682f331c75a904b8febc366a). The README says all models and datasets were released 2025-05-26.
- Language: `thuml/bytesized32-world-model-sft`, `-rlvr-binary-reward`, `-rlvr-task-specific-reward`, `thuml/webarena-world-model-sft`, `thuml/webarena-world-model-rlvr`.
- Video (RT-1): `thuml/rt1-frame-tokenizer`, `thuml/rt1-compressive-tokenizer`, `thuml/rt1-world-model-single-step-base`/`-rlvr`, `thuml/rt1-world-model-multi-step-base`/`-rlvr`.
- The repo is MIT. Per-model licenses were not checked individually.

## 2. Code
- https://github.com/thuml/RLVR-World, MIT license, about 280 stars. README news runs up to 2025-10-28.
- `lang_wm/` covers text-game and web-page state prediction and web-agent MPC. `vid_wm/` covers RT-1 trajectory prediction and real2sim policy evaluation. Training code was released 2025-05-21 and builds on verl.

## 3. Datasets
- **Text games:** ByteSized32-State-Prediction, 76,369 transitions from 31 games, 2,954 test transitions. SFT chain-of-thought data released as `thuml/bytesized32-world-model-cot`.
- **Web:** WMA's WebArena transition data, a 7K shorter-sample subset. Released as `thuml/webarena-world-model-cot`.
- **Video:** RT-1 (87,212 trajectories, 1% held out, 256x320, 13-dim actions); PushT, Rope and Granular from DINO-WM.
- All are public.

## 4. Evaluation and downstream tasks
- **Text-game state prediction (Table 1, R1-Distill-Qwen-1.5B):** overall accuracy goes from 32.87% (SFT) to 57.01% with a binary reward and 63.24% with a task-specific reward. The 7B binary-reward model reaches 65.53% overall, against GPT-4 at 64.76%.
- **Web-page state prediction (Table 2):** F1 goes from 49.94% (SFT) to 65.11% (RLVR).
- **Web agent on WebArena (Table 2):** MPC success rate goes from 12.06% to 14.29%, using DeepSeek-V3 as policy and value model.
- **RT-1 video world model (Table 3):**

| Setting | MSE (x100) | PSNR | SSIM (x100) | LPIPS (x100) | Repetition |
|---|---|---|---|---|---|
| Single-step, base | 0.336 | 25.3 | 81.7 | 13.0 | n/a |
| Single-step, RLVR | 0.287 | 25.9 | 83.1 | 12.2 | n/a |
| Multi-step, base | 0.659 | 23.1 | 80.9 | 14.8 | 48.6% |
| Multi-step, RLVR | 0.486 | 24.1 | 82.4 | 13.4 | 9.9% |

  The multi-step LPIPS gain is +9.2%, reached with only hundreds of RLVR steps, compared with hundreds of thousands of MLE steps.
- **DINO-WM benchmarks (Table 4):** comparable to DINO-WM overall and clearly better on Granular. Also compared with R3M, ResNet and DINO-CLS latent models and AVDC. Exact Table 4 values not extracted.
- **Real2Sim policy evaluation (Fig. 5; RT-1 and RT-1-X on 6 drawer tasks, human-judged):** world models track real success rates more closely than SIMPLER simulators, and RLVR improves on the base model.
- **Analysis (Fig. 4):** metric-specific rewards optimize their own metric best. Best-of-N: the base model catches up at N = 100. Larger GRPO groups help.

## 5. Architecture and techniques
- One sequence-modelling formulation: (state, action) tokens act as the "question" and next-state tokens as the "response".
- **Language world model:** DeepSeek-R1-Distill-Qwen-1.5B/7B, SFT (LoRA for web) followed by GRPO.
- **Video world model:** an iVideoGPT variant with a LLaMA-style 138M-parameter transformer trained from scratch. It uses a per-frame VQ tokenizer for single-step prediction and iVideoGPT's compressive tokenizer for multi-step. Each action dimension is discretized into 256 bins.
- **RL algorithm:** GRPO with reward R = -sum(L1 + LPIPS) on decoded frames. An optional repetition-penalty reward is also used.

## 6. Conclusions and future work
- **Authors:** RLVR is a general post-training paradigm for world models. Open problems they list: training plateaus within hundreds of steps; out-of-distribution and counterfactual generalization; the need for general-purpose pre-trained video world models to apply the full pre-train, SFT, RL pipeline; extension to diffusion models; and richer rewards (physics, temporal consistency).
- **Own observation:** The video experiments use small models (138M) trained per dataset. The real2sim evaluation depends on human annotation of success. Best-of-N results suggest RLVR mainly sharpens the distribution rather than adding new capability.

## 7. Sources and verification
- Full arXiv PDF v2 (main text and appendix sections on video models), NeurIPS proceedings and poster pages, Long's lab page, GitHub README, Semantic Scholar API.
- Not verified: the exact numbers in Table 4 (the PDF layout extraction missed them), the licenses of individual Hugging Face models, and whether the presentation was a poster or spotlight beyond the "poster" page.
