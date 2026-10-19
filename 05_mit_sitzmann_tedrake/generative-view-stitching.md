---
citation_count: 3
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; re-check returned HTTP 429)"
venue: "ICLR 2026"
date: 2025-10-28
research_group: "MIT (Vincent Sitzmann, Russ Tedrake)"
---

# Generative View Stitching

- **Authors:** Chonghyuk Song, Michal Stary, Boyuan Chen, George Kopanas, **Vincent Sitzmann**
- **Group role:** Senior author (Sitzmann last author). First author is an MIT CSAIL student; Boyuan Chen is co-author. Tedrake is not an author.
- **Date published:** 2026-04 (ICLR 2026; the camera-ready arXiv v3 is dated 2026-04-09); arXiv v1 2025-10-28
- **Venue:** ICLR 2026, poster (the arXiv comment says "Published at ICLR 2026. Camera-ready Submission"; ICLR virtual poster page 10008201)
- **Links:** arXiv https://arxiv.org/abs/2510.24718 · ICLR poster https://iclr.cc/virtual/2026/poster/10008201 · Project https://andrewsonga.github.io/gvs · Code https://github.com/andrewsonga/generative_view_stitching
- **Number of citations:** 3 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
GVS is a training-free sampling algorithm for camera-guided video generation along a predefined camera trajectory. It uses an off-the-shelf Diffusion Forcing video model (the DFoT RealEstate10K model) as the world simulator. Instead of autoregressive rollout, it denoises all chunks of a long video in parallel, conditioning each chunk on its past and future neighbours. This avoids "collisions" with generated scene content and allows loop closure. "Omni Guidance" and cyclic conditioning add temporal and long-range consistency. This is a borderline world-model paper: it is about planning-aware sampling of a camera-pose-conditioned video model, not a new dynamics model.

## 1. Weights availability
- No new weights. GVS is training-free and uses the public DFoT RealEstate10K checkpoint (https://huggingface.co/kiwhansong/DFoT, MIT). The GVS README says "pretrained DFoT models" are used but does not state the download source explicitly.
- Evaluation also uses Video-Depth-Anything weights (via `third_party/Video-Depth-Anything/get_weights.sh`) for collision detection.

## 2. Code
- https://github.com/andrewsonga/generative_view_stitching. It is forked from Boyuan Chen's research template; the LICENSE file is not named on the page (template license requires keeping attribution; the DFoT/DF repos from the same template are MIT, so this is likely MIT but not verified).
- 6 commits; includes `get_benchmark.sh` (benchmark camera trajectories), a MEt3R submodule for the consistency metrics, and experiment configs to reproduce all results (`configurations/shortcut/experiment/README.md`).
- No training code needed (sampling-only method). Last commit date not visible.

## 3. Datasets
- **Backbone training data:** RealEstate10K (via DFoT; public).
- **Evaluation:** an author-built benchmark of 7 predefined camera trajectories (Panorama 1-loop and 2-loop, Circle 1-loop and 2-loop, Straight line, Stairs, Staircase circuit). Downloadable with `get_benchmark.sh`. 40 generations per trajectory (Table 1).

## 4. Evaluation and downstream tasks
- **Metrics:** frame-to-frame consistency (F2FC, MEt3R cosine, lower is better), long-range/loop consistency (LRC, MEt3R), collision avoidance (CA, fraction of videos with metric depth below a threshold, via Video Depth Anything), VBench imaging quality (IQ) and aesthetic quality (AQ), and Inception Score in ablations.
- **Baselines:** history-guided autoregressive sampling (DFoT, with FOV-based retrieval memory for loops) and StochSync (with GVS loop closing). All use the same DFoT RE10K model with an 8-frame context.
- **Key numbers (Table 1):**
  - Straight line: F2FC 0.080 (GVS) vs 0.138 (AR) and 0.124 (StochSync); CA 0 vs 0.325 (AR).
  - Staircase circuit: LRC 0.176 vs 0.449 (AR) and 0.221 (StochSync); CA 0 vs 0.625 (AR).
  - Panorama 2-loop: LRC 0.116 vs 0.171 (AR).
  - IQ/AQ are comparable; StochSync has higher AQ on several trajectories but gets zero collisions by "shape-shifting" the scene.
- **Ablations:** Omni Guidance with partial stochasticity η=0.9 gives the best quality/consistency trade-off (Table 2). Without loop closing, LRC is about 0.95 on Panorama 1-loop; with loop closing plus Omni Guidance it is 0.141 (Table 3).
- **Downstream application:** a 120-frame navigation video through an "Impossible Staircase" (Fig. 7). No robotics, RL or planning-for-control tasks are evaluated.

## 5. Architecture and techniques
- **Backbone:** the DFoT pixel-space U-ViT video diffusion model with camera-pose conditioning (RE10K, 8-frame context), unchanged.
- **Stitching:**
  - The target video is split into non-overlapping chunks. Each chunk is jointly denoised with its (noisy) temporal neighbours inside the context window.
  - Only the target chunk's output is kept. This approximates a compositional distribution ∏ p(x_t | x_{t−1}, x_{t+1}, poses) (Eq. 3), extending CompDiffuser-style planning stitching to video.
- **Omni Guidance (Eq. 7–8):** a CFG-like guidance that strengthens conditioning on past and future neighbours by contrasting with a score where neighbours are replaced by pure noise. It is a generalisation of fractional history guidance, enabled by Diffusion Forcing's per-token noise levels.
- **Partial stochasticity** (η∈(0,1)) in DDIM-style sampling.
- **Loop closing via cyclic conditioning:** alternates temporal windows with manually defined "spatial windows" that group temporally distant but spatially close chunks.

## 6. Conclusions and future work
- **Authors:**
  - GVS gives stable, collision-free, consistent and loop-closing camera-guided video with any Diffusion Forcing model, without retraining.
  - Limitations: it breaks causality, so it does not apply to online or interactive generation. It struggles to condition on external images. Performance is bounded by the backbone (fails on wide-baseline loop closures; confuses staircase up and down). Spatial windows are hand-defined and do not scale.
  - Future work is discussed in their App. C.
- **Own observations:**
  - The only backbone is a small RE10K model, and evaluation uses 7 synthetic trajectories.
  - Camera poses are the only "action". There is no agent, reward or control loop, so its relevance to world models for decision making is indirect.
  - Collision metric relies on a monocular depth estimator threshold.

## 7. Sources and verification
- Paper PDF (arXiv v3, ICLR camera-ready): https://arxiv.org/pdf/2510.24718 (read Sec. 1 to 5)
- ICLR 2026 poster page: https://iclr.cc/virtual/2026/poster/10008201 (from search result; arXiv comment confirms "Published at ICLR 2026")
- Project page: https://andrewsonga.github.io/gvs ; code: https://github.com/andrewsonga/generative_view_stitching
- Citations: https://api.semanticscholar.org/graph/v1/paper/arXiv:2510.24718 (Semantic Scholar still lists venue as arXiv)
- Not verified:
  - The code license name and the last commit date (not shown on the repo page as fetched; GitHub API rate-limited).
  - The exact presentation track (poster per the ICLR page title in search results).
