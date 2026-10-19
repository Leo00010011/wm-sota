---
citation_count: 73
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check returned HTTP 429)"
venue: "ICLR 2026"
date: 2025-05-20
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# Vid2World: Crafting Video Diffusion Models to Interactive World Models

- **Authors:** Siqiao Huang, Jialong Wu, Qixing Zhou, Shangchen Miao, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); Zhou is from Chongqing University.
- **Date published:** 2026-04 (ICLR 2026 conference). arXiv v1 2025-05-20, camera-ready v3 2026-03-08. The repo README dates acceptance to 2026-01.
- **Venue:** ICLR 2026 (Fourteenth International Conference on Learning Representations), poster.
- **Links:** arXiv https://arxiv.org/abs/2505.14357 · poster https://iclr.cc/virtual/2026/poster/10007346 · project https://knightnemo.github.io/vid2world/ · code https://github.com/thuml/Vid2World · weights https://huggingface.co/collections/thuml/vid2world
- **Number of citations:** 73 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Vid2World turns a pre-trained, full-sequence (non-causal) video diffusion model, DynamiCrafter, into an autoregressive, action-conditioned interactive world model. It does this in two steps: video diffusion causalization (causal temporal attention, causal temporal convolution initialized by "extrapolative weight transfer", and Diffusion Forcing training) and causal action guidance (frame-level action injection with classifier-free guidance on the latest action). It is evaluated on robot manipulation (RT-1), 3D game simulation (CS:GO) and open-world navigation (RECON).

## 1. Weights availability
- Released on Hugging Face (README news: 2025-12). Repo license: Apache-2.0 according to the LICENSE file; the README badge says MIT, which is inconsistent.
- Main models: `thuml/Vid2World-RT1`, `thuml/Vid2World-CSGO`, `thuml/Vid2World-RECON` (extrapolative transfer, action guidance, 100k steps).
- Ablation models: `Vid2World-RT1-NAG`, `-RT1-Masked-NAG`, `-RT1-30k`, `-RT1-Masked`, `-RT1-Shift` (30k steps).
- The base model, DynamiCrafter 320x512, comes from its own release.

## 2. Code
- https://github.com/thuml/Vid2World, Apache-2.0 (LICENSE file), about 79 stars. Training, inference and evaluation code was released 2025-12.
- Training configs for RT-1, CS:GO and RECON; inference in AR, non-AR, long-rollout and OOD modes (Valorant, Delta Force); `eval.py` with `--env rt1|csgo|recon_time|recon_rollout`.

## 3. Datasets
- Base model pre-training: DynamiCrafter's internet-scale video data (not retrained here).
- **RT-1** (Google robot, public).
- **CS:GO:** Pearce & Zhu online gameplay, 5.5M frames (95 h), with DIAMOND's 0.5M-frame hold-out set for testing.
- **RECON:** outdoor navigation, (x, y, yaw) actions, 4 fps.
- All public.

## 4. Evaluation and downstream tasks
Metrics are FVD, FID, SSIM, LPIPS, PSNR and DreamSim (Table 1).

| Domain | Model | FVD | FID | SSIM | LPIPS | PSNR |
|---|---|---|---|---|---|---|
| RT-1 | AVID (non-AR) | 39.3 | 3.436 | 0.842 | 0.142 | 25.3 |
| RT-1 | Vid2World-NAR | 18.7 | 5.871 | 0.856 | 0.140 | 25.8 |
| RT-1 | Vid2World (AR) | 18.5 | 5.806 | 0.842 | 0.152 | 24.6 |
| CS:GO | DIAMOND-HQ | 368.5 | 87.2 | 0.447 | 0.510 | 18.3 |
| CS:GO | Vid2World | 106.6 | 17.5 | 0.481 | 0.404 | 18.7 |
| RECON | NWM (1B, one-step) | 31.2 | 34.1 | 0.389 | 0.295 | 15.34 |
| RECON | Vid2World (AR) | 59.4 | 42.9 | 0.481 | 0.324 | 16.10 |

- On RT-1, the authors note that baseline FIDs from AVID's code were computed without eval mode and are artificially low.
- The CS:GO result is about a 71% FVD gain and an 80% FID gain over the best DIAMOND setting.
- On RECON, Vid2World is on par with NWM and beats NWM + Ego4D on 4 of 6 metrics. In autoregressive rollouts (Fig. 6) it is better than or comparable to NWM and much better than DIAMOND.
- **Real2Sim policy evaluation (Fig. 5):** for RT-1 close-drawer with Begin, 15% and Converged checkpoints, human-judged success tracks the real-world trend.
- **Ablations (Table 2, 30k steps):** masked and extrapolative transfer both beat shift transfer, extrapolative is slightly better, and action guidance helps. Too high a guidance scale degrades quality (Fig. 8).
- **Baselines:** action-conditioned and language-conditioned fine-tuning, ControlNet, classifier guidance, AVID, DIAMOND and NWM.

## 5. Architecture and techniques
- **Backbone:** DynamiCrafter, a U-Net latent video diffusion model at 320x512 (1.1B trainable parameters; the intro says 1.4B in total).
- **Causalization:** causal masks on temporal attention. Temporal convolutions are converted with shift, masked or extrapolative weight transfer; extrapolative transfer redistributes future-tap weights by local linear extrapolation (m = 1, p = 2).
- **Training:** Diffusion Forcing, with independent noise levels per frame.
- **Actions:** each action is embedded by an MLP and added at the matching frame position. Action dropout during training enables classifier-free guidance on the most recent action (Theorem 4.1).
- **Inference:** autoregressive denoising of the newest frame with a clean history.
- **Compute:** about 7 days on 4x A100 for 100k steps.

## 6. Conclusions and future work
- **Authors:** first systematic transfer of passive full-sequence video diffusion models into interactive world models, with state-of-the-art results across three domains. Limitations: a lightweight base model (larger models such as Cosmos or Open-Sora 2 should help) and long training time.
- **Own observation:** On RT-1 the autoregressive model is not better than the non-autoregressive baselines on SSIM, LPIPS or PSNR. On RECON it trails NWM on FVD and FID. Downstream use is limited to human-judged policy evaluation on one task family, with no planning or RL inside the model.

## 7. Sources and verification
- Full arXiv PDF v3 (ICLR 2026 camera-ready, main text; Table 1 values from a layout-mode extraction), ICLR poster page, project page, GitHub README and LICENSE, Semantic Scholar API.
- Not verified: the exact real2sim success numbers (figure only) and whether the ICLR presentation was upgraded beyond poster.
