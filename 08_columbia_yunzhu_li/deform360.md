---
citation_count: 1
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; 2026-10-15 re-check returned HTTP 429)"
venue: "ECCV 2026"
date: 2026-07-06
research_group: "Columbia (Yunzhu Li)"
---

# Deform360: A Massive Multi-view Visuotactile Dataset for Deformable World Models

- **Authors:** Hongyu Li\*, Wanjia Fu\*, Xiaoyan Cong, Zekun Li, Binghao Huang, Hanxiao Jiang, Xintong He, Yiqing Liang, Rao Fu, Tao Lu, Srinath Sridhar, Kevin A. Smith, George Konidaris, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author, affiliated with Columbia. This is joint work with Brown (Konidaris, Sridhar) and MIT (Kevin A. Smith); neither Brown PI is a summary PI.
- **Date published:** 2026-09 (ECCV 2026; I did not verify the conference dates). arXiv v1 2026-07-06.
- **Venue:** European Conference on Computer Vision (ECCV) 2026. The title appears in the ECCV 2026 accepted-papers listing at https://eccv.ecva.net/virtual/2026/papers.html (checked 2026-10-15). The arXiv comment reads "Accepted by ECCV 2026". Presentation type not found.
- **Links:** arXiv https://arxiv.org/abs/2607.05390 · Project https://deform360.lhy.xyz · Code https://github.com/lhy0807/deform360 · Dataset https://huggingface.co/datasets/brownu/deform360
- **Number of citations:** 1 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
Deform360 is a large real-world benchmark for deformable-object world models. It has two parts:
- **Data:** 198 everyday objects and 1,980 interactions (5 unimanual and 5 bimanual per object), recorded with 41 surround cameras and bimanual tactile UMI grippers.
- **Annotation:** a markerless visuotactile pipeline that produces dense 3D particle tracks. It uses per-frame 3DGS geometry, CoTracker3 tracks lifted to 3D and fused with RANSAC, and optimisation with shape, ARAP, Laplacian and tactile no-slip losses.

The authors use it to benchmark 3D particle world models (PGND, ParticleFormer, PhysTwin) against a 2D action-conditioned video world model (Cosmos-Predict 2.5 2B, post-trained with actions). They also show contact prediction from vision and MPC planning on a different robot.

## 1. Weights availability
Not released. The repository README states: "World-model baselines, training code, and pretrained world-model checkpoints are not released." Only the dataset and annotations are released (Hugging Face `brownu/deform360`, MIT license per the HF API, last modified 2026-08-20).

## 2. Code
- https://github.com/lhy0807/deform360, with a LICENSE file present (type not inspected). It contains:
  - Dataset access.
  - Camera undistortion and alignment.
  - Tactile processing and alignment.
  - The annotation pipeline (`deform360-annotate`: SAM3 masks, ArUco poses, Splatfacto reconstruction, rendered depth, CoTracker tracking, PhysTwin control-point inputs).
- There is no world-model training or evaluation code (see above).

## 3. Datasets
- **Deform360 (this paper):**
  - 198 objects: 28 one-dimensional, 98 two-dimensional and 72 three-dimensional volumetric deformables.
  - 1,980 sequences (about 10.3 s each).
  - 41 views at 720p and 30 FPS; 74,850 raw videos, about 23.3M frames, 215.7 hours of cumulative multi-view footage.
  - Tactile: four 16×32 sensor streams.
  - Public on Hugging Face (MIT).
- **Evaluation splits:**
  - Per-episode: train on early frames, test on later frames.
  - Multi-episode: unseen episodes of the same object.
  - Multi-object: zero-shot on unseen objects.

## 4. Evaluation and downstream tasks
- **Annotation quality:**
  - Reconstruction on held-out views (Table 2), global average: PSNR 27.66, SSIM 0.96, LPIPS 0.0708.
  - Visuotactile tracking reaches Chamfer distance 2.71×10⁻⁵ m², against 1.41×10⁻⁴ m² for vision-only tracking (Sec. 5.1).
