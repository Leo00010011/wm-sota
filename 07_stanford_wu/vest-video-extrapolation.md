---
citation_count: 8
citation_source: "OpenAlex, 2026-10-15"
venue: "ECCV 2022"
date: 2022-05-04
research_group: "Stanford (Jiajun Wu)"
---

# Video Extrapolation in Space and Time (VEST)

- **Authors:** Yunzhi Zhang, **Jiajun Wu**
- **Group role:** senior (last) author; Wu advises the first author.
- **Date published:** 2022-10 (ECCV 2022 conference, Tel Aviv, October 2022). The arXiv preprint was first posted 2022-05-04; v3 is dated 2022-07-26.
- **Venue:** ECCV 2022, LNCS. DOI 10.1007/978-3-031-19787-1_18. Presentation type not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2205.02084
  - Proceedings: https://www.ecva.net/papers/eccv_2022/papers_ECCV/html/4275_ECCV_2022_paper.php
  - Project page: https://cs.stanford.edu/~yzzhang/projects/vest/
  - Code: https://github.com/zzyunzhi/vest
- **Number of citations:** 8 (source: OpenAlex, DOI record, checked 2026-10-15). The Semantic Scholar API does not resolve this paper by its arXiv ID or DOI, and a title search was rate-limited. OpenAlex likely undercounts compared with Google Scholar.

## Summary
VEST is a self-supervised model that, from two consecutive monocular frames, predicts both future frames (video prediction) and novel viewpoints (view synthesis). It uses a "generalized multiplane image" (MPI): RGBA depth planes, each with its own affine motion. Future frames are rendered by warping each plane with its motion; novel views are rendered by homography warping.

It is a passive (not action-conditioned) model that predicts future observations. It is included here as early video-prediction / 4D world-modelling work from the group.

## 1. Weights availability
Not found.
- The repo README shows evaluation commands that take `PATH_TO_CHECKPOINT`.
- No download link for checkpoints appears in the README or on the project page (both checked).

## 2. Code
- **Repository:** https://github.com/zzyunzhi/vest
  - Training code: `train.py`
  - Evaluation code: `compute_scores.py`, plus visualization scripts
  - Built on NVIDIA Imaginaire
- **License:** no LICENSE file in the repo (raw `LICENSE` returns 404).
- **Last commit:** 2023-04-14.

## 3. Datasets
All are public datasets except the cloud set, which the authors collected.

- **KITTI-raw:** 28 city scenes. Two splits:
  - LDI split: 20 / 4 / 4 (train / validation / test).
  - FVS split: 24 / 4 (train / test).
- **RealEstate10K:** 10K training and 5K test sequences (subset from Lai et al.). Evaluation uses 372 test sequences that overlap with MINE's test set.
- **ACID:** aerial coastline videos. Results are qualitative only.
- **Lin et al. dataset:** 10 static synchronized cameras; 86 / 10 train / test scenes.
- **Cloud videos:** 175 YouTube cloud videos collected by the authors. Available through a Google Drive link in the README.

## 4. Evaluation and downstream tasks
**Tasks:** novel view synthesis (LPIPS-VGG, SSIM, PSNR) and video prediction (LPIPS-AlexNet at t+1, t+3, t+5).

**KITTI, LDI split (Table 1), video prediction LPIPS at t+1 / t+3 / t+5:**

| Model | t+1 | t+3 | t+5 |
|---|---|---|---|
| VEST | 0.1154 | 0.2881 | 0.3911 |
| PredRNN-V2 | 0.3085 | 0.4573 | 0.5422 |

For novel view synthesis on KITTI, VEST beats Tucker & Snavely, LDI and MINE (the MINE baseline uses ImageNet pre-training).

**RealEstate10K (Table 2):**
- Better or comparable to MINE, SynSin and Tucker & Snavely.
- Comparable to the binocular method of Zhou et al.
- Only VEST predicts future frames: t+1 scores are LPIPS 0.0436, SSIM 0.9334, PSNR 31.68.

**Lin et al. dataset, single-view input (Table 3):**

| Model | LPIPS | SSIM | PSNR |
|---|---|---|---|
| VEST | 0.2591 | 0.6249 | 21.77 |
| Lin et al. | 0.3719 | 0.4929 | 16.78 |

**Downstream tasks:** none. There is no planning or control.

## 5. Architecture and techniques
- **Encoder:** an MPI encoder (architecture after Tucker & Snavely) takes I_{t−1} and I_t. It outputs D planes (16 in the KITTI visualization), each with alpha and 6 affine motion parameters. Plane RGB is copied from the source image.
- **Per-plane motion:** a spatial average of the motion features, weighted by each plane's compositing weight.
- **Novel views:** homography warping of the planes, followed by over-compositing.
- **Future frames:** affine warping of each plane, followed by compositing. Longer horizons are predicted autoregressively.
- **Depth scale:** set by aligning to sparse COLMAP points.
- **Loss:** ℓ1 plus VGG-19 perceptual loss, applied to both the spatial target (stereo or nearby frame) and the temporal target (next frame).
- **Generative paradigm:** deterministic regression. It is not diffusion and not a GAN.

## 6. Conclusions and future work
**Authors' conclusions:**
- View synthesis and video prediction give complementary self-supervision.
- The generalized MPI handles both tasks, matching or beating single-task methods.
- Further discussion is in Appendix C.5, which I did not read.

**Own observations:**
- The motion model is a per-plane affine transform. This limits complex non-rigid dynamics, although alpha blending mitigates it.
- The model is not action-conditioned and predicts short horizons only.
- It is borderline as a "world model". It is included because it is a top-venue future-observation predictor with Wu as last author.
- KITTI Table 1 values were read from extracted PDF text with merged columns. I report only the VEST and PredRNN-V2 rows, which I could align confidently.

## 7. Sources and verification
- **Paper:** arXiv PDF v3 https://arxiv.org/pdf/2205.02084. Read Secs. 1–5 and Tables 1–3.
- **Code and data:** repo README https://github.com/zzyunzhi/vest
- **Venue and DOI:** ECVA paper page and mlanthology, found via web search.
- **Citation count:** OpenAlex API (works search, DOI record). Semantic Scholar did not resolve the paper.
