---
citation_count: 142
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "CVPR 2024"
date: 2023-12-29
research_group: "HKU / OpenDriveLab (Hongyang Li)"
---

# ViDAR: Visual Point Cloud Forecasting enables Scalable Autonomous Driving (borderline)

- **Authors:** Zetong Yang, Li Chen, Yanan Sun, **Hongyang Li**
- **Group role:** Hongyang Li is last author (OpenDriveLab, Shanghai AI Lab).
- **Date published:** 2024-06 (CVPR 2024). arXiv v1 2023-12-29.
- **Venue:** CVPR 2024, Highlight (according to the GitHub README and the OpenDriveLab publications page; Semantic Scholar lists the venue as CVPR).
- **Links:**
  - arXiv: https://arxiv.org/abs/2312.17655
  - Code: https://github.com/OpenDriveLab/ViDAR
- **Number of citations:** 142 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
ViDAR is a pre-training method for vision-only driving stacks. It forecasts future LiDAR point clouds from past multi-view images, conditioned on future ego-motion. The model works in three steps:

1. A BEVFormer encoder extracts BEV features from the image history.
2. A "Latent Rendering" operator turns the ray-shaped BEV features into a 3D geometric latent space.
3. An autoregressive "Future Decoder" predicts future BEV features from the previous BEV state and the next ego-motion. These are projected to an occupancy volume and rendered into point clouds.

The paper's FAQ states that ViDAR "could be deemed as a world model": it predicts the future conditioned on observations and actions. It is kept as borderline because its main purpose is encoder pre-training rather than simulation or planning.

## 1. Weights availability
- Pre-trained and fine-tuned checkpoints are on GitHub Releases (v1.0.0). Examples: `pretrain-ViDAR-RN101-nus-full-1future.pth`, BEVFormer fine-tunes on 25% and 100% of nuScenes, and ViDAR-UniAD (2024-04).

## 2. Code
- https://github.com/OpenDriveLab/ViDAR, Apache-2.0, about 354 stars. It is based on mmDet3D and includes pre-training and fine-tuning configs for BEVFormer, OccNet and UniAD.

## 3. Datasets
- nuScenes (Image-LiDAR sequences) for both pre-training and downstream tasks. The authors suggest scaling to more Image-LiDAR datasets, but no OpenScene or nuPlan training was found in this paper version.

## 4. Evaluation and downstream tasks
**Point cloud forecasting against 4D-Occ, which uses LiDAR input (Table 2, Chamfer distance in m², lower is better):**

| Input history | Model | 0.5 s | 1.0 s | 1.5 s | 2.0 s | 2.5 s | 3.0 s |
|---|---|---|---|---|---|---|---|
| 1 s | 4D-Occ | 1.26 | 1.88 | – | – | – | – |
| 1 s | ViDAR | 1.11 | 1.25 | 1.40 | 1.57 | 1.76 | 1.97 |
| 3 s | 4D-Occ | 0.91 | 1.13 | 1.30 | 1.53 | 1.72 | 2.11 |
| 3 s | ViDAR | 1.01 | 1.12 | 1.25 | 1.38 | 1.54 | 1.73 |

ViDAR's error at 1 s is about 33% lower with 1 s of history, and about 18% lower at 3 s with 3 s of history. It is worse than 4D-Occ at the 0.5 s horizon in the 3 s setting.

**Downstream pre-training:**
- **BEVFormer detection (Table 3, NDS):**
  - RN101 nuScenes 3D-det initialization: 51.7 → 54.8.
  - Intern-B COCO initialization: 52.0 → 57.6.
- **Label efficiency:** with half of the 3D labels, BEVFormer reaches 39.4 mAP, against 37.7 mAP fully supervised without ViDAR.
- **UniAD (abstract and Tables 7–9):** +3.1% NDS on detection, about 10% lower minADE on motion forecasting, and about 15% lower average collision rate (0.27 → 0.23) on planning, with planning L2 0.21 m lower. Map segmentation, tracking and future occupancy also improve.

## 5. Architecture and techniques
- **Encoder:** BEVFormer-base (ResNet101-DCN with FPN and 6 BEV encoder layers), 200×200 BEV.
- **Latent Rendering:** 16 groups of 16 channels, emulating volume rendering in latent space.
- **Future Decoder:** 6 layers, each with deformable self-attention, temporal cross-attention using ego-motion-aligned reference points, and a feed-forward network. The ego-motion is MLP-embedded into the queries.
- **Output:** a 200×200×16 occupancy volume. Point clouds come from differentiable ray-casting with a ray-wise cross-entropy loss.
- **Training:** 5 history frames; 6 autoregressive steps predict 3 s at 0.5 s intervals, with loss on 1 random future per step. 50 epochs with AdamW at learning rate 2e-4.

## 6. Conclusions and future work
- **Authors:** visual point cloud forecasting is a scalable pre-training task that jointly captures semantics, 3D geometry and dynamics. They suggest using ViDAR as a simulator for model-based RL and scaling to more Image-LiDAR data.
- **Own observation:**
  - Action conditioning (ego-motion) is present, but the paper never uses ViDAR for planning by imagination or for policy evaluation.
  - All results are on nuScenes and open-loop.
  - It is the earliest action-conditioned predictive model from this group and fed into the CVPR 2024 "Predictive World Model" challenge.

## 7. Sources and verification
- **Read:** full arXiv v1 PDF (pdftotext), GitHub README and API metadata, OpenDriveLab publications page, Semantic Scholar API.
- **Not verified:** the CVF open-access page returned 403, so the Highlight status rests on the README and the lab page.
