---
citation_count: 10
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; 2026-10-15 re-check returned HTTP 429)"
venue: "NeurIPS 2026 (poster)"
date: 2026-06-25
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# Hallucination in World Models is Predictable and Preventable

- **Authors:** Nicklas Hansen, **Xiaolong Wang** (listed as "Nick Hansen, Xiaolong Wang" on the NeurIPS site)
- **Group role:** first author (Hansen) and senior author (Wang)
- **Date published:** acceptance listed for NeurIPS 2026; poster session scheduled for 2026-12-10, so the conference has not yet taken place as of 2026-10-13. arXiv v1 2026-06-25.
- **Venue:** NeurIPS 2026, main track, poster ("Accept (poster)" in the NeurIPS 2026 virtual-site paper data)
- **Links:** arXiv https://arxiv.org/abs/2606.27326 · OpenReview https://openreview.net/forum?id=4KT2peVuUl · NeurIPS https://neurips.cc/virtual/2026/poster/155605 · Project https://nicklashansen.com/mmbench2 · Code https://github.com/nicklashansen/mmbench2 · Data https://huggingface.co/datasets/nicklashansen/mmbench2 · Models https://huggingface.co/nicklashansen/mmbench2-models
- **Number of citations:** 10 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The authors train a 350M-parameter generative, action-conditioned visual world model in the style of Dreamer 4: a masked-autoencoder video tokenizer plus a block-causal Transformer dynamics model trained with shortcut flow matching. It is trained on a new 427-hour, 210-task dataset, MMBench2. They identify three hallucination modes: perceptual, action-marginalized, and scene-diverging. Three label-free predictors of where the model fails are proposed. Coverage-aware sampling at training time and curiosity-driven data collection with these predictors both reduce hallucination. With the second, the model adapts to unseen tasks using about 50 trajectories per task.

## 1. Weights availability
- Released on Hugging Face: https://huggingface.co/nicklashansen/mmbench2-models.
  - Pretrained and finetuned 350M checkpoints.
  - Each variant is a (tokenizer.pt, dynamics.pt) pair at 224×224: `base`, `coverage_aware`, `combined`.
- Download: `python download_checkpoints.py --variant combined` (or `base`, `coverage_aware`, `all`).
- An interactive browser demo runs with `./run_interactive.sh combined` (needs a CUDA GPU; seeds rollouts from live simulators).
- License: the repo is MIT; a separate license for the weights was not checked.

## 2. Code
- https://github.com/nicklashansen/mmbench2, MIT license.
- Contains: tokenizer and dynamics training, hallucination predictors, targeted data collection, interactive web interface, dataset preprocessing (the full sharded dataset is about 8 TB).
- Last push 2026-10-02 (actively maintained).

## 3. Datasets
- **MMBench2** (new, public at https://huggingface.co/datasets/nicklashansen/mmbench2):
  - 65,600 mixed-quality trajectories (including human play), 427 hours of 224×224 video at 15 fps (about 23M frames), with ground-truth actions, rewards, language instructions and live simulators.
  - 210 tasks in 10 domains (DMControl, DMControl Ext., Meta-World, ManiSkill3, MuJoCo, MiniArcade, Box2D, RoboDesk, OGBench, continuous Atari). Actions have 1–16 dimensions, zero-padded with masks.
  - 200 tasks for pretraining (about 20M frames for training, 3M held out); 10 new tasks held out as unseen.
  - Dataset license not stated on the project page.

## 4. Evaluation and downstream tasks
Metrics:
- reconstruction PSNR;
- rollout ΔPSNR versus a frame-repeat baseline (scene divergent if ≤ 0);
- action-shuffle ratio (actions counted as ignored if ≤ 1.1);
- hallucination predictors (dynamism-normalized);
- downstream normalized task score via closed-loop MPC with CEM (H=32, replanning every 16 steps).

Results:
- **Detection (Fig. 5):** all three predictors (tokenizer round-trip residual, flow instability, inter-seed variance) correlate with rollout ΔPSNR at Spearman ρ ≈ −0.79 to −0.81 on 9k held-out sequences. AUROC results for action-ignored and scene-divergent labels are in App. E.
- **Coverage-aware training (Table 1):** uniform-over-tasks sampling for both tokenizer and dynamics gives +0.44 dB reconstruction PSNR, +0.29 action-shuffle ratio, +0.88 dB rollout ΔPSNR, and lower values on all three predictors.
- **Targeted data collection, 10 unseen tasks (Table 2):** 50 trajectories per task; task performance via MPC.

  | Model / data source | Task score |
  |---|---|
  | Random policy | 0.118 |
  | Zero-shot coverage-aware model | 0.276 |
  | Finetuned, no-op data | 0.163 |
  | Finetuned, random-policy data | 0.228 |
  | Finetuned, curiosity data (u_r^norm) | 0.325 |
  | Finetuned, expert data | 0.362 |
  | Finetuned, human-play data | 0.362 |
  | Finetuned, all combined | 0.390 (rollout ΔPSNR +4.02 dB) |

- **Tokenizers (Table 3):** on seen tasks the in-domain tokenizer (102M) scores 38.29–39.66 PSNR versus 36.45 for the Wan 2.1 VAE. On unseen tasks it generalizes poorly (17.3 dB) until finetuned (38.04 dB).

## 5. Architecture and techniques
- **Tokenizer:** symmetric encoder–decoder Transformer. 224×224 input, patch stride 14 (256 patches), 64 latent queries, tanh bottleneck to a 64×64 code per frame. Masked-autoencoding loss (pixel MSE + LPIPS). 50M + 50M parameters.
- **Dynamics:** 250M-parameter block-causal Transformer over packed latents (32 spatial tokens, action token, shortcut-noise token, 4 registers, agent tokens). Shortcut flow-matching objective (Frans et al., 2025); about 4 Euler substeps per frame at inference.
- **Heads:** reward head (L=8 multi-step symlog two-hot regression) and Gaussian behaviour-cloning policy head, both conditioned on CLIP-ViT/B instruction embeddings.
- **Compute:** 58 GPU-days on 8×H100; context length T=24.
- **Mitigation:** task-uniform resampling; curiosity data collection that executes the world-model rollout with the highest predicted hallucination (H=32, replan every K=16).

## 6. Conclusions and future work
- **Authors:**
  - Hallucination in generative world models is mainly a data-coverage problem. Label-free predictors detect it, and the same signals mitigate it through coverage-aware training and targeted collection.
  - Limitations: 350M scale only; simulation only (transfer to billion-parameter models and to real robot data with sensor noise is open); high compute cost; need for more diverse data.
  - Open question raised: how to evaluate world models, given that goal-directed MPC tests only a narrow part of state space.
- **Own observations:**
  - Downstream scores are low in absolute terms (≤ 0.39 normalized).
  - The MPC evaluation uses only 3 episodes per task.
  - The architecture largely follows Dreamer 4, so the contribution is diagnostic and data-centric rather than architectural.
  - The paper has not yet appeared in printed proceedings (conference in Dec 2026).

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2606.27326 v1 (main text, Tables 1–3, Figs. 1–6); project page; GitHub README and API.
- NeurIPS 2026 acceptance confirmed in the NeurIPS 2026 virtual-site paper JSON ("Accept (poster)", poster ID 155605). The author's homepage listing matches.
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Unverified: dataset and weight licenses; whether the camera-ready version differs from arXiv v1.
