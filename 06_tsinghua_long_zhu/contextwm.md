---
citation_count: 55
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429)"
venue: "NeurIPS 2023"
date: 2023-05-29
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# Pre-training Contextualized World Models with In-the-wild Videos for Reinforcement Learning

- **Authors:** Jialong Wu, Haoyu Ma, Chaoyi Deng, **Mingsheng Long**
- **Group role:** Senior / last author (Long); all authors are THUML.
- **Date published:** 2023-12 (NeurIPS 2023 conference). arXiv v1 2023-05-29, v2 2023-10-27.
- **Venue:** NeurIPS 2023 (37th Conference on Neural Information Processing Systems), poster.
- **Links:** arXiv https://arxiv.org/abs/2305.18499 · poster https://neurips.cc/virtual/2023/poster/72660 · code https://github.com/thuml/ContextWM
- **Number of citations:** 55 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
ContextWM is a Dreamer-style latent dynamics world model that splits time-invariant context (one randomly chosen frame, passed through a context encoder) from time-varying dynamics. This lets the world model be pre-trained on diverse action-free in-the-wild videos and then fine-tuned for model-based RL. The authors call the paradigm In-the-wild Pre-training from Videos (IPV).

## 1. Weights availability
- Released. Google Drive: https://drive.google.com/drive/folders/1nCTU-MZYS7ssxNusPX_k1XnhKdmrptgD and Tsinghua Cloud: https://cloud.tsinghua.edu.cn/d/901049483a674e80b1e0/ (links taken from the README).
- Checkpoints: ContextWM pre-trained on SSv2 (recommended), Human3.6M, YouTube Driving, the three combined, and APV's RLBench videos, plus a plain WM pre-trained on SSv2. The repo is MIT; the checkpoints carry no separate license.

## 2. Code
- https://github.com/thuml/ContextWM, MIT license, 6 commits, about 70 stars. Date of the last commit not visible.
- Contains `wmlib` (the world-model library), action-free pre-training (`examples/train_apv_pretraining.py`), MBRL fine-tuning (`examples/train_apv_finetuning.py`) for Meta-World, DMC Remastered and CARLA, and a DreamerV2 baseline. There is no standalone evaluation script; evaluation runs inside training (CARLA uses a second server).

## 3. Datasets
- Pre-training (public, action-free): Something-Something-v2 (193K videos), Human3.6M (more than 3M frames), YouTube Driving (134 videos, more than 120 h), and their union. RLBench videos from APV are used for comparison.
- Downstream (simulated, public): Meta-World (6 tasks), DMC Remastered (3 tasks), CARLA Town04 highway (several weather conditions).

## 4. Evaluation and downstream tasks
- Metrics: success rate (Meta-World) and episode return (DMCR, CARLA), reported as interquartile mean with bootstrap CIs over 8 runs.
- Baselines: DreamerV2 and APV (renamed "IPV w/ Plain WM").
- **Meta-World (Fig. 5):** SSv2 pre-training improves on DreamerV2. ContextWM is more sample-efficient than plain WM on 5 of 6 tasks, and no method solves dial-turn. A plain WM barely benefits from pre-training (Fig. 5b).
- **DMC Remastered (Fig. 6):** SSv2 pre-training helps ContextWM despite the domain gap. Human3.6M pre-training transfers negatively (Fig. 7b).
- **CARLA (Fig. 6):** ContextWM learns faster early on, with only a small final gain over plain WM.
- **Data scaling (Fig. 7a):** 1.5k or 15k SSv2 videos come close to the full-data result.
- Qualitative results: video prediction on SSv2, t-SNE of representations, and compositional decoding.
- All results are in learning-curve figures; the main text has no tables.

## 5. Architecture and techniques
- Built on DreamerV2's RSSM with APV's stacked latent model: an action-free latent for video pre-training, plus an action-conditional latent for fine-tuning.
- A context encoder (U-Net-like) feeds the decoder through multi-scale cross-attention. Encoder and decoder are 13-layer ResNets.
- Dual reward predictors: a behavioural one (reward plus the APV intrinsic bonus) and a representative one (pure reward, beta_r = 1).
- Objective: ELBO with a contextualized image loss, action-free and action-conditional KL terms, and reward losses. Behaviour is learned by DreamerV2 actor-critic in imagination.
- Parameter count: not stated in the main text.

## 6. Conclusions and future work
- **Authors:** pre-training on in-the-wild video with explicit context/dynamics separation improves MBRL sample efficiency in manipulation, locomotion and driving. Stated limitations are that one random context frame may not capture enough context (for example in driving), that model and data scale are medium, and that generative reconstruction objectives waste capacity. Proposed next steps are multiple or multimodal context, Transformers with larger video corpora, and contrastive or self-predictive objectives.
- **Own observation:** Evaluation is simulation-only, with low-resolution (64x64) Dreamer-scale models. Gains over plain WM are small on CARLA, and the dial-turn task stays unsolved.

## 7. Sources and verification
- Full arXiv PDF v2 (pages 1-10 read), arXiv abstract page, NeurIPS virtual poster page, GitHub README, Semantic Scholar API.
- Not verified: a proceedings.neurips.cc URL (not returned by search), parameter count, and exact numbers (figures only).