- **Contact prediction from vision:** 88.67% accuracy (chance 50.31%), F1 0.8909, over 36 views (Sec. 5.2).
- **World-model benchmark, future prediction:**

  | Setting | Model | CD ↓ | Track error ↓ | PSNR ↑ | LPIPS ↓ |
  |---|---|---|---|---|---|
  | Per-episode (Table 3) | PhysTwin | 0.014 | 0.025 | 26.57 | 0.047 |
  | Per-episode (Table 3) | ParticleFormer | 0.044 | 0.041 | 26.29 | 0.047 |
  | Per-episode (Table 3) | PGND | 0.073 | 0.073 | 25.30 | 0.050 |
  | Multi-episode (Table 4) | ParticleFormer | 0.051 | 0.079 | 25.20 | 0.038 |
  | Multi-episode (Table 4) | PGND | 0.130 | 0.144 | 23.79 | 0.040 |
  | Multi-episode (Table 4) | Cosmos | — | — | 24.95 | 0.043 |
  | Multi-object, zero-shot (Table 5) | Cosmos | — | — | 25.04 | 0.037 |
  | Multi-object, zero-shot (Table 5) | ParticleFormer | 0.038 | 0.048 | 23.31 | 0.038 |
  | Multi-object, zero-shot (Table 5) | PGND | 0.429 | 0.320 | 22.05 | 0.041 |

  - Per-episode: Cosmos is not included because there is too little data per episode.
  - Multi-episode: in reconstruction, Cosmos has the best PSNR (27.75).
  - Multi-object: Cosmos's main failure is not following actions over long horizons.
  - Per-cell values were reconstructed from extracted table text, ordered by row.
- **Key finding (authors):** 3D particle and physics models win in low-data regimes thanks to structural priors. Large pretrained video models generalise better zero-shot to new objects, but are harder to use for planning because rewards on generated video are difficult to design.
- **Robot planning (Sec. 5.4):** PhysTwin representations learned from Deform360 are used in MPC on a different xArm setup in another lab. This is qualitative and preliminary.
- **Downstream tasks:** future prediction across generalisation levels, contact prediction and MPC planning.

## 5. Architecture and techniques
- **Capture:** the BRICS dome at Brown, with 41 calibrated RGB cameras (ArUco grid), UMI grippers with normal-pressure tactile arrays, and ArUco-based gripper pose tracking with RANSAC.
- **Annotation:**
  - Per-frame 3DGS on masked objects (L1+SSIM, λ = 0.2).
  - CoTracker3 on up to 1,600 points over 15-frame clips with stride 5, as in PGND.
  - Depth lifting from the 3DGS render and RANSAC fusion into a velocity field.
  - Optimisation of L_shape + 20·L_ARAP + 0.1·L_Laplacian + 1.0·L_tactile.
- **Benchmarked models:**
  - PGND (particle-grid neural dynamics).
  - ParticleFormer (transformer particle dynamics).
  - PhysTwin (spring-mass system identification).
  - Cosmos-Predict 2.5 2B (video diffusion, post-trained with 6-D wrist pose plus gripper action conditioning).
- No new world-model architecture is proposed.

## 6. Conclusions and future work
- **Authors:** Deform360 is a foundational benchmark. It exposes a trade-off between structural priors (3D) and scalability with pretraining (2D video), and reward design for video models is still an open problem.
- **Authors' limitations:**
  - Heavy self-occlusion degrades tracking.
  - Highly plastic materials break the rigidity and smoothness assumptions.
  - Slip violates the tactile no-slip prior, and normal-only tactile sensing cannot detect micro-slip.
  - They plan richer tactile sensing and more extreme materials.
- **My own observations:**
  - This is mainly a dataset and benchmark paper; the world models are the group's earlier ones or third-party models.
  - No world-model code or weights are released, so the benchmark numbers are not directly reproducible.
  - The planning demo is qualitative.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2607.05390 (v1, full main text read).
- ECCV 2026 accepted-papers listing: https://eccv.ecva.net/virtual/2026/papers.html (title match).
- Code README: raw.githubusercontent.com (lhy0807/deform360).
- Hugging Face dataset API: https://huggingface.co/api/datasets/brownu/deform360
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - ECCV 2026 conference dates and presentation type.
  - The GitHub LICENSE type.
  - Exact table cell alignment (reconstructed).
