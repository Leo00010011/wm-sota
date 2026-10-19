---
citation_count: 61
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "ICLR 2026"
date: 2025-09-15
research_group: "Shanghai AI Lab (Physical Intelligence Team)"
---

# OmniWorld: A Multi-Domain and Multi-Modal Dataset for 4D World Modeling

- **Authors:** Yang Zhou, Yifan Wang, Jianjun Zhou, Wenzheng Chang, Haoyu Guo, Zizun Li, Kaijing Ma, Xinyue Li, Yating Wang, Haoyi Zhu, Mingyu Liu, Dingning Liu, Jiange Yang, Zhoujie Fu, Junyi Chen, **Chunhua Shen**, **Jiangmiao Pang**, **Kaipeng Zhang**, **Tong He** (Shanghai AI Laboratory; Zhejiang University for J. Zhou, M. Liu and C. Shen)
- **Group role:** Shanghai AI Lab is the senior affiliation; Tong He is the last and corresponding author (per his homepage). Haoyu Guo and Chunhua Shen are contributors to the Physical Intelligence Team's report 2607.06401. Jianjun Zhou, Zizun Li, Xinyue Li and Yating Wang are also on InternW0-Δ. The dataset is hosted under the InternRobotics organization.
- **Date published:** 2026-04 (ICLR 2026; accepted 2026-01-26 per the README; arXiv v1 2025-09)
- **Venue:** ICLR 2026, main conference poster
- **Links:** arXiv https://arxiv.org/abs/2509.12201 · ICLR https://iclr.cc/virtual/2026/poster/10011787 · Proceedings https://proceedings.iclr.cc/paper_files/paper/2026/hash/6d7d9b266865cc83d1f12bb8e4c2d0f9-Abstract-Conference.html · Project https://yangzhou24.github.io/OmniWorld/ · Code https://github.com/yangzhou24/OmniWorld · Data https://huggingface.co/datasets/InternRobotics/OmniWorld (also on ModelScope)
- **Number of citations:** 61 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
OmniWorld is a dataset and benchmark for 4D world modelling (geometry plus dynamics). It has a new synthetic game subset, OmniWorld-Game (96K clips, 214 h, 720p, with full annotations), and re-annotates 11 public robot, human and internet video datasets with depth, camera pose, captions, optical flow and foreground masks. In total it covers more than 4,000 hours, 600K sequences and 300M frames. Its benchmark shows that state-of-the-art 3D-geometry and camera-controlled video models struggle, and that fine-tuning on OmniWorld improves them.

## 1. Weights availability
- Not applicable as a main deliverable: this is a dataset and benchmark paper. No new world-model checkpoint is introduced. The fine-tuned baselines (DUSt3R*, CUT3R*, Reloc3r*, AC3D*) are not listed as released. The README points to Pi3X (https://github.com/yyfz/Pi3), a related geometry model trained with the metric-scale OmniWorld-Game.

## 2. Code
- https://github.com/yangzhou24/OmniWorld: 499 stars, last push 2026-04-16 (GitHub API, checked 2026-10-15). The repo shows no license.
- Provides download instructions (`hf download InternRobotics/OmniWorld`), data loaders and visualization for each modality, and the OmniWorld-Game benchmark (released on Hugging Face 2025-10-15).

## 3. Datasets
- Hugging Face `InternRobotics/OmniWorld`: CC BY-NC-SA 4.0 (dataset card), webdataset format, 59,875 downloads and 100 likes (HF API, 2026-10-15). Game content is non-commercial only, under the publishers' terms.
- Release status (README table): OmniWorld-Game 11k of 96k clips released (metric-scale version added 2026-03-21). DROID, RH20T, RH20T-Human, HOI4D, Epic-Kitchens, Ego-Exo4D, EgoDex and CityWalk annotations are released in full. AgiBot, HoloAssist and Assembly101 are still "planned".
- Annotation: game depth captured at render time through ReShade; AgiBot/HOI4D depth refined with Prior Depth Anything; DROID depth from FoundationStereo. Camera poses come from VGGT or DroidCalib, then tracking (SIFT, SuperPoint, CoTracker3) and bundle adjustment. Captions from Qwen2-VL-72B (150-250 tokens, per 81-frame segment). Flow from DPFlow. Masks from RoboEngine + SAM 2 (robot) and Grounding DINO + SAM (game).

## 4. Evaluation and downstream tasks
- 3D geometric prediction on OmniWorld-Game. Monocular depth: MoGe-2 is best (Abs Rel 0.401, δ<1.25 0.589). Video depth (scale & shift): VGGT is best (Abs Rel 0.194, δ 0.755, 18.75 FPS); MASt3R is close (0.217) but runs at 0.79 FPS. Also evaluated: DUSt3R, MonST3R, Fast3R, CUT3R, FLARE.
- Camera-controlled video generation on OmniWorld-Game: CamCtrl (I2V) is best (TransErr 1.29, RotErr 0.20, CamMC 1.39, FVD 615). AC3D (T2V) gets TransErr 6.28, FVD 1746. MotionCtrl and CAMI2V are also evaluated.
- Fine-tuning gains: DUSt3R* Sintel monocular Abs Rel 0.488 to 0.370 (NYU-v2 slightly worse, 0.081 to 0.089). CUT3R* Sintel video depth 0.537 to 0.314, ATE 0.210 to 0.178 (Bonn monocular worse, 0.058 to 0.075). Reloc3r* DynPose-100K AUC@5 6.9 to 14.4. AC3D* OmniWorld-Game TransErr 6.28 to 4.14, FVD 1746 to 1437; RealEstate10K TransErr 3.44 to 2.86.
- No action-conditioned (robot-action) world model or control evaluation.

## 5. Architecture and techniques
- No new model architecture. The contribution is the data pipeline: game capture with UI removal (ReShade), two-stage camera estimation with bundle adjustment on static regions, and multi-source re-annotation.
- Scale: OmniWorld-Game has 18,515K frames at 1280x720 and 24 FPS. Human video is the largest share of the whole dataset.

## 6. Conclusions and future work
- Authors: current models have "considerable room for improvement" in complex 4D environments; OmniWorld is meant as a resource for training and evaluating general-purpose 4D world models. The paper has no dedicated limitations section.
- Own observation: this is a world-modelling data/benchmark paper, not a world model. "4D world modeling" here means geometry plus camera-controlled generation, with no agent actions. Several promised subsets (including the largest robot one, AgiBot) are still unreleased about a year after publication, and most of the game subset is unreleased. Some fine-tuning results are mixed across benchmarks.

## 7. Sources and verification
- https://arxiv.org/html/2509.12201 (full paper, read through a summarizing fetch)
- ICLR 2026 virtual poster page and proceedings entry (found by search); README news entry "accepted by ICLR 2026"
- GitHub README and API; Hugging Face dataset API; Tong He's homepage (corresponding author)
- Not verified: the exact ICLR presentation type beyond "poster"; the dataset license was taken from the HF card tags, since the repo has no license file.
