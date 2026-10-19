---
citation_count: 1
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICLR 2026"
date: 2025-05-22
research_group: "Tsinghua (Mingsheng Long, Jun Zhu)"
---

# Foresight Diffusion: Improving Sampling Consistency in Predictive Diffusion Models

- **Authors:** Yu Zhang, Xingzhuo Guo, Haoran Xu, Jialong Wu, **Mingsheng Long** (corresponding)
- **Group role:** Senior / corresponding author (Long); all authors are THUML.
- **Date published:** 2026-04 (ICLR 2026 conference; poster session 2026-04-25). arXiv v1 2025-05-22, under the title "Consistent World Models via Foresight Diffusion"; v2 2026-03-21.
- **Venue:** ICLR 2026, poster.
- **Links:** arXiv https://arxiv.org/abs/2505.16474 · proceedings https://proceedings.iclr.cc/paper_files/paper/2026/hash/de0f2a9943b7bd060e5c10c2fb2654f3-Abstract-Conference.html · poster https://iclr.cc/virtual/2026/poster/10011106 · PDF on lab page https://ise.thss.tsinghua.edu.cn/~mlong/doc/Foresight-Diffusion-ICLR26.pdf
- **Number of citations:** 1 (source: Semantic Scholar API, checked 2026-10-13; the count may be split across the arXiv v1 title and the camera-ready title)

## Summary
ForeDiff is a predictive (action- or instruction-conditioned) diffusion model for video prediction and spatiotemporal forecasting. It addresses the poor sampling consistency of diffusion world models, meaning large spread across samples for the same past and actions. It separates condition understanding from denoising: a deterministic ViT "predictive stream" is pre-trained to predict the future, then frozen, and its features condition a DiT "generative stream". It is a borderline inclusion (see README): the arXiv v1 explicitly framed it as a world model ("Consistent World Models via Foresight Diffusion"), and its main benchmark is action-conditioned RoboNet prediction.

## 1. Weights availability
Not released. No checkpoint link appears in the paper. The thuml GitHub organization has no matching repository (searches for "fore" and "diffusion" on 2026-10-13), and no Hugging Face model was found.

## 2. Code
No code found. The paper gives no repository link, the thuml GitHub org search returns no ForeDiff repo, and a web search found nothing.

## 3. Datasets
- **RoboNet:** 162k videos, 7 robots. Predict 10 frames from 2 past frames plus actions, at 64x64.
- **RT-1:** 87k videos. Predict 14 frames from 2 frames plus the language instruction, at 64x64.
- **HeterNS:** heterogeneous 2D Navier-Stokes, 20 frames at 64x64. Predict 10 frames from 10.
- All public.

## 4. Evaluation and downstream tasks
Metrics are FVD, PSNR, SSIM and LPIPS, plus the new STD_PSNR, STD_SSIM and STD_LPIPS (spread across samples, lower is more consistent); L2 and relative L2 for HeterNS. Table 1 values are averages over samples:

| Dataset | Model | FVD | PSNR | SSIM | LPIPS | STD_LPIPS |
|---|---|---|---|---|---|---|
| RoboNet | Vanilla diffusion | 53.8 | 27.1 | 88.2 | 5.65 | 0.65 |
| RoboNet | ForeDiff | 51.5 | 27.4 | 88.8 | 5.25 | 0.35 |
| RT-1 | Vanilla diffusion | 11.7 | 30.4 | 93.6 | 3.79 | 0.53 |
| RT-1 | ForeDiff | 12.0 | 31.2 | 94.4 | 3.42 | 0.17 |

- **With CFG on RoboNet (Table 2):** ForeDiff LPIPS 5.05 and STD_LPIPS 0.24, against vanilla diffusion with CFG at 5.27 and 0.49.
- **Against prior work on RoboNet, best of 100 samples (Table 3):** FVD 51.5 against iVideoGPT 63.2 and FitVid 62.5. PSNR 28.2 equals FitVid. LPIPS 4.5 is worse than FitVid's 2.4.
- **HeterNS (Table 4):** relative L2 of 0.18 for ForeDiff, 0.83 for ForeDiff-zero and 1.50 for vanilla diffusion (x100).
- **Ablations (Fig. 8):** internal features work better than PredHead outputs as the condition. A few ViT blocks are enough. The gain is not just from more parameters: an 18-block vanilla DiT is still worse.
- There is no planning, RL or policy-evaluation downstream task.

## 5. Architecture and techniques
- Latent flow-matching or diffusion model (linear interpolation, velocity prediction) in the latent space of a pretrained autoencoder.
- **Predictive stream:** 6 ViT blocks on the condition (past frames plus actions or instruction), pre-trained with an MSE prediction head and then frozen with the head removed.
- **Generative stream:** 12 DiT (or SiT) blocks on the noised target, fused with the predictive features and the timestep.
- **ForeDiff-zero:** the same two-stream design trained end-to-end without stage-1 pre-training.
- **Theory (Lemma 3.1):** at t = 1 the diffusion model reduces to a deterministic predictor, which motivates using predictive ability as a proxy.

## 6. Conclusions and future work
- **Authors:** decoupling condition understanding from denoising, plus deterministic pre-training, improves both accuracy and sampling consistency of predictive diffusion models. The camera-ready conclusion states no explicit future-work list.
- **Own observation:** Experiments are at 64x64 with short horizons and no closed-loop use of the model. FVD on RT-1 is slightly worse than vanilla diffusion. The world-model framing was dropped from the final title, and no code or weights are available.

## 7. Sources and verification
- Full arXiv PDF v2 (ICLR 2026 camera-ready, main text; tables from a layout-mode extraction), ICLR poster page, ICLR proceedings link (from search results), Long's lab publication list, Semantic Scholar API.
- Not verified: an official code release (none found) and the reason for the low citation count (the S2 record may be fragmented).
