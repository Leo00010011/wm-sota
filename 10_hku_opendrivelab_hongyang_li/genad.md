---
citation_count: 197
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "CVPR 2024"
date: 2024-03-14
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# GenAD: Generalized Predictive Model for Autonomous Driving

- **Authors:** Jiazhi Yang*, Shenyuan Gao*, Yihang Qiu*, Li Chen†, Tianyu Li, Bo Dai, Kashyap Chitta, Penghao Wu, Jia Zeng, Ping Luo, Jun Zhang♮, Andreas Geiger♮, Yu Qiao♮, **Hongyang Li**†
  - *Equal contribution. †Project lead (Li Chen and Hongyang Li). ♮Equal co-advising (Zhang, Geiger, Qiao).
  - Primary contact: Jiazhi Yang.
- **Group role:** Hongyang Li is last author and project lead (OpenDriveLab, Shanghai AI Lab at the time). Co-authors come from HKUST, HKU and Tübingen.
- **Date published:** 2024-06 (CVPR 2024). arXiv v1 2024-03-14, v2 2024-08-08.
- **Venue:** CVPR 2024, Highlight (arXiv comment and DriveAGI README).
- **Links:**
  - arXiv: https://arxiv.org/abs/2403.09630
  - Dataset and toolkit: https://github.com/OpenDriveLab/DriveAGI (OpenDV section)
  - Language annotations: https://huggingface.co/datasets/OpenDriveLab/OpenDV-YouTube-Language
- **Number of citations:** 197 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
GenAD is presented as the first large-scale video prediction model for autonomous driving. The authors build OpenDV-2K, a 2,059-hour text-paired driving video corpus: 1,747 h of curated YouTube front-camera video (OpenDV-YouTube) plus 312 h from 7 public datasets. They lift SDXL into a video predictor in two stages:

1. Fine-tune SDXL on driving frames (image domain transfer).
2. Insert "temporal reasoning blocks" and train them for future prediction conditioned on past frames and text.

The pre-trained predictor is then extended to trajectory-conditioned prediction (GenAD-act) and to open-loop planning through a small MLP on frozen features.

## 1. Weights availability
- No GenAD model weights were found. The DriveAGI repo releases only the dataset (video list, download and processing scripts, language annotations, and the OpenDV-mini subset).
- The successor model, Vista, is trained on OpenDV-YouTube and does release weights.

## 2. Code
- No official GenAD training or inference code was found. https://github.com/OpenDriveLab/DriveAGI (Apache-2.0, about 808 stars) holds the OpenDV toolkit.

## 3. Datasets
- **OpenDV-YouTube:** 1,747 h from 2,139 videos by 43 YouTubers, covering more than 40 countries and 244 cities. Context captions come from BLIP-2 and command labels from a classifier. Validation uses 3 held-out YouTubers. The video list is released under YouTube terms, and OpenDV-mini (25 h) is also available.
- **OpenDV-2K:** OpenDV-YouTube plus 312 h from 7 public datasets (including nuScenes, Waymo Open, Argoverse 2, nuPlan, Honda-HAD/HDD and Talk2Car), with GPT-refined text. Annotations for the public-dataset part are not released.
- **Downstream:** nuScenes for action-conditioned prediction and planning. Zero-shot tests use Waymo, KITTI and Cityscapes.

## 4. Evaluation and downstream tasks
**Video prediction on nuScenes (Table 2):**

| Method | Training data | FID ↓ | FVD ↓ |
|---|---|---|---|
| DriveGAN | nuScenes | 73.4 | 502 |
| DriveDreamer (needs 3D layout) | nuScenes | 52.6 | 452 |
| DrivingDiffusion (needs 3D layout) | nuScenes | 15.8 | 332 |
| GenAD-nus | nuScenes | 15.4 | 244 |
| GenAD | OpenDV-2K | 15.4 | 184 |

- **FVD:** 44.5% lower than DrivingDiffusion, without 3D layout inputs.
- **Zero-shot generation:** qualitative only (Fig. 5) on held-out YouTube, Waymo, KITTI and Cityscapes, against I2VGen-XL, VideoCrafter1 and DMVFN.
- **Ablation (Table 3, OpenDV-YouTube):**
  - Interleaving temporal and spatial blocks (deep interaction) improves FVD from 244.4 to 201.7.
  - The full model reaches FID 17.67, FVD 189.5 and CLIPSIM 0.865.
- **Action-conditioned prediction (Table 4):** the action prediction error, measured by an inverse dynamics model, falls from 2.54 (text-only) to 2.02 with the trajectory condition, against 0.90 for ground-truth video. This is a 20.4% reduction.
- **Open-loop planning (Table 5, nuScenes, UniAD protocol):**
  - A 0.8M-parameter MLP on frozen GenAD features reaches ADE 1.23 and FDE 2.31, using front view only.
  - This compares with UniAD's 1.03 / 1.65 from multi-view input with 58.8M trainable parameters, and ST-P3's 2.65 / 3.73.
  - Training takes 10 minutes on one V100, which the authors call "3400× more efficient" than training UniAD's planner.

## 5. Architecture and techniques
- **Backbone:** SDXL latent diffusion UNet. The model has 5.9B parameters in total; stage 1 trains 2.7B (UNet) and stage 2 trains 2.5B.
- **Temporal reasoning block:** causal temporal attention plus decoupled spatial attention (separate horizontal and vertical axial attention, linear cost). These blocks are interleaved with the SDXL spatial, cross-attention and feed-forward blocks, and their final layers are zero-initialized. Stage 2 freezes the SDXL weights.
- **Training objective:** denoise future latents conditioned on clean past latents and text.
- **Training setup:** clips of 4 s at 2 Hz, resolution 256×448, with 10% text dropout for classifier-free guidance.
- **Compute:**
  - Stage 1: 300K iterations on 32 A100 GPUs, batch size 256.
  - Stage 2: 112.5K iterations on 64 GPUs, batch size 64.
  - GenAD-act: 100K steps on 16 GPUs.
- **Action conditioning:** trajectory Fourier embeddings are added to the cross-attention conditions.

## 6. Conclusions and future work
- **Authors:** web-scale driving video plus language makes a generalizable predictive model whose features transfer to planning. Limitations are training efficiency and real-time deployment because of model size. Future directions are video prediction as a scalable objective for representation and policy learning, and distillation.
- **Own observation:**
  - The prediction runs at only 2 Hz and 256×448.
  - Action control was fine-tuned only on nuScenes.
  - The planning result is open-loop L2 and does not beat UniAD.
  - The model's main lasting contribution is the OpenDV dataset, since the weights were never released.

## 7. Sources and verification
- **Read:** full arXiv v2 PDF (pdftotext), DriveAGI GitHub README and API metadata, Semantic Scholar API.
- **Not verified:** the CVPR open-access page (it returned 403), and whether OpenDV-2K's public-dataset portion is exactly seven datasets with the names listed above (taken from Table 1 and the appendix prompts).
