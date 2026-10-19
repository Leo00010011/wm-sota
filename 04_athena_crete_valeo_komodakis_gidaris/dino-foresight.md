---
citation_count: 52
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live re-check returned HTTP 429 on 2026-10-15)"
venue: "NeurIPS 2025"
date: 2024-12-16
research_group: "Athena / Univ. of Crete + valeo.ai (Nikos Komodakis, Spyros Gidaris)"
---

# DINO-Foresight: Looking into the Future with DINO

- **Authors:** Efstathios Karypidis, Ioannis Kakogeorgiou, **Spyros Gidaris**, **Nikos Komodakis**
- **Group role:** All authors from the group (Archimedes/Athena RC and valeo.ai). Komodakis is senior (last) author.
- **Date published:** 2025-09-19 (acceptance date stated in the official GitHub README). arXiv v1: 2024-12-16. Conference: NeurIPS 2025, December 2025, San Diego.
- **Venue:** NeurIPS 2025 (39th Conference on Neural Information Processing Systems), poster
- **Links:** arXiv https://arxiv.org/abs/2412.11673 · OpenReview https://openreview.net/forum?id=gimtybo07H · NeurIPS poster https://neurips.cc/virtual/2025/poster/116713 · project page https://dino-foresight.github.io/ · code https://github.com/Sta8is/DINO-Foresight
- **Number of citations:** 52 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
DINO-Foresight is a world model that forecasts in the feature space of a frozen vision foundation model (DINOv2) instead of pixels or VAE latents. A masked feature transformer takes 4 context frames of multi-layer DINOv2 features, compressed by PCA, and regresses the features of the next frame. Task heads trained on ordinary DINOv2 features (segmentation, depth, surface normals, instance segmentation) then run on the forecast features without retraining. The model is deterministic and is rolled out autoregressively for longer horizons.

