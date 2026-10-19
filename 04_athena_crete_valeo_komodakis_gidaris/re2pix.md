---
citation_count: 1
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check on 2026-10-15 rate-limited)"
venue: "ECCV 2026"
date: 2026-04-13
research_group: "Athena / Univ. of Crete + valeo.ai (Nikos Komodakis, Spyros Gidaris)"
---

# Representations Before Pixels: Semantics-Guided Hierarchical Video Prediction (Re2Pix)

- **Authors:** Efstathios Karypidis, **Spyros Gidaris**, **Nikos Komodakis**
- **Group role:** All authors from the group (Archimedes/Athena RC, NTUA, valeo.ai, Univ. of Crete, IACM-FORTH). Komodakis is senior (last) author.
- **Date published:** 2026-06-17 (acceptance date stated in the official GitHub README); ECCV 2026 poster session 2026-09-10. arXiv v1: 2026-04-13.
- **Venue:** ECCV 2026, poster (ExHall #189, Thursday 2026-09-10)
- **Links:** arXiv https://arxiv.org/abs/2604.11707 · ECCV poster page https://eccv.ecva.net/virtual/2026/poster/5276 · ECCV PDF https://media.eventhosts.cc/Conferences/ECCV2026/pdfs/9706.pdf (linked from the poster page) · code https://github.com/Sta8is/Re2Pix · HF paper page https://huggingface.co/papers/2604.11707
- **Number of citations:** 1 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Re2Pix is a two-stage (hierarchical) video world model for driving. Stage 1 forecasts future frames in the frozen DINOv2 feature space with a DINO-Foresight-style masked feature transformer; stage 2 is a latent video diffusion transformer that renders future RGB frames conditioned on those predicted features. Two training tricks, nested dropout over PCA feature channels and mixed supervision (ground-truth vs predicted features), close the gap between training on clean features and inference on predicted ones. It takes 13 context frames and predicts 12 future frames.

## 1. Weights availability
Released on Hugging Face: `Sta8is/Re2Pix_cityscapes_40k` (https://huggingface.co/Sta8is/Re2Pix_cityscapes_40k), license MIT (model card), created/updated 2026-09-05. Files: `re2pix_cs_40k.pt` (video DiT, Cityscapes, 40k iterations), `re2pix_stage1.ckpt` (feature forecaster), `re2pix_stage1_dinov2_pca_448_l[2_5_8_11]_1152.pth` (PCA). Loading per `docs/Usage.md`: set paths in `cosmos_predict2/auxiliary/foresight/foresight_utils.py`, then run `python -m examples.video2world --dit_path checkpoints/Re2Pix_cs_40k/re2pix_cs_40k.pt --num_conditional_frames 13 --num_comp 1152 ...`. Only the Cityscapes model is released; multi-dataset (Cityscapes+nuScenes+CoVLA) weights were not found. The README header still says "Code and pretrained models will be released soon" although the HF weights exist.

## 2. Code
https://github.com/Sta8is/Re2Pix, MIT license, last push 2026-09-06 (GitHub API), 5 commits. Built on NVIDIA Cosmos-Predict2 and DINO-Foresight. To-do list: inference script done; training scripts NOT yet released; documentation/cleanup pending. So: inference code only, no training or evaluation scripts as of 2026-10-13.

## 3. Datasets
All frames resized to 432x768; context frames 3-15 (13), targets 16-27 (Sec. 4.1, Sec. 8).
- **Cityscapes**: 2,975 train / 500 val sequences, 30 frames, 16 fps (main setting). Public (registration).
- **nuScenes**: 1,000 scenes of 20 s at 12 fps; 750 train / 150 val. Public (registration).
- **CoVLA**: 10,000 clips of about 30 s at 20 fps; 8,000 train / 2,000 val. Public dataset (driving, Turing Inc.).
- **KITTI**: zero-shot evaluation only.
- Depth targets are pseudo-labels from Depth Anything V2.

## 4. Evaluation and downstream tasks
Metrics: FID and FVD over predicted frames (generation quality); semantic consistency via mIoU (all classes, moving objects) and depth δ1 / AbsRel computed by DINOv2 + DPT heads on generated frame 19. Baselines: a same-architecture diffusion model without semantic conditioning (782M), a parameter-matched Baseline-Large (1.5B), REPA, VideoREPA, and fine-tuned pretrained Vista and Cosmos-Predict-2.

- Cityscapes, single-dataset (Table 1), mIoU(A) / IoU(M) / AbsRel / FID / FVD: Re2Pix 63.53 / 62.29 / .1413 / 9.90 / 52.66; Baseline 60.55 / 57.64 / .1460 / 12.86 / 60.70; REPA 61.45 / 59.63 / .1465 / 12.34 / 55.15; VideoREPA 60.98 / 58.61 / .1466 / 12.95 / 59.03; Baseline-Large 61.69 / 59.65 / .1453 / 11.99 / 56.81.
- Multi-dataset training (Table 2), Cityscapes mIoU / FID / FVD: Re2Pix 64.63 / 9.29 / 49.03; Baseline 62.77 / 9.64 / 52.12; Vista 63.88 / 12.17 / 84.14; Cosmos-Predict-2 (large pretrained model, fine-tuned) 65.69 / 7.74 / 46.22.
- nuScenes (Table 2), δ1 / FID / FVD: Re2Pix 81.09 / 18.96 / 134.03; Baseline 79.92 / 21.08 / 136.71; Vista 80.46 / 21.06 / 174.86; Cosmos-Predict-2 81.28 / 15.90 / 133.19.
- KITTI zero-shot (Table 2), FID / FVD: Re2Pix 15.94 / 61.73; Baseline 20.02 / 61.02; Vista 7.95 / 97.96; Cosmos-Predict-2 8.62 / 41.64.
- Training efficiency (Fig. 3): reaches FID 15 at ~20k vs ~140k iterations for the baseline (~7x); segmentation mIoU converges ~14x faster.
- Ablations: nested dropout FID 12.63 → 9.90, FVD 58.91 → 52.66 (Table 3); 90/10 GT/predicted mixed supervision best FVD (Table 4); 256 PCA components nearly as good as 1152 (Table 5); SigLIP-2 features also work (mIoU 63.02, Table 6); sampling variance mIoU 63.70 ± 0.23 (Table 7).
- Downstream tasks: future video generation, future semantic segmentation and depth (via heads on generated frames). No planning, RL or control.

## 5. Architecture and techniques
- Semantic latent: frozen DINOv2-Reg ViT-B/14, features from blocks 3/6/9/12, PCA to 1152 channels.
- Stage 1 forecaster: DINO-Foresight masked feature transformer (12 layers, hidden 1152, SmoothL1), autoregressive at inference; trained on 8x A100 40GB.
- Stage 2 generator: DiT following the Cosmos-Predict design in the WAN2.1 VAE latent space; 14 layers, 16 heads, width 2048; cross-attention removed, AdaLN-LoRA for timestep conditioning; trained from scratch (not initialised from Cosmos weights).
- Paradigm: latent diffusion (standard denoising objective, Cosmos-Predict2 code base) conditioned on predicted features by early fusion (channel-wise sum with patchified VAE latents, no extra tokens).
- Nested dropout: feature channels truncated to one of 8 nested sizes (8 to 1152). Mixed supervision: 10% of batches use predicted features.
- Parameters: about 800M stated in Sec. 4.1; Table 1 lists Re2Pix at 1.1B total (presumably including the forecaster). The inference command uses `--model_size 2B`, which seems to be a Cosmos config flag; not reconciled.
- Compute: 40k iterations on 8x H200 (~7 h, batch 8) single dataset; 120k iterations (~28 h) multi-dataset.
- No action conditioning.

## 6. Conclusions and future work
Authors: explicitly modelling hierarchical semantic structure (representations before pixels) is an effective and scalable way to generate future frames, improving temporal semantic consistency, perceptual quality and convergence speed; gains come from the hierarchy rather than extra parameters. Future work (Sec. 10): richer VFM guidance (3D cues, RADIOv2.5, DUNE), explicit controllability (text, trajectories, scene graphs), and the CFG-style representation guidance explored in Sec. 6.2.

Own observations (not from the authors):
- A large pretrained, fine-tuned Cosmos-Predict-2 still beats Re2Pix on most Table 2 metrics, and Vista beats it on KITTI FID; the advantage is clearest against from-scratch baselines of similar size.
- KITTI zero-shot FVD is slightly worse than the baseline (61.73 vs 61.02), so the generalisation claim is only partially supported.
- No action or trajectory conditioning, so it is a passive video predictor, not yet usable for planning.
- Training code is not released, which limits reproducibility.

## 7. Sources and verification
- https://arxiv.org/html/2604.11707 (v1 full text)
- https://eccv.ecva.net/virtual/2026/poster/5276 (acceptance and poster session confirmed)
- https://github.com/Sta8is/Re2Pix (README, docs/Usage.md, To-Do list); GitHub API for push date and license
- https://huggingface.co/Sta8is/Re2Pix_cityscapes_40k (weights, via HF API)
- https://api.semanticscholar.org/graph/v1/paper/arXiv:2604.11707 (citations; S2 still lists venue as arXiv)
- Not verified: the ECCV camera-ready PDF was not read; numbers are from arXiv v1. ECCV preliminary accepted-papers list (eccv2024.ecva.net/Conferences/2026/AcceptedPapers) was truncated in fetching and did not show the paper, but the official ECCV virtual poster page does. Segmentation-label source for nuScenes evaluation is not stated in the paper.
