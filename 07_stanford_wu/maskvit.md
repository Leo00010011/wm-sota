---
citation_count: 160
citation_source: "Semantic Scholar, 2026-10-13 (value from existing note; live recheck on 2026-10-15 returned HTTP 429)"
venue: "ICLR 2023"
date: 2022-06-23
research_group: "Stanford (Jiajun Wu)"
---

# MaskViT: Masked Visual Pre-Training for Video Prediction

- **Authors:** Agrim Gupta, Stephen Tian, Yunzhi Zhang, **Jiajun Wu**, Roberto Martín-Martín, Li Fei-Fei
- **Group role:** co-author (4th of 6). The senior author is Li Fei-Fei (Stanford SVL), who is not a PI of any group in the summary. Following the ownership rule, the paper is kept here because no other summary group owns it. The first three authors are Stanford SVL members.
- **Date published:** 2023-05 (ICLR 2023 conference, May 2023; arXiv v1 2022-06-23, v2 2022-08-06)
- **Venue:** ICLR 2023 (Semantic Scholar venue: ICLR; Wu's homepage lists "ICLR 2023"). Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2206.11894
  - Project page: https://maskedvit.github.io/
  - Repository: https://github.com/agrimgupta92/maskvit
- **Number of citations:** 160 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
MaskViT is an action-conditioned video prediction world model that can be used for planning. It has two stages:
- A per-frame VQ-GAN tokenizes each frame into a 16×16 grid of tokens.
- A bidirectional transformer, pre-trained with masked visual modeling (MVM), predicts the masked future tokens. The transformer alternates spatial and spatiotemporal window attention.

At inference, iterative non-autoregressive decoding with a mask schedule makes prediction up to 512× faster than autoregressive decoding. This speed makes real-robot visual MPC possible.

## 1. Weights availability
Not released. The GitHub repo contains only `README.md` and an `images/` folder: no code and no checkpoints (repo file listing checked). The project page has no weight links.

## 2. Code
No code found. https://github.com/agrimgupta92/maskvit is a placeholder with only a README and images; its last commit is 2022-06-22. VP² (s-tian/vp2) uses MaskViT as a baseline, but its README lists pretrained weights only for SVG′, FitVid, MCVD and Struct-VRNN.

## 3. Datasets
All public:
- **BAIR robot pushing:** 64×64, 1 context frame, 15 frames predicted, no actions.
- **KITTI:** 57 training videos, 5 context frames, 25 frames predicted. Evaluated at 64×64 and at 256×256.
- **RoboNet:** 15M frames from 7 robot arms, 5-D actions, 2 context frames, 10 frames predicted.
- **Own Sawyer data:** 120K frames of autonomous random interaction, used for fine-tuning in the real-robot experiments. Not released as far as I found.

## 4. Evaluation and downstream tasks
**Metrics:** FVD, PSNR, SSIM, LPIPS. PSNR, SSIM and LPIPS are best-of-100 samples, except BAIR, which uses 1 trial.

**Table 1 results:**

| Dataset | MaskViT | Best or reference baseline |
|---|---|---|
| BAIR, action-free (FVD) | 93.7 (189M params) | FitVid 93.6 (302M params) |
| BAIR, goal-conditioned (FVD) | 76.9 | — |
| BAIR, action-conditioned (FVD) | 70.5 | — |
| KITTI (FVD / PSNR / SSIM / LPIPS) | 401.9 / 27.2 / 58.1 / 0.089 | GHVAE FVD 552.9; FitVid PSNR 17.1, LPIPS 0.217 |
| RoboNet (FVD) | 133.5 | FitVid 62.5 |

On RoboNet, MaskViT is worse than prior work. The authors attribute this to VQ-GAN flicker: VQ-GAN reconstructions alone have FVD 121, a lower bound for MaskViT.

**Ablations (Table 2, BAIR):**
- Spatiotemporal window: 16×4×4 gives FVD 96.6 with 7.0 GB and 12.5 h of training, versus 98.2 with 16.4 GB and 40.3 h for full attention.
- Masking ratio: a variable ratio (0.5–1) gives FVD 96.6, better than any fixed ratio (best fixed: 110.9 at 0.95).

**Speed (Table 3):** 160× fewer forward passes than autoregressive decoding on BAIR, 320× on BAIR with actions, 133× on KITTI and 512× on RoboNet.

**Real-robot visual MPC (Table 4):** Sawyer arm, CEM planner, ℓ2 cost to a 64×64 goal image, 30 trials per method.

| Model | Success rate |
|---|---|
| MaskViT, all data | 60% |
| MaskViT, fine-tuned | 53% |
| MaskViT, RoboNet only | 3% |
| Random policy | 3% |

Planning takes about 6.5 s per CEM iteration.

**Downstream tasks:** visual model-predictive control on real-robot pushing ("table setting" and "sweeping").

## 5. Architecture and techniques
- **Tokenizer:** per-frame VQ-GAN, 16×16 tokens per frame. Using per-frame codes allows conditioning on any subset of frames, e.g. goal frames.
- **Transformer:** L blocks. Each block has two layers: spatial window attention (1×16×16) and spatiotemporal window attention (T×4×4). Positional embeddings are learnable sums of space and time embeddings. The default is 6 blocks with 768-d embeddings; models have 181M–257M parameters depending on the dataset.
- **Training objective:** masked visual modeling with a variable masking ratio r ∈ [0.5, 1), using negative log-likelihood of the masked tokens.
- **Inference:** iterative decoding with a concave (cosine-type) mask schedule. At each step the most confident tokens are kept, with temperature-annealed Gumbel noise added to confidences; temperature 4.5 is best.
- **Action conditioning:** actions are linearly projected and added to the token embeddings.
- **Planner:** CEM, run in visual-foresight style.

## 6. Conclusions and future work
- **Authors' conclusions:** MVM pre-training with window attention gives competitive video prediction, and iterative decoding enables real-robot planning.
- **Limitations stated by the authors:** per-frame quantization causes flicker, especially with static backgrounds such as RoboNet. Scaling to strong camera motion (driving, egocentric video) remains hard.
- **Future work stated by the authors:** more complex robot tasks and planners.
- **Own observations:**
  - The real-robot evaluation is small (30 trials per method) and limited to pushing.
  - No code or weights were released, so reproduction is hard.
  - Wu is not the senior author. Including this paper here follows the precedent of keeping co-authored papers whose senior author is not a summary PI.

## 7. Sources and verification
- **Paper:** arXiv PDF v2, https://arxiv.org/pdf/2206.11894. Read Secs. 1–5 and Tables 1–4.
- **Repository:** README and file listing at https://github.com/agrimgupta92/maskvit.
- **Project page:** https://maskedvit.github.io/.
- **Semantic Scholar API:** `paper/arXiv:2206.11894` (venue ICLR).
- **Not verified:** OpenReview decision page and presentation type (not opened).
