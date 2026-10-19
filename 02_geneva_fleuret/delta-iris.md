---
citation_count: 56
citation_source: "Google Scholar (F. Fleuret profile), 2026-10-13; Semantic Scholar unavailable (HTTP 429)"
venue: "ICML 2024"
date: 2024-06-27
research_group: "Geneva (Francois Fleuret)"
---

# Efficient World Models with Context-Aware Tokenization

- **Authors:** Vincent Micheli, Eloi Alonso, **François Fleuret** (University of Geneva; Micheli and Alonso equal contribution)
- **Group role:** Entirely in-group; Fleuret is senior (last) author.
- **Date published:** 2024-07 (ICML 2024, 21-27 July 2024; arXiv 2024-06-27)
- **Venue:** ICML 2024 (PMLR vol. 235, pp. 35623-35638), poster
- **Links:** arXiv https://arxiv.org/abs/2406.19320 · Proceedings https://proceedings.mlr.press/v235/micheli24a.html · ICML page https://icml.cc/virtual/2024/poster/34714 · Code https://github.com/vmicheli/delta-iris
- **Number of citations:** 56 (source: Google Scholar, François Fleuret's profile, checked 2026-10-13; Semantic Scholar API returned HTTP 429)

## Summary
Delta-IRIS is the successor to IRIS. Its discrete autoencoder encodes only the stochastic change ("delta") between time steps, conditioned on past frames and actions, so it needs only 4 tokens per frame. An autoregressive transformer predicts future deltas, conditioned on continuous per-frame embeddings ("I-tokens"). The result is a state-of-the-art agent on Crafter that trains about 10x faster than IRIS.

## 1. Weights availability
- Partially released. A Crafter agent checkpoint (5M frames) is at https://huggingface.co/vmicheli/delta-iris (`last.pt`, about 100 MB, PyTorch pickle).
- Loading (README): copy it to `checkpoints/last.pt` and run `./scripts/play.sh` (`-w` plays in the world model, `-a` shows the agent in the world model).
- No Atari checkpoints are mentioned. The Hugging Face page shows no license; the code is GPL-3.0.

## 2. Code
- https://github.com/vmicheli/delta-iris, GPL-3.0, 7 commits on `main` (a frozen release; the page shows no last-commit date).
- Includes training for Crafter (`python src/main.py`) and Atari (`env=atari params=atari`), Hydra configs and play/visualization scripts. No dedicated evaluation script is listed.

## 3. Datasets
- Online RL, no fixed dataset.
- Crafter (public procedurally generated 2D Minecraft-like benchmark), evaluated at 1M, 5M and 10M frames.
- Atari 100k (26 games, public ALE), reported in the appendix.

## 4. Evaluation and downstream tasks
- Downstream task: RL in imagination.
- Crafter return (Table 1): Delta-IRIS 7.7 / 15.4 / 16.1 at 1M / 5M / 10M frames. DreamerV3-XL (200M params): 9.2 / 14.2 / 15.1. DreamerV3-M: 6.2 / 12.6 / 13.7. IRIS at 1M: 5.5 (64 tokens) and 4.4 (16 tokens).
- Delta-IRIS beats DreamerV3 beyond about 3M frames but not at 1M (Fig. 4). It solves 17 of 22 tasks on average at 10M.
- Crafter score (Table 9): 9.30 / 39.67 / 42.47 at 1M / 5M / 10M.
- Speed and size (Table 1): 25M parameters, 20 FPS imagination, versus IRIS at 2-6 FPS (48-50M parameters). The text reports about 10x faster training than IRIS.
- Atari 100k (Table 8): mean HNS 1.39, IQM 0.65, 11 superhuman games, versus IRIS 1.05 / 0.50 / 10, DreamerV3 mean 1.10 and STORM mean 1.27. About 26 h of training, 5x faster than IRIS.
- Ablations (Tables 6-7, Fig. 6): removing I-tokens raises next-token cross-entropy from 1.57 to 1.73, and the imagined agent then glitches through walls. Reconstruction L2 is 0.000185 for Delta-IRIS with 4 tokens, versus 0.0017 for IRIS with 64 tokens.

## 5. Architecture and techniques
- Tokenizer: CNN encoder/decoder on 64x64 frames (5 layers, channels 64-256), conditioned on past frames and actions. K = 4 delta-tokens per frame, vocabulary 1024, token dim 64. Factorized, normalized VQ codes with an EMA codebook. Losses: L1 + L2 + max-pixel + commitment (Table 3, App. A.1).
- Dynamics: GPT-like causal transformer with 3 layers, 8 heads, dim 512 and 21 timesteps. It interleaves continuous I-tokens (CNN frame embeddings), action tokens and discrete delta-tokens. It predicts delta-tokens (cross-entropy), reward (two-hot symlog) and termination (Table 4).
- Policy: CNN + LSTM actor-critic, imagination horizon 15, REINFORCE with a value baseline, lambda-returns.
- Training: ratio of imagined to collected data 64; count-based replay sampling.
- About 25M parameters in total.

## 6. Conclusions and future work
- Authors: context-aware delta tokenization separates deterministic dynamics from stochastic events and gives large efficiency gains. Future work: let the policy use world-model representations, use a variable number of tokens per step, and address hard exploration on the missing Crafter tasks (e.g. through biased data sampling). The paper has no formal limitations section.
- Own observation: the gains at 1M frames are below DreamerV3-XL, and the method is still evaluated only on 2D discrete-action games at 64x64.

## 7. Sources and verification
- https://arxiv.org/html/2406.19320 (full paper)
- https://proceedings.mlr.press/v235/micheli24a.html ; https://icml.cc/virtual/2024/poster/34714
- https://github.com/vmicheli/delta-iris ; https://huggingface.co/vmicheli/delta-iris
- Google Scholar profile of F. Fleuret (citation count)
- Note: Fleuret's own publication page lists an older working title, "Efficient World Models with Time-Aware and Context-Augmented Tokenization". The proceedings title is used here. ICML presentation type (poster) was taken from the ICML virtual page URL; oral/spotlight status was not checked further.
