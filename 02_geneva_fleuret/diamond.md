---
citation_count: 426
citation_source: "Google Scholar (F. Fleuret profile), 2026-10-13; Semantic Scholar unavailable (HTTP 429)"
venue: "NeurIPS 2024 (Spotlight)"
date: 2024-05-20
research_group: "Geneva (Francois Fleuret)"
---

# Diffusion for World Modeling: Visual Details Matter in Atari

- **Authors:** Eloi Alonso, Adam Jelley, Vincent Micheli, Anssi Kanervisto, Amos Storkey, Tim Pearce, **François Fleuret** (University of Geneva, University of Edinburgh, Microsoft Research)
- **Group role:** First author (Alonso) and senior (last) author (Fleuret) from the Geneva group; Edinburgh (Jelley, Storkey) and Microsoft Research (Kanervisto, Pearce) are co-authors.
- **Date published:** 2024-12 (NeurIPS 2024, 10-15 Dec 2024; arXiv v1 2024-05-20)
- **Venue:** NeurIPS 2024, Spotlight (Advances in Neural Information Processing Systems 37, pp. 58757-58791)
- **Links:** arXiv https://arxiv.org/abs/2405.12399 · Proceedings https://proceedings.neurips.cc/paper_files/paper/2024/file/6bdde0373d53d4a501249547084bed43-Paper-Conference.pdf · NeurIPS page https://nips.cc/virtual/2024/poster/95428 · Project https://diamond-wm.github.io · Code https://github.com/eloialonso/diamond
- **Number of citations:** 426 (source: Google Scholar, François Fleuret's profile, checked 2026-10-13; Semantic Scholar API returned HTTP 429)

## Summary
DIAMOND (DIffusion As a Model Of eNvironment Dreams) is an RL agent trained entirely inside a diffusion world model. The model generates the next frame directly in pixel space with an EDM-style U-Net, conditioned on past frames and actions. It achieved the best mean human-normalized score on Atari 100k (1.46) for agents trained fully in a world model. The same model, scaled up, also works as an interactive "neural game engine" trained on static Counter-Strike: GO gameplay.

## 1. Weights availability
- Released on Hugging Face: https://huggingface.co/eloialonso/diamond, with folders `atari_100k` (world model and policy for each Atari 100k game) and `csgo` (CS:GO world model).
- Loading: `python src/play.py --pretrained` on the `main` branch lets you pick a game, then downloads and caches the Atari models. For CS:GO, run `git checkout csgo` then `python src/play.py`.
- The Hugging Face page shows no license; the code is MIT.

## 2. Code
- https://github.com/eloialonso/diamond, MIT license, 27 commits on `main`. The page shows no last-commit date.
- Branches: `main` (Atari agent), `csgo` (CS:GO world model), `ddpm` (the DDPM ablation from Sec. 5.1).
- Includes training (`python src/main.py env.train.id=BreakoutNoFrameskip-v4`, Hydra configs, a model-free option), play/visualization modes, and per-game, per-seed results (`results/data/DIAMOND.json`). There is no separate evaluation script.

## 3. Datasets
- Atari 100k: 26 games, 100k agent steps (about 2 h of play) per game, collected online; public ALE.
- Static ablation datasets: 100k frames from expert policies (Breakout for EDM vs DDPM, and the comparison against IRIS).
- CS:GO: the public "Online" dataset of Pearce & Zhu (2022), 5.5M frames (about 95 h) of human play on the Dust II map at 16 Hz. 5M frames (about 87 h) are used for training and 0.5M (about 8 h, 500 episodes) are held out.

## 4. Evaluation and downstream tasks
- RL on Atari 100k (5 seeds per game). Metrics: HNS mean / IQM, superhuman game count (rliable aggregates).
- Table 1: DIAMOND mean HNS 1.459, IQM 0.641, 11 superhuman games. STORM 1.266 / 0.636 / 10. DreamerV3 1.097 / 0.497 / 9. IRIS 1.046 / 0.501 / 10. TWM 0.956 / 0.459 / 8. SimPLe 0.332 / 0.130 / 1.
- Gains are largest in games where small details matter (Asterix, Breakout, Road Runner). Search-based and model-free methods (EfficientZero, BBF) are compared separately in App. J.
- Analysis (Sec. 5): DDPM drifts out of distribution with few denoising steps while EDM stays stable even with 1 step. One step blurs multimodal outcomes, so 3 steps are used. DIAMOND shows fewer frame-to-frame inconsistencies than IRIS and needs 3 network evaluations per frame versus IRIS's 16.
- CS:GO world model (Sec. 6): qualitative only (no quantitative metric). It runs at about 10 Hz on an RTX 3090 and stays stable over hundreds of steps. No FID/FVD/LPIPS numbers are reported.

## 5. Architecture and techniques
- Generative paradigm: score-based diffusion with the EDM formulation (Karras et al. 2022; preconditioning, sigma_data 0.5, log-normal noise sampling) and an Euler sampler with 3 steps. Trained with denoising score matching, reparameterized as a reconstruction of the clean next frame (Eqs. 5, 7).
- Backbone: 2D U-Net (4 levels, [2,2,2,2] residual blocks, 64 channels each). It conditions on the last 4 frames (stacked as channels) and injects actions and diffusion time through adaptive group normalization.
- Separate reward and termination model: CNN + LSTM with two heads.
- Policy: CNN-LSTM actor-critic trained in imagination (horizon 15, gamma 0.985, lambda 0.95, REINFORCE with a value baseline).
- Size: about 13M parameters in total (about 4M for the diffusion model), versus 30M for IRIS and 18M for DreamerV3 (as stated in the paper). Compute: about 2.9 days per run on one RTX 4090.
- CS:GO version: 381M parameters, including a 51M diffusion upsampler from 56x30 to 280x150 (10 steps). Trained for 12 days on an RTX 4090.

## 6. Conclusions and future work
- Authors: visual detail matters for RL in world models, and diffusion with EDM gives stable, sample-efficient world models. Stated limitations: discrete-control environments only, limited memory through frame stacking (a cross-attention variant was worse; App. M), reward/termination not integrated into the diffusion model. Future work: scale model, data and memory, continuous control, transformer/DiT-style temporal models, quantitative evaluation of the CS:GO model. Stated CS:GO failure modes: drift in rarely seen areas, forgetting when visibility is lost, unrealistic repeated mid-air jumps.
- Own observation: the 4-frame context means the model has no real long-term memory. The CS:GO result is only qualitative and trained on a single map. The Atari gains in IQM over STORM are marginal (0.641 vs 0.636).

## 7. Sources and verification
- https://arxiv.org/html/2405.12399 (full paper; the last ~20% of the appendix text was not read)
- NeurIPS proceedings PDF and https://nips.cc/virtual/2024/poster/95428
- https://github.com/eloialonso/diamond ; https://huggingface.co/eloialonso/diamond ; https://diamond-wm.github.io
- Google Scholar profile of F. Fleuret (citation count and page numbers)
- Not verified: median HNS was not extracted. The introduction's "87 hours" of CS:GO data is consistent with 5M frames at 16 Hz.
