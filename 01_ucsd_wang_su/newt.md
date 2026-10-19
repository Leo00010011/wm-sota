---
citation_count: 15
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICLR 2026"
date: 2025-11-24
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Learning Massively Multitask World Models for Continuous Control

- **Authors:** Nicklas Hansen, **Hao Su**, **Xiaolong Wang** (Su and Wang marked "equal advising")
- **Group role:** first author (Hansen) plus both PIs as senior authors
- **Date published:** 2026-04 (ICLR 2026, conference month). arXiv v1 2025-11-24; v2 2025-12-02.
- **Venue:** ICLR 2026, poster ("Accept (Poster)" in the ICLR 2026 virtual-site data; https://www.iclr.cc/virtual/2026/poster/10009948)
- **Links:** arXiv https://arxiv.org/abs/2511.19584 · OpenReview https://openreview.net/forum?id=MPabX9LEds · Project https://www.nicklashansen.com/NewtWM · Code https://github.com/nicklashansen/newt · Models https://huggingface.co/nicklashansen/newt · Data https://huggingface.co/datasets/nicklashansen/mmbench
- **Number of citations:** 14 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Newt is a language-conditioned multitask world model built on TD-MPC2. It is first pretrained on a small set of demonstrations and then trained with online RL on 200 tasks at once. The paper also introduces MMBench, a 200-task, 10-domain benchmark with language instructions, demonstrations and optional 224×224 RGB observations. A single 20M-parameter agent beats PPO, FastTD3 and multitask TD-MPC2, can execute open-loop plans, and adapts quickly to unseen tasks.

## 1. Weights availability
- Released: 200+ checkpoints at https://huggingface.co/nicklashansen/newt (model-card license tag: MIT).
  - Multitask "soup" models in sizes S=2M, B=5M, L=20M and XL=80M.
  - 200 single-task TD-MPC2 expert agents used to collect the demonstrations.
- Download with `python download_checkpoints.py --filename "walker-walk"` or `--all` (requires `huggingface_hub`), then pass `checkpoint=<path>`. Single-task checkpoints need `model_size=B`.

## 2. Code
- https://github.com/nicklashansen/newt. The README states MIT; the GitHub API reports no detected license file.
- Contains training (`train.py`; single- and multitask, state or RGB), demo generation, a checkpoint downloader, and a Dockerfile.
- Last push 2026-01-09.

## 3. Datasets
- **MMBench:** 200 training tasks plus 20 test tasks (220 in total) across 10 domains: DMControl (21), DMControl Ext. (16), Meta-World (49), ManiSkill3 (36), MuJoCo (6), MiniArcade (19, new), Box2D (8), RoboDesk (6), OGBench (12) and continuous Atari (27). 41 tasks are new (Fig. 2).
- **Demonstrations:** 10–40 per task (4000+ in total, 32 GB), collected by single-task TD-MPC2 agents trained for 5M steps each. Public on Hugging Face (`nicklashansen/mmbench`).
- Each task has one language instruction.

## 4. Evaluation and downstream tasks
Metric: average normalized score across the 200 tasks; training uses 100M environment steps in total.
- **Multitask online RL (Fig. 1, Fig. 7):** Newt is more data-efficient and reaches a higher final score than PPO, FastTD3, multitask TD-MPC2, multitask BC and single-task BC.
  - Clear gains on DMControl, DMControl Ext., ManiSkill and MiniArcade.
  - On MuJoCo, Box2D and Atari it is close to BC.
- **Comparison with single-task experts (Table 9):** Newt lags the single-task agents trained to convergence (1B total steps). Example, DMControl: Newt 0.50 @100M vs single-task 0.81 @1B.
- **Ablations (Fig. 8):**
  - Language conditioning raises the score from 0.371 to 0.438.
  - Demonstrations, pretraining and the BC loss each help; combining them is best.
  - Larger models and batches help up to a point.
- **Few-shot transfer to 20 unseen tasks/embodiments (Fig. 9):** zero-shot 0.192 vs 0.013 from scratch; 0.868 vs 0.480 after 100k steps.
- **Zero-shot instructions (Table 1):** unseen object nouns hurt pushing (0.3% vs 21.0% success when "cube" is used instead), while the reverse holds for picking (10.5% vs 0%).
- **Open-loop control (Table 8):** planning up to 48 steps (16× the training horizon of 3) without feedback keeps an average 0.44 / 0.32 / 0.24 of closed-loop performance at 33% / 66% / 100% of the horizon.
- **Visual RL (Table 2):** adding RGB and finetuning for 30M steps changes the overall score by +0.004. RoboDesk +0.125, Meta-World +0.069, DMControl −0.029.
- **Training cost (Table 3):** 11.2 days on 1× RTX 3090, or 4.6 days on 2× RTX 5090 (state observations).

## 5. Architecture and techniques
- Built on TD-MPC2: a self-predictive, decoder-free latent world model with MLP components, SimNorm, discrete-regression rewards and values, a Q-ensemble, and MPPI planning (horizon 3, 6 iterations, 512 samples).
- Inputs: frozen CLIP-ViT/B text embedding (512-d), optional frozen DINOv2/B image embedding (768-d), and a 128-d state. Actions are padded to 16 dimensions with validity masks.
- New compared with TD-MPC2:
  - language conditioning in every component;
  - model-based pretraining of all components on the demos;
  - planning constrained toward the pretrained policy, annealed over the first 12% of training;
  - 50/50 oversampling of demonstrations;
  - a model-based BC term in the policy loss;
  - per-task discount factors;
  - a multi-GPU, asynchronous, `torch.compile` training pipeline.
- Default size 20M parameters; the family covers 2M, 5M, 20M and 80M (Table 6). Replay buffer of 10M.

## 6. Conclusions and future work
- **Authors:**
  - Online RL on hundreds of tasks at once is feasible and produces world models with transfer and open-loop capabilities.
  - Future work: cheaper visual RL, better language understanding (currently a bottleneck), stronger pretraining and base models, Transformer or diffusion architectures, analysis of latent structure, per-task curricula and sampling, and more and procedurally generated environments.
- **Own observations:**
  - The demonstrations come from privileged state-based expert agents, which limits how far the results apply to real-robot settings.
  - Most results use state observations; vision brings almost no aggregate gain.
  - Each task has only one fixed instruction, so the "language conditioning" is close to a task ID (as Table 1 shows).
  - Simulation only.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2511.19584 v2 (main text, Tables 1–3, 6–9, Figs. 1–10); GitHub README and API; Hugging Face model card.
- ICLR 2026 acceptance confirmed in the ICLR 2026 virtual-site paper data ("Accept (Poster)").
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: exact presentation day.
- The arXiv PDF still carries the "Preprint" header, not the camera-ready version.
