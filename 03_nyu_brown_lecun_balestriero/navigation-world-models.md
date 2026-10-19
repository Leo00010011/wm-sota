---
citation_count: 350
citation_source: "Semantic Scholar API, 2026-10-13 (carried over from file; live recheck 2026-10-15 returned HTTP 429)"
venue: "CVPR 2025 (oral)"
date: 2024-12-04
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Navigation World Models

- **Authors:** Amir Bar, Gaoyue Zhou, Danny Tran, Trevor Darrell, **Yann LeCun**
- **Group role:** Senior (last) author: LeCun. Affiliations in the paper: FAIR at Meta, New York University, Berkeley AI Research.
- **Date published:** 2025-06 (CVPR 2025 conference, June 2025; proceedings pp. 15791–15801). arXiv v1: 2024-12-04.
- **Venue:** CVPR 2025, oral (cvpr.thecvf.com/virtual/2025/oral/35338).
- **Links:** arXiv https://arxiv.org/abs/2412.03572 · CVF Open Access https://openaccess.thecvf.com/content/CVPR2025/html/Bar_Navigation_World_Models_CVPR_2025_paper.html · Project page https://www.amirbar.net/nwm · Code https://github.com/facebookresearch/nwm · Weights https://huggingface.co/facebook/nwm
- **Number of citations:** 350 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
NWM is an action-conditioned video world model for egocentric navigation: a Conditional Diffusion Transformer (CDiT) predicts the next frame's VAE latent from up to 4 past frames, a navigation action (planar translation + yaw) and a time shift. It is used to plan goal-image navigation with MPC/CEM, to rank trajectories sampled from an external policy (NoMaD), and to imagine trajectories in unseen environments from a single image.

## 1. Weights availability
- Released on Hugging Face, gated (users must accept sharing contact info): https://huggingface.co/facebook/nwm. Files: `cdit_s_100000.pth.tar`, `cdit_b_100000.pth.tar`, `cdit_l_100000.pth.tar`, `cdit_xl_100000.pth.tar`, `cdit_xl_ego4d_200000.pth.tar` (HF API, checked 2026-10-13).
- License: the GitHub repo LICENSE.md is CC BY-NC 4.0 (non-commercial). The HF model metadata reports "cc-by-4.0"; the two disagree, so check the model card after access is granted.
- Loading: download a checkpoint and place it in `./logs/nwm_cdit_xl/checkpoints` (README "Pretrained Models"); an interactive demo notebook `interactive_model.ipynb` is provided.

## 2. Code
- https://github.com/facebookresearch/nwm, CC BY-NC 4.0 (LICENSE.md), last push 2025-11-24 (GitHub API).
- Contains training (`train.py`, SLURM `submitit_train_cw.py`, config `nwm_cdit_xl.yaml`), evaluation (`isolated_nwm_eval.py` for single-step and trajectory-following prediction) and planning evaluation (`planning_eval.py`). Data preprocessing reuses the visualnav-transformer (GNM/NoMaD) scripts.

## 3. Datasets
- Training (robot navigation, public): SCAND, TartanDrive, RECON, HuRoN (SACSon). Actions derived from robot poses and normalized by average step size. The authors used a private higher-resolution HuRoN version that they cannot distribute (README).
- Unlabeled: a subset of Ego4D egocentric video (time-shift action only).
- Evaluation: RECON (known environment) and GO Stanford (unknown environment).
- Sizes are given in the paper's Appendix 8.1 (not checked).

## 4. Evaluation and downstream tasks
- Metrics: LPIPS, DreamSim, PSNR (single-step prediction), FID and FVD (video quality), ATE and RPE (trajectory accuracy).
- Ablations (Table 1, RECON, 4 s ahead): 4 goals per sample and 4 context frames are best (LPIPS 0.296); time-only conditioning fails (LPIPS 0.760); action + time gives LPIPS 0.295, DreamSim 0.091, PSNR 15.34.
- CDiT vs DiT (Fig. 5): CDiT is better up to 1B parameters; at equal parameters (CDiT-L vs DiT-XL) it is about 4× faster and more accurate.
- Video generation (Fig. 6): FVD 200.97 vs 762.73 for DIAMOND (16 s at 4 FPS on RECON).
- Goal-conditioned navigation (Table 2, RECON, 2 s trajectories): standalone NWM planning ATE 1.13 / RPE 0.35 vs NoMaD 1.93 / 0.52 and GNM 1.87 / 0.73; NWM ranking of NoMaD samples: ×16 → 1.83 / 0.50, ×32 → 1.78 / 0.48.
- Planning with constraints (Table 3): constraints are met with small changes in final position/yaw (e.g. "left-right first": δu −0.03, δφ +0.20).
- Unknown environments (Table 4, GO Stanford): adding unlabeled Ego4D improves LPIPS 0.658 → 0.652 and DreamSim 0.478 → 0.464, but hurts on the known RECON set (LPIPS 0.295 → 0.368).

## 5. Architecture and techniques
- Tokenizer: Stable Diffusion VAE (frozen); prediction in VAE latent space.
- Backbone: Conditional Diffusion Transformer (CDiT). Self-attention only over target-frame tokens, cross-attention to context-frame tokens, so cost is linear in the number of context frames. Default CDiT-XL, about 1B parameters, 4 context frames.
- Conditioning: action (u ∈ R², yaw φ) and time shift k (up to ±16 s) embedded with sine-cosine features + MLP, summed with the diffusion timestep embedding, injected via AdaLN.
- Objective: DiT diffusion loss (MSE on clean target) plus variational lower-bound loss for the predicted covariance; DiT noise schedule.
- Planner: MPC with the Cross-Entropy Method on an energy = −perceptual similarity (LPIPS on VAE-decoded final frame vs goal) + indicator penalties for invalid actions or unsafe states.
- Training: batch 1024 × 4 goals, AdamW lr 8e-5, 8 nodes × 8 H100 GPUs for XL.

## 6. Conclusions and future work
- Authors: NWM is a scalable data-driven navigation world model that plans standalone or with a policy and handles constraints. Stated limitations: mode collapse in out-of-distribution environments (outputs drift toward training data); weak simulation of dynamic agents such as pedestrians; only 3-DoF actions (6-DoF and arm control left as future work). They do not yet know what internal representations make this work, and suggest egocentric next-frame prediction may give rise to allocentric representations.
- Own observations: planning is expensive (1B-parameter diffusion rollouts, 3 samples per candidate, CEM), and planning is evaluated on short 2 s, 8-step trajectories with offline metrics (ATE/RPE vs ground truth), not closed-loop robot success. Ego4D data helped unknown-environment prediction only slightly and hurt in-domain prediction (Table 4).

## 7. Sources and verification
- Paper: https://arxiv.org/html/2412.03572 (Sections 3–6, Tables 1–4, Figs. 5–6).
- Venue: CVF Open Access page and CVPR 2025 oral page (https://cvpr.thecvf.com/virtual/2025/oral/35338); Semantic Scholar DOI 10.1109/CVPR52734.2025.01472.
- Code and license: GitHub API (repo contents, LICENSE.md). Weights: Hugging Face API for facebook/nwm.
- Citations: Semantic Scholar API (arXiv:2412.03572).
- Not verified: a "best paper candidate/honorable mention" claim seen only on a secondary site; dataset sizes (Appendix 8.1 not read in detail); exact HF model-card license text (gated).
