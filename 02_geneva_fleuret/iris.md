---
citation_count: 551
citation_source: "Google Scholar, 2026-10-13 (Semantic Scholar rate-limited, HTTP 429, 2026-10-15)"
venue: "ICLR 2023"
date: 2022-09-01
research_group: "Geneva (Francois Fleuret)"
---

# Transformers are Sample-Efficient World Models

- **Authors:** Vincent Micheli, Eloi Alonso, **François Fleuret** (all University of Geneva)
- **Group role:** Entirely in-group. Fleuret is senior (last) author; Micheli and Alonso are his PhD students.
- **Date published:** 2023-05 (ICLR 2023 conference, 1-5 May 2023; arXiv v1 2022-09-01, v2 2023-03-01)
- **Venue:** ICLR 2023, notable top 5%, oral (Oral session "Reinforcement Learning II"). An earlier version won the best paper award at the NeurIPS 2022 Deep RL Workshop (per the first author's page).
- **Links:** arXiv https://arxiv.org/abs/2209.00588 · OpenReview https://openreview.net/forum?id=vhFu1Acb0xb · ICLR oral page https://iclr.cc/virtual/2023/oral/12543 · Code https://github.com/eloialonso/iris
- **Number of citations:** 551 (source: Google Scholar, François Fleuret's profile, checked 2026-10-13; Semantic Scholar API returned HTTP 429)

## Summary
IRIS is a model-based RL agent that learns its policy only inside a learned world model ("in imagination"). The world model is a discrete autoencoder that turns each frame into 16 tokens and a GPT-style autoregressive transformer that predicts the next frame's tokens, the reward and episode termination. On Atari 100k it set the best mean human-normalized score among methods without lookahead search at the time.

## 1. Weights availability
- Released. Hugging Face: https://huggingface.co/eloialonso/iris/tree/main/pretrained_models has 26 checkpoints, one per Atari 100k game (e.g. `Breakout.pt`).
- Loading (README): copy the checkpoint to `checkpoints/last.pt` and run `./scripts/play.sh`. To resume training from a checkpoint, set `path_to_checkpoint` and the `load_tokenizer` / `load_world_model` / `load_actor_critic` flags in `config/trainer.yaml`.
- License: the Hugging Face page shows no license; the code repository is GPL-3.0.

## 2. Code
- https://github.com/eloialonso/iris, GPL-3.0, about 900 stars, 12 commits on `main`. The page shows no last-commit date, but the small commit count suggests the repo is a frozen release.
- Includes training (`src/main.py`, Hydra configs), evaluation (`eval.py`), play/visualization scripts, raw result scores and a notebook that reproduces the paper's figures (`results/results_iris.ipynb`). Developed with PyTorch 1.11.

## 3. Datasets
- No fixed dataset. The agent collects its own experience online in the Atari 100k benchmark: 26 Atari games, 100k agent actions per game (about 2 h of play), through the public Arcade Learning Environment (Atari ROMs needed).
- Appendix F also runs a 10M-step setting.

## 4. Evaluation and downstream tasks
- Downstream task: RL control on Atari 100k, with the policy trained entirely in imagination. 5 runs per game, 100 evaluation episodes at the end of training.
- Metrics: human-normalized score (HNS) mean / median / IQM, optimality gap, number of superhuman games, performance profiles, probability of improvement (rliable).
- Table 1: IRIS mean 1.046, median 0.289, IQM 0.501, optimality gap 0.512, 10 superhuman games. SPR: mean 0.616, IQM 0.337, 6 superhuman. DrQ: mean 0.465. SimPLe: mean 0.332. CURL: mean 0.261. MuZero (with search): mean 0.562.
- EfficientZero (lookahead search) does better (mean 1.943, median 1.090, 14 superhuman), so the "state of the art" claim is limited to methods without search. IRIS's median is below SPR's (0.396).
- Appendix F: with 10M environment steps, mean HNS reaches 7.488 and 15 games are superhuman.
- Qualitative: the world model captures game mechanics such as Pong score updates and Breakout brick removal (Figs. 3-4) and produces several plausible stochastic futures (Fig. 2).

## 5. Architecture and techniques
- Tokenizer: VQ-VAE-style discrete autoencoder on 64x64 frames, with a CNN encoder and decoder (4 layers, 2 residual blocks each, self-attention at 8/16 resolution). K = 16 tokens per frame, vocabulary N = 512, embedding dim 512. Loss: L1 + commitment + perceptual (the VQGAN discriminator is dropped) (App. A.1).
- Dynamics: minGPT / GPT-2-style causal transformer with 10 layers, 4 heads, D = 256 and a 20-timestep context. It autoregressively predicts frame tokens, reward and termination (Eqs. 1-3, App. A.2).
- Action conditioning: action tokens are interleaved with frame tokens in the sequence.
- Policy: actor-critic (CNN + 512-unit LSTM, shared trunk) trained on reconstructed imagined frames. Imagination horizon 20, DreamerV2-style lambda-returns (lambda 0.95, gamma 0.995), REINFORCE with a value baseline, entropy weight 0.001 (App. A.3).
- Parameter count: not stated in the IRIS paper. The Delta-IRIS paper (Table 1) lists IRIS at about 48-50M parameters.
- Compute: 8 A100 40GB, about 3.5 days per environment on average (App. G).

## 6. Conclusions and future work
- Authors: transformer world models are sample-efficient and capture game mechanics. Future work: scale to harder tasks, feed the world model's internal representations to the policy, and combine imagination with MCTS (Sec. 5). Stated failure modes: rare events (Frostbite igloo, a "double exploration" problem), small visual details erased with only 16 tokens (Alien player; App. E), sparse reward needing a temperature hack (Freeway, App. H).
- Own observation: the cost of autoregressive token-by-token generation (16 sequential predictions per frame) makes imagination slow, which the group's later papers (Delta-IRIS, DIAMOND) address. Evaluation is limited to discrete-action Atari.

## 7. Sources and verification
- https://arxiv.org/html/2209.00588 (full paper with appendices)
- https://iclr.cc/virtual/2023/oral/12543 and ICLR poster page (labelled "top 5% paper")
- https://github.com/eloialonso/iris ; https://huggingface.co/eloialonso/iris/tree/main/pretrained_models
- https://fleuret.org/francois/publications.html (lists it as ICLR 2023)
- Google Scholar profile of F. Fleuret (citation count)
- Not verified: the OpenReview page could not be read (bot check); the decision label comes from the ICLR virtual site and the repo README. Semantic Scholar was rate-limited (HTTP 429), so the citation count comes only from Google Scholar.