## 1. Weights availability
Released on Hugging Face, repo `Sta8is/DINO-Foresight` (https://huggingface.co/Sta8is/DINO-Foresight), license MIT (HF model card metadata):
- `dinof_lowres.ckpt` (224x448 stage), `dinof_highres.ckpt` (448x896 stage)
- `dinov2_pca_448_l[2_5_8_11]_1152.pth` (PCA matrices, 1152 components)
- DPT head checkpoints `head_segm_pca1152.ckpt`, `head_depth_pca1152.ckpt`, `head_normals_pca1152.ckpt` (same HF repo). The instance-segmentation (Mask2Former) head is not in the HF file list.
- Loading: pass `--ckpt dinof_highres.ckpt --eval_ckpt_only` to `train.py`. A Colab demo and `demo.ipynb` are linked in the README. HF repo last modified 2025-11-24.

## 2. Code
https://github.com/Sta8is/DINO-Foresight, MIT license, 13 commits, last push 2025-11-26 (GitHub API). Contains training code (two-stage low-res then high-res schedule via `train.py`), PCA precomputation (`pca.py`), preprocessing scripts for depth/normal pseudo-labels, and evaluation with DPT heads (`Downstream/`). Note: the README PCA example uses layers 2,5,9,11 but the checkpoints use 2,5,8,11.

## 3. Datasets
- **Cityscapes** (main): 2,975 train / 500 val sequences, 30 frames at 16 fps, 1024x2048; frame 20 annotated with 19-class segmentation. Public (registration required): https://www.cityscapes-dataset.com/
- **nuScenes**: 700 train / 150 val scenes, 12 Hz, about 20 s each. Public (registration): https://www.nuscenes.org/
- Depth and surface-normal targets are pseudo-labels (Depth Anything V2, Lotus on nuScenes) because the datasets lack them.

## 4. Evaluation and downstream tasks
Horizons: Cityscapes short-term (3 frames, ~0.18 s) and mid-term (9 frames, ~0.54 s); nuScenes mid (~0.75 s) and long (1.5 s). Baselines: Oracle (heads on real future frame), Copy-Last, VISTA fine-tuned (2.5B video diffusion world model), FUTURIST.

Cityscapes results (Table 1 / Table 6), short / mid:
- Semantic segmentation forecasting, mIoU ALL: 71.8 / 59.8; movable objects: 71.7 / 57.6 (VISTA-ft 64.9 / 53.9; Copy-Last 54.7 / 40.4; Oracle 77.0 short; FUTURIST 73.9 / 62.7, a segmentation-specialised model).
- Instance segmentation forecasting, AP50: 50.5 / 27.3; AP: 26.6 / 12.6 (VISTA-ft AP50 33.1 / 19.8).
- Depth forecasting, δ1: 88.6 / 85.4; AbsRel 0.114 / 0.136 (VISTA-ft δ1 86.4 / 82.8).
- Surface normals forecasting, % within 11.25°: 94.4 / 91.3; mean angular error 3.39 / 4.00 (VISTA-ft 93.0 / 90.0).
- nuScenes (Table 7): depth δ1 80.7 (mid) / 76.3 (long); normals 93.9 / 90.6. Zero-shot from Cityscapes still beats VISTA-ft (δ1 78.4 / 72.1).
- Speed: about 5 min vs about 8.3 h for VISTA on the 500 validation scenes (paper text).
- Ablations: DINOv2 beats EVA2-CLIP, SAM and VAE latents (VAE latents: ALL 33.4 short) (Table 2); continuous features beat a discrete 4M tokenizer at mid-term (57.3 vs 53.7) (Table 3); two-phase 224→448 training is best (Table 4); full masking of future frames beats random masking (Table 9); intermediate transformer layers improve downstream heads (Fig. 4).
- No planning, RL or control tasks are evaluated.

## 5. Architecture and techniques
- Encoder/latent space: frozen DINOv2-Reg ViT-B/14; features from layers 3, 6, 9, 12 concatenated and reduced with PCA (1152 components per released file).
- Predictor: masked feature transformer, 12 layers, hidden size 1152, factorised temporal + spatial attention; 5-frame sequence (4 context + 1 masked future frame with learnable [MASK] tokens).
- Paradigm: deterministic masked feature prediction (JEPA-like latent regression, no generative sampling); SmoothL1 loss (β = 0.1) on masked tokens.
- Resolution: 448x896 input, 32x64 = 2,048 tokens per frame; two-stage training with high-resolution adaptation.
- Parameters: 258M (Base); Small 115M, Large 460M (Table 11).
- Heads: DPT (segmentation, depth, normals), Mask2Former (instance segmentation).
- Training: Adam, lr 6.4e-4 cosine, effective batch 64, 8x A100 40GB.
- No action conditioning and no planner.

## 6. Conclusions and future work
Authors: forecasting VFM features is feasible and effective; one forecaster serves many tasks through plug-in heads; intermediate forecaster features are useful representations. Future work: model uncertainty (diffusion loss as in MAR, or GMM as in GIVT), hierarchical transformers for efficiency at higher feature resolution, scaling model size and data, newer encoders such as RADIOv2.5.

Own observations (not from the authors):
- Deterministic regression gives a mean future; this is a known weakness at longer horizons, which the later Latent-Foresight preprint addresses.
- Evaluation is on dense perception of the next frame in driving videos only; no action conditioning, so it is not usable for planning or control as is.
- Depth and normal "ground truth" are pseudo-labels from other networks, so these numbers measure agreement with those models.

## 7. Sources and verification
- https://arxiv.org/html/2412.11673 (v2, full text incl. appendix and limitations)
- https://neurips.cc/virtual/2025/poster/116713 (poster listing, track = poster)
- https://github.com/Sta8is/DINO-Foresight (README, license, acceptance date); GitHub API for push date
- https://huggingface.co/Sta8is/DINO-Foresight (weights)
- https://api.semanticscholar.org/graph/v1/paper/arXiv:2412.11673 (citations)
- Not verified: the exact NeurIPS proceedings page (proceedings pages 181689-181721 reported by a search result only).
