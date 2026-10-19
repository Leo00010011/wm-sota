---
citation_count: 30
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "ICML 2025"
date: 2025-04-23
research_group: "Stanford / Toronto (policy evaluation)"
---

# Latent Diffusion Planning for Imitation Learning

- **Authors:** Amber Xie, Oleh Rybkin, Dorsa Sadigh, **Chelsea Finn** (Stanford; UC Berkeley)
- **Group role:** Senior (last) author (Finn); co-advised with Dorsa Sadigh (not a PI of any summary group)
- **Date published:** 2025-07 (ICML 2025, venue date); arXiv 2025-04-23
- **Venue:** ICML 2025, spotlight poster; PMLR 267:68710-68724
- **Links:** arXiv https://arxiv.org/abs/2504.16925 · PMLR https://proceedings.mlr.press/v267/xie25h.html · ICML https://icml.cc/virtual/2025/poster/43658 · project page https://amberxie88.github.io/ldp/ · code https://github.com/amberxie88/latent_diffusion_planning
- **Number of citations:** 30 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
LDP learns a beta-VAE latent space from images, a diffusion "planner" that forecasts a dense sequence of future latent states (a latent, action-free forecasting model), and a diffusion inverse dynamics model that turns consecutive latent states into actions. Because the planner needs no actions and the IDM can use suboptimal data, LDP exploits action-free and failed trajectories that behavior cloning cannot use.

Scope note: the forecaster is not action-conditioned. It is included as world-model-based planning (prediction of future latent states used for control) and is a borderline case.

## 1. Weights availability
Not released. The project page and repository README document training from scratch (VAE, then latent processing, then agent) with user-supplied checkpoint paths; no checkpoint links found.

## 2. Code
https://github.com/amberxie88/latent_diffusion_planning, 69 stars, last push 2025-06-29 (GitHub API, checked 2026-10-15). No license file reported by the API. Contains VAE training (`train_vae.py`), latent pre-processing, LDP training (`train_bc.py`, `train_mixed_bc.py`), and suboptimal-data collection instructions.

## 3. Datasets
- Simulation: Robomimic Lift, Can, Square (public) and ALOHA sim Transfer Cube (public). Low-data regime: 3 demos (Lift), 100 demos (Can, Square), 25 demos (Cube); 500 failed trajectories from an undertrained BC agent as suboptimal data; 100 (or 25) action-free demos.
- Real: Franka Lift on a DROID-style setup, 82 demos, 84 suboptimal trajectories, 12 action-free demos (not released).

## 4. Evaluation and downstream tasks
- Metric: success rate (50 trials, 2 seeds in sim; 45 trials, 3 seeds real).
- Action-free data (Table 1), average over 4 tasks: DP 0.51, DP-VPT 0.59, UniPi-OL 0.11, UniPi-CL 0.18, LDP 0.65, LDP + action-free 0.66, LDP + action-free + suboptimal 0.95.
- Suboptimal data (Table 2) average: RC-DP 0.60, DP+Repr 0.49, DP PT+FT 0.64, LDP + subopt 0.70, LDP + action-free + subopt 0.95.
- Real Franka Lift (Table 3): DP 69.6%, LDP 73.3%, LDP + subopt 74.8%, LDP + action-free 79.3%.
- Ablation (Table 4): dense forecasting beats a hierarchical (subgoal) LDP variant (e.g. Square 0.46 vs 0.31).
- Downstream task: visuomotor imitation learning / planning.

## 5. Architecture and techniques
- Encoder: beta-VAE (image reconstruction + KL), latent concatenated with proprioception.
- Planner: DDPM over a horizon of future latents, Diffusion Policy conditional 1D-CNN U-Net backbone, conditioned on current latent.
- IDM: DDPM over actions given latent pairs, MLP-ResNet (IDQL-style).
- Receding-horizon control (plan horizon Hp, execute Ha).
- Parameter count: not stated in the main text.

## 6. Conclusions and future work
- Authors: dense latent-state forecasting with diffusion gives performance competitive with state-of-the-art imitation learning and makes heterogeneous data usable. Limits: VAE latent may not capture control-relevant features; diffusing over states costs extra compute; newer diffusion advances (DiT, flow matching) not explored. Future: harder real-world tasks, human/handheld data, autonomous robot data.
- Own observations: experiments are single-task and low-data; the gains from suboptimal data are largest where the base demos are very scarce (e.g. Lift with 3 demos), so the scaling story to large multi-task data is untested.

## 7. Sources and verification
- Paper: arXiv 2504.16925v1 PDF, main text read.
- Venue: PMLR v267 entry and ICML 2025 poster page (search results); project page says Spotlight Poster.
- Code: GitHub API and README; project page via WebFetch (2026-10-15).
- Citations: Semantic Scholar API, 2026-10-15.
