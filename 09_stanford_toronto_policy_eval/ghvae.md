---
citation_count: 122
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "CVPR 2021"
date: 2021-03-06
research_group: "Stanford / Toronto (policy evaluation)"
---

# Greedy Hierarchical Variational Autoencoders for Large-Scale Video Prediction (GHVAE)

- **Authors:** Bohan Wu, Suraj Nair, Roberto Martín-Martín, Li Fei-Fei†, **Chelsea Finn**† (†equal advising, ordered alphabetically; Stanford University)
- **Group role:** Senior (last, co-advising) author (Finn)
- **Date published:** 2021-06 (CVPR 2021, venue date); arXiv 2021-03-06
- **Venue:** CVPR 2021
- **Links:** arXiv https://arxiv.org/abs/2103.04174 · CVF supplemental https://openaccess.thecvf.com/content/CVPR2021/supplemental/Wu_Greedy_Hierarchical_Variational_CVPR_2021_supplemental.pdf · project page https://sites.google.com/view/ghvae
- **Number of citations:** 122 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
GHVAE is an action-conditioned stochastic video prediction model built from stacked VAE modules (encoder, decoder, prior, posterior per module) that are trained one module at a time with earlier modules frozen. Greedy training avoids GPU memory limits and the instability of end-to-end hierarchical VAEs, so much larger models can be trained on RoboNet-scale data. The model is used for visual-foresight planning on a real Franka robot.

## 1. Weights availability
Not released. Project page has no code or checkpoint links (checked 2026-10-15).

## 2. Code
No code found. Checked project page, web search and GitHub search for "GHVAE" and the title.

## 3. Datasets
- RoboNet (public, CC BY 4.0), KITTI, Human3.6M, Cityscapes (public).
- Real-robot fine-tuning set: 5000 autonomously collected random-interaction videos on a Franka (not released).

## 4. Evaluation and downstream tasks
- Video prediction (Table 1, 6-module GHVAE vs SVG'): RoboNet FVD 95.2 vs 123.2, LPIPS 0.036 vs 0.060; KITTI FVD 552.9 vs 1217.3; Human3.6M FVD 355.2 vs 429.9; human preference 86.6-93.3% for GHVAE.
- Cityscapes vs Hier-VRNN (Table 2): FVD 418.0 vs 567.5; SSIM 74.0 vs 62.8.
- Ablations: monotonic gains with more modules (RoboNet FVD 292.4 / 151.2 / 95.2 for 2/4/6 modules, Table 3); greedy beats end-to-end training (FVD 95.2 vs 509.9, Table 4); learned prior beats uniform prior (Table 6).
- Real-robot planning (Table 7, 20 trials, unseen objects): Pick&Wipe 90% vs SVG' 50%; Pick&Sweep 85% vs 50%.
- Downstream task: visual MPC (random-shooting planning) on a real robot.

## 5. Architecture and techniques
- Paradigm: hierarchical VAE, recurrent-convolutional prior and posterior; only the deepest latent is sampled at test time.
- Encoders compress spatial size while expanding channels; decoders receive skip information from the past.
- Action conditioning through the prior network (action input).
- Training: per-module ELBO (Eq. 2), with theorems on ELBO validity and monotonic improvement.
- Size: 599M parameters (vs 298M for SVG' at the same 24 GB memory).
- Planning: 2 context frames, 10 future actions, 4-D action space, goal image cost.

## 6. Conclusions and future work
- Authors: greedy hierarchical training improves stability and memory efficiency, giving 17-55% FVD gains and 35-40% higher real-robot success. Future: more than six modules, harder long-horizon tasks, other generative problems.
- Own observations: real-robot tests cover two tasks with 20 trials each; slight overfitting on smaller datasets (Table 5).

## 7. Sources and verification
- Paper: arXiv 2103.04174v3 PDF, main text read.
- Venue: CVF open-access supplemental listing (CVPR 2021), Semantic Scholar venue, Finn CV.
- Citations: Semantic Scholar API, 2026-10-15.
