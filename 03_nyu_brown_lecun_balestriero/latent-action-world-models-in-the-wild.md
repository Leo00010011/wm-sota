---
citation_count: 50
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICML 2026"
date: 2026-01-08
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Learning Latent Action World Models in the Wild

- **Authors:** Quentin Garrido, Tushar Nagarajan, Basile Terver, Nicolas Ballas, **Yann LeCun**, Michael Rabbat
- **Group role:** Co-author (LeCun, second to last). Last author is Michael Rabbat (Meta FAIR), who is not a PI of another group in the summary, so the paper stays in this folder. Affiliations: FAIR at Meta, Inria, NYU. Corresponding author: Quentin Garrido (Meta).
- **Date published:** 2026-07 (ICML 2026 conference; PMLR 306:34113–34150). arXiv v1: 2026-01-08; v2: 2026-01-20.
- **Venue:** ICML 2026, poster (icml.cc/virtual/2026/poster/65056; OpenReview venue "ICML 2026 regular").
- **Links:** arXiv https://arxiv.org/abs/2601.05230 · Proceedings https://proceedings.mlr.press/v306/garrido26a.html · ICML page https://icml.cc/virtual/2026/poster/65056
- **Number of citations:** 48 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper trains a latent action world model on uncurated in-the-wild video (YouTube-Temporal-1B): an inverse dynamics model infers a continuous latent action between consecutive frozen V-JEPA 2 representations, and a ViT-L world model predicts the next representation from the past and that latent action. It studies how to limit latent-action information (sparsity, noise, vector quantization), shows actions transfer across videos, and uses a small controller mapping real robot or navigation actions to latent actions so the world model can be used for planning.

## 1. Weights availability
Not released as far as found. No model or checkpoint links on the arXiv page, PMLR page or ICML page; GitHub search and a web search found no repository (checked 2026-10-13).

## 2. Code
No code found. Checked the arXiv abstract, PMLR proceedings page (its GitHub link is only the PMLR assets repo), GitHub repository search and a web search.

## 3. Datasets
- Training: YouTube-Temporal-1B (public video IDs; Zellers et al., 2022), 16-frame clips at 4 fps, 30k iterations at batch 1024 (Section 4). The default recipe sees each video about twice but only about 1% of all frames (Fig. 12 caption).
- Evaluation: Kinetics (leakage and transfer tests), RECON (navigation; transfer, controller and planning), DROID (Franka manipulation; controller training), and a set of real-world Franka Panda videos following the protocol of Terver et al. (2025) for arm-reaching planning.
- Ablation: mixing DROID into pretraining at 0–100% weight (Table S3).

## 4. Evaluation and downstream tasks
- One-step prediction error on in-the-wild video ("capacity"): sparse and noisy continuous latents span the range between unconstrained and deterministic models; VQ stays close to the deterministic baseline (Fig. 4).
- Future leakage (Table 1, Kinetics, LPIPS): error rises ×2.0–2.5 under artificial scene cuts for all regularizers, so latent actions do not simply copy the next frame.
- Cycle-consistent transfer (Table 2, LPIPS over 2 s): increase ×1.03–1.34 on Kinetics and ×1.03–1.22 on RECON.
- Manipulation planning (Table S1, DROID controller, CEM with H = 3, distance to goal Δxyz in m, lower is better): noisy-high 0.10, noisy-mid 0.11, sparse-high 0.13 vs V-JEPA 2-AC 0.15 and the best V-JEPA 2-based model of Terver et al. 0.05.
- Navigation planning (Table S2, RECON, NWM protocol): noisy-high ATE 1.40 / RPE 0.40 vs NoMaD 1.93 / 0.52 and NWM 1.13 / 0.35.
- Adding DROID to pretraining (Table S3): noisy latents reach Δxyz 0.06 at 75–90% DROID weight, close to the action-labeled 0.05.
- Scaling (Fig. 12): bigger models, longer training and more data improve in-the-wild IDM prediction; on DROID planning, only longer training gives a clear gain.

## 5. Architecture and techniques
- Encoder: frozen V-JEPA 2-L, frame-causal.
- World model p_ψ: ViT-L with RoPE, conditioned frame-wise on latent actions via AdaLN-zero; teacher forcing; L1 loss in representation space.
- Inverse dynamics model g_φ: infers a 128-D continuous latent action z_t from (s_t, s_{t+1}); trained jointly with the world model.
- Information regularizers: sparsity (L1 + VICReg-style variance-covariance-mean terms), noise (VAE-style KL to N(0,1) with weight β), or VQ (UniVLA-style with codebook reset) as baseline.
- Controller: MLP (actions only) or cross-attention adapter (actions + past representations) trained with L2 to predict IDM latent actions; the action-only version collapses to "no motion" because latent actions are camera-relative.
- Optimizer: Muon (lr 0.02) + AdamW (6.25e-4), cosine schedule. A separate ViT-L video decoder is trained for visualization only.
- Planner: CEM over real actions mapped to latent actions, goal cost in V-JEPA 2 representation space.

## 6. Conclusions and future work
- Authors: latent action world models can be trained purely on in-the-wild video; continuous constrained latents capture complex actions (people entering a scene, motion transfer between objects) where VQ fails; without a shared embodiment, latent actions become spatially localized and camera-relative; with a light controller the model plans close to action-labeled baselines. Future work: adapt the information constraint per video; sample and plan directly in latent-action space; single-stage training of encoder and world model (e.g. inside V-JEPA 2 pretraining).
- Own observations: planning results are in the appendix and come from small evaluation sets; the in-the-wild model still trails the best domain-trained world model by 2× on Δxyz (0.10 vs 0.05) and trails NWM on RECON. No code or weights, so results cannot be reproduced directly.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2601.05230 (Sections 3–10, Tables 1–2) and PDF (Supplementary Tables S1–S3).
- Venue: PMLR v306 index (garrido26a) and ICML 2026 poster page.
- Citations: Semantic Scholar API (arXiv:2601.05230).
- Not verified: number of evaluation episodes for the DROID/Franka planning protocol (in Supplementary A, not read in full); parameter count (only "ViT-L" is stated).
