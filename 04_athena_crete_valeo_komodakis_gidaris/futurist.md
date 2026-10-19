---
citation_count: 12
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check 2026-10-15 returned HTTP 429)"
venue: "CVPR 2025"
date: 2025-01-14
research_group: "Athena / Univ. of Crete + valeo.ai (Nikos Komodakis, Spyros Gidaris)"
---

# Advancing Semantic Future Prediction through Multimodal Visual Sequence Transformers (FUTURIST)

- **Authors:** Efstathios Karypidis, Ioannis Kakogeorgiou, **Spyros Gidaris**, **Nikos Komodakis**
- **Group role:** All authors from the group (Archimedes/Athena RC and valeo.ai). Komodakis is senior (last) author.
- **Date published:** 2025-06 (CVPR 2025; poster session 2025-06-13). arXiv v1: 2025-01-14.
- **Venue:** CVPR 2025, poster (ExHall D #345), proceedings pp. 3793-3803
- **Links:** arXiv https://arxiv.org/abs/2501.08303 · proceedings https://openaccess.thecvf.com/content/CVPR2025/html/Karypidis_Advancing_Semantic_Future_Prediction_through_Multimodal_Visual_Sequence_Transformers_CVPR_2025_paper.html · CVPR poster https://cvpr.thecvf.com/virtual/2025/poster/32474 · project page https://futurist-cvpr2025.github.io/ · code https://github.com/Sta8is/FUTURIST
- **Number of citations:** 12 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
FUTURIST is a semantic world model for driving video: a multimodal masked visual sequence transformer that takes 4 past frames of semantic segmentation and depth maps and predicts the next frame's segmentation and depth jointly. It uses a VAE-free tokenizer (learned per-pixel embeddings plus patch projection), so the whole model trains end-to-end at high resolution. Longer horizons are reached by autoregressive rollout.

## 1. Weights availability
Released on Hugging Face: `Sta8is/FUTURIST`, file `futurist.ckpt` (https://huggingface.co/Sta8is/FUTURIST/resolve/main/futurist.ckpt), plus `config.json`. License MIT (HF model card). Load by passing `--ckpt futurist.ckpt --evaluate --eval_ckpt_only` to `train_futurist.py` (README). Precomputed Cityscapes segmentation/depth pseudo-labels are also offered via Google Drive links in the README.

## 2. Code
https://github.com/Sta8is/FUTURIST, MIT license, 5 commits, last push 2025-08-19 (GitHub API). Contains training (`train_futurist.py`, example: 8 GPUs, fp16, up to 3200 epochs, `segmaps_depth` modality, concat fusion) and evaluation code, plus data preparation instructions (Segmenter for segmentation, DepthAnythingV2 for depth, cityscapesScripts for gtFine).

## 3. Datasets
- **Cityscapes** only: 2,975 train / 500 val / 1,525 test sequences, 30 frames each at 16 fps, 1024x2048; 19-class labels on frame 20. Train on train split, evaluate on val. Public (registration): https://www.cityscapes-dataset.com/
- Training inputs/targets are pseudo-labels from Segmenter (segmentation) and DepthAnythingV2 (depth); evaluation of segmentation uses gtFine, depth uses DepthAnythingV2 pseudo-ground-truth.

## 4. Evaluation and downstream tasks
Metrics: mIoU over all classes (ALL) and movable objects (MO) for segmentation; δ1 and AbsRel for depth. Horizons: short (~0.18 s) and mid (~0.54 s). Baselines: PFA, F2MF, VISTA fine-tuned (video diffusion world model, labels extracted from generated RGB), Copy-Last, Oracle.

- Future semantic segmentation (Table 1), short ALL/MO, mid ALL/MO: FUTURIST 73.9 / 74.9, 62.7 / 61.2; PFA (prior SOTA) 71.1 / 69.2, 60.3 / 56.7; F2MF 69.6 / 67.7, 57.9 / 54.6; VISTA-ft 65.7 / 64.1, 53.2 / 49.1; Copy-Last 55.5 / 52.7, 40.5 / 32.2; Oracle 78.6 / 80.8.
- Future depth (Table 6, 3200 epochs), δ1 / AbsRel: short 96.0 / 5.384, mid 91.9 / 9.111; VISTA-ft 93.4 / 8.134 and 87.4 / 12.923.
- Multimodal vs single-modality (Table 1, Table 2): segmentation-only model 73.4 / 62.4 ALL; depth-only (800 ep.) δ1 95.4 / 90.6 vs 95.8 / 91.5 multimodal.
- Ablations: VAE-free tokenization 72.9 ALL short vs 60.7 with an LDM VQ-VAE and 69.2 with a fine-tuned VQ-VAE (Table 7); "partially-shared + exclusive" masking best (Table 4); concat fusion slightly better than add (Table 3); fused tokens halve training time vs separate per-modality tokens (Table 8).
- No planning, RL or control downstream tasks.

## 5. Architecture and techniques
- Backbone: 12-layer transformer, hidden size 1536, factorised temporal and spatial attention, Pre-LN, built on a PyTorch MaskGIT reproduction.
- Tokenizer: VAE-free; each pixel's class/depth bin mapped to a 10-d learned embedding, 16x16 patches flattened and linearly projected; 512 tokens per frame at 256x512; modalities fused by channel concatenation.
- Paradigm: masked token prediction (MaskGIT-style), masked cross-entropy over future tokens per modality (Eqs. 1-2), weight-tied softmax heads. Deterministic single-step prediction; autoregressive rollout for longer horizons.
- Sequence: 5 frames (4 context, 1 future).
- Parameters: about 465M (558M for the VQ-VAE variant) (Appendix A.2).
- Training: Adam, lr 1.6e-4 cosine, batch 64, 8x A100 40GB; 800 epochs ≈ 8 h, 3200 epochs ≈ 32 h.
- No action conditioning, no planner.

## 6. Conclusions and future work
Authors: an end-to-end, VAE-free multimodal masked transformer reaches state-of-the-art Cityscapes future segmentation at short and mid horizons, and multimodal training beats single-modality training. Future work (Appendix C): handle noisy predicted context in rollouts (training uses teacher forcing), add stochasticity for uncertainty, extend to instance/panoptic segmentation, add action conditioning for a controllable world model, scale model/data/modalities, reduce reliance on pretrained pseudo-labelers.

Own observations (not from the authors):
- Operates on low-dimensional label maps (segmentation + depth), so it forecasts semantics but discards appearance; DINO-Foresight later replaces label maps with foundation-model features.
- Single dataset (Cityscapes) and pseudo-labelled training data; generalisation to other driving datasets is not shown.
- Deterministic; no uncertainty over multiple futures.

## 7. Sources and verification
- https://arxiv.org/html/2501.08303 (v2, full text incl. appendix)
- https://openaccess.thecvf.com/content/CVPR2025/html/Karypidis_Advancing_Semantic_Future_Prediction_through_Multimodal_Visual_Sequence_Transformers_CVPR_2025_paper.html (proceedings; page numbers via project page and bibtex)
- https://cvpr.thecvf.com/virtual/2025/poster/32474 (poster session, via search result)
- https://futurist-cvpr2025.github.io/, https://github.com/Sta8is/FUTURIST, https://huggingface.co/Sta8is/FUTURIST
- https://api.semanticscholar.org/graph/v1/paper/arXiv:2501.08303 (citations)
- Not verified: whether the CVPR camera-ready numbers differ from arXiv v2 (v2 was revised Nov 2025, after the conference); numbers above are from arXiv v2.
