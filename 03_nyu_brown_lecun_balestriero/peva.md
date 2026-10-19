---
citation_count: 34
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live re-check returned HTTP 429 on 2026-10-15)"
venue: "NeurIPS 2025"
date: 2025-06-26
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Whole-Body Conditioned Egocentric Video Prediction (PEVA)

- **Authors:** Yutong Bai*, Danny Tran*, Amir Bar*, **Yann LeCun**†, Trevor Darrell†, Jitendra Malik† (* equal contribution, † equal advising)
- **Group role:** Co-senior author (LeCun is one of three equal advisors; listed fourth). Last author is Jitendra Malik (UC Berkeley), who is not a PI of another group in the summary, so the paper stays in this folder. Affiliations: UC Berkeley (BAIR), FAIR at Meta, NYU.
- **Date published:** 2025-12 (NeurIPS 2025 conference, December 2025). arXiv v1: 2025-06-26.
- **Venue:** NeurIPS 2025, Main Conference Track, poster (Advances in Neural Information Processing Systems 38).
- **Links:** arXiv https://arxiv.org/abs/2506.21552 · Proceedings https://papers.neurips.cc/paper_files/paper/2025/hash/f0552f14388d95b19740dee809f5cad1-Abstract-Conference.html · Poster page https://neurips.cc/virtual/2025/poster/117535 · Project page https://dannytran123.github.io/PEVA · Code https://github.com/DannyTran123/peva · BAIR blog https://bair.berkeley.edu/blog/2025/07/01/peva/
- **Number of citations:** 34 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
PEVA is an action-conditioned egocentric video world model in which the action is the person's whole-body motion: root translation plus relative rotations of 15 upper-body joints (48-D). An autoregressive Conditional Diffusion Transformer (extended from NWM) predicts future Stable-Diffusion-VAE latents of first-person video, and is used to rank candidate arm motions toward a goal image.

## 1. Weights availability
- The README says "The code and model weights are licensed under CC BY-NC 4.0", but no checkpoint download link was found in the README, the repo file list, or the project page (checked 2026-10-13). Treat weights as not publicly linked.

## 2. Code
- https://github.com/DannyTran123/peva, CC BY-NC 4.0 (LICENSE.md), last push 2026-07-22 (GitHub API).
- Contains training (`train.py`, `submitit_train_cw.py`, config `peva_cdit_xl.yaml`, 8 nodes × 8 GPUs) and evaluation (`isolated_peva_infer.py`, `isolated_peva_eval.py`: single-step, rollout, LPIPS/DreamSim/FID/FVD). Data preparation is in a separate repo: https://github.com/DannyTran123/wm_nymeria_dataset

## 3. Datasets
- Nymeria (Ma et al., 2024; Meta): large-scale real-world egocentric video with synchronized XSens full-body motion capture. Public dataset (license set by its providers). Sampled at 4 FPS, center-cropped to 224×224, 80/20 train/validation split (Section 4.1).
- Atomic-action evaluation subset: 100 automatically extracted clips per action type (forward, rotate left/right, hand up/down/left/right).

## 4. Evaluation and downstream tasks
- Metrics: LPIPS, DreamSim, PSNR, FID (single-step 2 s ahead and over 16 s rollouts); 5 samples per sequence.
- Baselines (Table 1, 2 s ahead): PEVA-XL LPIPS 0.303, DreamSim 0.193, FID 62.29; CDiT (NWM-style) 0.313 / 0.202 / 63.71; Diffusion Forcing variant 0.352 / 0.244 / 73.05.
- Atomic actions (Table 2, LPIPS): PEVA-XXL is best on all 11 actions, e.g. forward 0.325 vs CDiT 0.348.
- Ablations (Table 3): context 3 → 15 frames improves DreamSim 0.199 → 0.193; simple action concatenation beats an MLP embedding; scaling S → XXL improves FID 101.38 → 61.10.
- Long horizon: DreamSim degrades from 0.178 (1 s) to 0.390 (16 s) (Section 4.5).
- Planning: qualitative only. Candidate action sequences (left or right arm, 12-D, 8 steps) are simulated and ranked by LPIPS to a goal image; CEM planning as in NWM (Fig. 6, Section 5.1). No quantitative planning success metric.

## 5. Architecture and techniques
- Tokenizer: frozen Stable Diffusion VAE; prediction in latent space with 2×2 patches.
- Backbone: Conditional Diffusion Transformer (CDiT), sizes S to XXL (up to 32 layers); default XL.
- Changes vs NWM: random timeskips (16 frames sampled from a 32 s window, timeskip given as an action), sequence-level training with loss on every prefix (spatial attention within the current frame, attention to past frames only), actions concatenated into one vector fed to AdaLN.
- Action: 48-D (3 root translation + 15 upper-body joints × 3 Euler-angle deltas), normalized.
- Training: AdamW lr 8e-5, betas (0.9, 0.95), grad clip 10, batch 512; hardware as in NWM.
- Parameter count: not stated explicitly (sizes follow DiT S–XXL naming).

## 6. Conclusions and future work
- Authors: an initial attempt at modeling how whole-body human actions change the first-person view. Stated limitations: planning is preliminary (selection among candidates for one arm, no long-horizon planning or full trajectory optimization); no closed-loop or interactive control; no conditioning on task intent or semantic goals (image similarity is used as a proxy). Future work: closed-loop control, high-level goal conditioning, object-centric representations.
- Own observations: evaluation is almost entirely perceptual (LPIPS/FID) on one dataset; there is no quantitative downstream task. Gains over the NWM-style CDiT baseline are modest (e.g. LPIPS 0.303 vs 0.313).

## 7. Sources and verification
- Paper: arXiv PDF v1 (Sections 3–5, Tables 1–3), https://arxiv.org/pdf/2506.21552
- Venue: NeurIPS 2025 proceedings page and poster page; OpenReview venue "NeurIPS 2025 poster"; Semantic Scholar DBLP key conf/nips/BaiTBLDM25.
- Code: GitHub API (README, contents, LICENSE.md).
- Citations: Semantic Scholar API (arXiv:2506.21552).
- Not verified: location of released weights (license text mentions them, but no link found); Nymeria dataset size in hours (not given in the paper's main text).
