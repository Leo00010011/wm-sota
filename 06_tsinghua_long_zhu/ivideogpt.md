---
citation_count: 180
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; 2026-10-15 re-check returned HTTP 429)"
venue: "NeurIPS 2024"
date: 2024-05-24
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# iVideoGPT: Interactive VideoGPTs are Scalable World Models

- **Authors:** Jialong Wu, Shaofeng Yin, Ningya Feng, Xu He, Dong Li, Jianye Hao, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); first author Jialong Wu is a THUML student.
- **Date published:** 2024-12 (NeurIPS 2024 conference; poster session 2024-12-12). arXiv v1 2024-05-24.
- **Venue:** NeurIPS 2024 (Advances in Neural Information Processing Systems 37), Main Conference Track, poster.
- **Links:** arXiv https://arxiv.org/abs/2405.15223 · proceedings https://proceedings.neurips.cc/paper_files/paper/2024/hash/7dbb5bfab324e3b86af9bd0df15498dd-Abstract.html · poster https://neurips.cc/virtual/2024/poster/96668 · project https://thuml.github.io/iVideoGPT · code https://github.com/thuml/iVideoGPT
- **Number of citations:** 180 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
iVideoGPT is an autoregressive transformer world model that puts visual observations, actions and rewards into one token sequence and predicts the next token. A "compressive" tokenizer encodes future frames into a few tokens conditioned on context frames, which makes long interactive rollouts cheap. It is pre-trained on about 1.4M human and robot manipulation trajectories and then adapted to action-conditioned video prediction, visual planning and visual model-based RL.

## 1. Weights availability
- Released on Hugging Face under the `thuml` organization (MIT license on the model card), with a Tsinghua Cloud mirror (per the README).
- Pre-trained OXE models: `thuml/ivideogpt-oxe-64-act-free` (138M transformer), `thuml/ivideogpt-oxe-64-act-free-medium` (436M), `thuml/ivideogpt-oxe-64-goal-cond`, `thuml/ivideogpt-oxe-256-act-free`.
- Downstream models: `ivideogpt-bair-64-act-free`, `ivideogpt-bair-64-act-cond`, `ivideogpt-robonet-64-act-cond`, `ivideogpt-vp2-robosuite-64-act-cond`, `ivideogpt-vp2-robodesk-64-act-cond`.
- Not released: an OXE action-conditioned model (heterogeneous action spaces). The README says the RoboNet 256x256 checkpoints were accidentally deleted.
- Loading: the model card points to the GitHub repo for usage (`inference/predict.py`). The card's generic `DiffusionPipeline` snippet looks like Hugging Face template text and was not verified.

## 2. Code
- https://github.com/thuml/iVideoGPT, MIT license, 59 commits, about 186 stars. The latest README news item is 2025-09-18.
- Includes tokenizer and transformer pre-training (`train_tokenizer.py`, `train_gpt.py`, `scripts/pretrain`), fine-tuning (`scripts/finetune`), evaluation (`scripts/evaluation`), a visual-planning folder (`vp/`) and MBRL (`mbrl/train_metaworld_mbpo.py`).

## 3. Datasets
- Pre-training: Open X-Embodiment (35 sub-datasets, filtered and weighted as in Octo) plus 95 of the 174 Something-Something-v2 classes (15% weight). That is 1,417,954 trajectories in total, 120,581 of them SSv2 videos. Both datasets are public.
- Evaluation: BAIR robot pushing, RoboNet, the VP2 benchmark (Robosuite 5k trajectories, RoboDesk 35k), and Meta-World (6 tasks). All are public.

## 4. Evaluation and downstream tasks
Metrics are FVD, PSNR, SSIM and LPIPS (SSIM and LPIPS x100), plus success rate.
- **Video prediction, BAIR 64x64 (Table 1):** action-conditioned FVD 60.8 / PSNR 24.5 / SSIM 90.2 / LPIPS 5.0, against MaskViT action-conditioned FVD 70.5. Action-free FVD 75.0, against MAGVIT 62.0.
- **Video prediction, RoboNet, action-conditioned:** at 64x64, FVD 63.2 against FitVid 62.5. At 256x256, FVD 197.9 against MaskViT 211.7. The authors call the results competitive rather than uniformly best.
- **Visual planning (VP2; 4 Robosuite and 7 RoboDesk tasks):** matches the strongest baseline (SVG') on average and beats all baselines by a large margin on two RoboDesk tasks. It underperforms on "open slide". Per-task numbers are only in Fig. 5.
- **Visual MBRL (Meta-World, 6 tasks; MBPO-style with a DrQ-v2 actor-critic):** much more sample-efficient than DrQ-v2 and matches or beats DreamerV3, both with and without pre-training. The authors call this the first successful MBPO on visual continuous control.
- **Analysis:** zero-shot prediction for an unseen gripper (after tokenizer adaptation), few-shot gains at 100 and 1,000 trajectories, and validation loss that keeps falling with model size.

## 5. Architecture and techniques
- Conditional VQGAN "compressive tokenizer": context frames become 16x16 = 256 tokens, and future frames become 4x4 = 16 tokens via multi-scale cross-attention to the context. Codebook size 8,192, embedding dim 64.
- LLaMA-style decoder-only transformer (GPT-2 sizes): 138M (12 layers, 768 hidden) and 436M (24 layers, 1024 hidden). Vocabulary 16,386.
- Actions are linearly projected and added to slot-token embeddings. Rewards come from a linear head on the last hidden state (MSE loss). Training uses next-token cross-entropy.
- Resolution is 64x64 for the main experiments and 256x256 for RoboNet.
- Downstream use: MPC-style planning on VP2, and synthetic rollouts for MBPO with DrQ-v2.

## 6. Conclusions and future work
- **Authors:** iVideoGPT is a scalable interactive world model that can serve as a drop-in environment for MBRL. They list these limitations: little diversity in public robot data; the need for more modalities (multi-view, proprioception); compressive tokenization assumes the first frames give enough context, which can fail on long videos or with strong camera motion; and real-robot tasks are still untested.
- **Own observation:** Every result is at low resolution (mostly 64x64) and in simulated or curated benchmarks. Its video-prediction advantage over FitVid and MAGVIT is small or mixed. The released models cover action-free OXE pre-training only, so action-conditioned use needs fine-tuning per domain.

## 7. Sources and verification
- arXiv HTML v3 (https://arxiv.org/html/2405.15223v3), NeurIPS proceedings and poster pages, GitHub README, Hugging Face model card `thuml/ivideogpt-oxe-64-act-free`, Semantic Scholar API.
- Not verified: per-task VP2 numbers (figure only), the date of the latest commit (not visible), and the presentation type beyond "poster".
