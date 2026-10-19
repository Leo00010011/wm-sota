---
citation_count: 11
citation_source: "Semantic Scholar, 2026-10-13"
venue: "CVPR 2026"
date: 2026-02-04
research_group: "Stanford (Jiajun Wu)"
---

# PerpetualWonder: Long-Horizon Action-Conditioned 4D Scene Generation

- **Authors:** Jiahao Zhan*, Zizhang Li*, Hong-Xing Yu, **Jiajun Wu** (*equal contribution; Zhan was a visiting student at Stanford and is now at Fudan)
- **Group role:** senior (last) author
- **Date published:** 2026-06 (CVPR 2026 conference, June 2026; arXiv v1 2026-02-04, v2 2026-05-20)
- **Venue:** CVPR 2026, proceedings pp. 25370–25380. Poster page listed. Award or highlight status not verified.
- **Links:**
  - arXiv: https://arxiv.org/abs/2602.04876
  - CVF: https://openaccess.thecvf.com/content/CVPR2026/html/Zhan_PerpetualWonder_Long-horizon_Action-conditioned_4D_Scene_Generation_CVPR_2026_paper.html
  - Poster page: https://cvpr.thecvf.com/virtual/2026/poster/37621
  - Project page: https://johnzhan2023.github.io/PerpetualWonder/
  - Code: https://github.com/JohnZhan2023/PerpetualWonder
- **Number of citations:** 11 (source: Semantic Scholar API, checked 2026-10-13; Semantic Scholar still labels the venue "arXiv")

## Summary
PerpetualWonder is a closed-loop "hybrid generative simulator". From a single image and a sequence of physical actions (gravity, wind fields, point forces), it produces a long-horizon, action-conditioned 4D scene. It extends WonderPlay in two ways:
- **Shared physical and visual state.** A visual-physical aligned particle (VPP) representation ties the Gaussians to the physics particles. Corrections from the video model therefore update the physical state that the next action starts from.
- **Multi-view refinement.** The 4D scene is refined progressively from several viewpoints, which keeps it 3D-consistent.

## 1. Weights availability
- No new weights are trained. The pipeline uses pre-trained models:
  - GEN3C (Cosmos-Predict1 based) for dense view synthesis.
  - Go-with-the-Flow (CogVideoX-based) as the flow- and RGB-conditioned refiner.
  - SAM2, COLMAP, Hunyuan3D (rigid object meshes) and a VLM for material parameters.
- The README says it "will provide" intermediate results for reproducibility from a downloads.cs.stanford.edu URL. It requires 80 GB of GPU memory.

## 2. Code
- https://github.com/JohnZhan2023/PerpetualWonder
  - Contents: the full pipeline (scene reconstruction, forward pass, backward optimization), a modified Genesis, gsplat, and three example scenes (play_doh, dumpling, jam).
  - Two conda environments are needed (main, and cosmos-predict1 for GEN3C).
- No LICENSE file was found (raw `LICENSE` returns 404).
- Last commit: 2026-04-12 (commits Atom feed).
- No training code, because nothing is trained.

## 3. Datasets
- No training data.
- Evaluation: a curated set of 10 scenes (cloth, rigid, elastic, liquid, gas, granular). Each scene runs 3 time windows of 392 simulation steps with a different action per window (Sec. 4). The set is not released as a benchmark.

## 4. Evaluation and downstream tasks
- **Metrics:**
  - WorldScore camera controllability, 3D consistency and imaging quality on renders along a fixed camera path.
  - A 2AFC user study with 350 participants rating physics plausibility and motion fidelity (Supp. D).
- **Table 1 (10 scenes):**

  | Method | Camera Ctrl | 3D Consist | Imaging |
  |---|---|---|---|
  | PerpetualWonder | 93.26 | 80.41 | 66.98 |
  | GEN3C | 80.29 | 61.69 | 66.25 |
  | WonderPlay | 75.95 | 63.93 | 36.80 |
  | Veo3.1 | 60.61 | 73.93 | 67.82 |
  | Wan2.2 | 59.73 | 65.35 | 67.03 |

  PerpetualWonder is best on camera control and 3D consistency; imaging quality is close to the best (Veo3.1 67.82, Wan2.2 67.03).
- **Table 2 (human preference for PerpetualWonder):**
  - Physics plausibility: 62.0% over Veo3.1, 80.8% over WonderPlay, 93.5% over GEN3C.
  - Motion fidelity: 70.8% to 86.3% over all baselines.
- **Baselines:** conditional video generators (Wan2.2, Wan2.6, Veo3.1, Tora, DaS, GEN3C) and WonderPlay. The text also mentions a WonderPlay++ variant (WonderPlay with the new multi-view initialization), but no numbers for it appear in Table 1.
- **Qualitative ablations (Figs. 6–7):**
  - Without VPP, dynamics become chaotic.
  - Without progressive multi-view optimization, textures blur and flicker.
- **Runtime (Table S2):** about 16 min for the first loop (8 min initialization, under 1 min forward pass, about 7 min backward optimization).
- **Downstream tasks:** none (no robotics, planning or RL).

## 5. Architecture and techniques
- **Representation:** visual-physical aligned particles (VPP).
  - Each physics particle anchors K isotropic Gaussians through bounded offsets (tanh(p̃)·δ).
  - Each Gaussian has a spatial opacity and a Gaussian-shaped temporal opacity.
  - K = 1 for rigid objects and cloth; K = 20 for volumetric materials.
- **Initialization:**
  1. GEN3C generates 242 dense views (two 90° arcs).
  2. COLMAP and 3DGS reconstruct the scene; Gaussian Grouping with SAM2 separates the objects.
  3. TSDFusion builds meshes, replaced by Hunyuan3D meshes for rigid objects.
  4. Physics particles are sampled from the meshes.
- **Forward pass:** Genesis solvers (rigid, MPM, PBD), stepped through the action sequence.
- **Backward pass:**
  - The coarse simulation is rendered to RGB and optical flow and refined by Go-with-the-Flow (704×1280, 49 frames).
  - The loss is a photometric term plus a simulation-consistency term L_sim, which keeps the Gaussians near their particles.
  - Optimization is progressive: frontal view, then side views with weaker control, then all views together.
- **Loop closure:** particle positions are reset to the mean of their optimized Gaussians. Velocities are kept from the simulation. This becomes the starting state for the next action window.

## 6. Conclusions and future work
- **Authors' conclusions:**
  - Unifying the physical and visual representations, and updating them with multi-view optimization, enables sequential multi-step interaction.
  - Limitations (Supp. H): the system is not real-time because of backward-optimization overhead, and it cannot complete geometry not seen in the input (e.g., an object entering from out of view). Completing unseen geometry is left as future work.
- **Own observations:**
  - Physics parameters still come from a VLM with optional manual tuning.
  - The evaluation is small (10 scenes) and uses perceptual and consistency proxies; there is no comparison with ground-truth dynamics.
  - The "horizon" is about 3–4 action windows.
  - The pipeline depends on several heavy foundation models and an 80 GB GPU.

## 7. Sources and verification
- arXiv PDF v2: https://arxiv.org/pdf/2602.04876. I read Secs. 1–5, Tables 1–2, and Supp. A–H (Tables S1–S2).
- CVPR 2026 open-access page and poster page (found via web search). Wu's homepage lists "CVPR 2026".
- Repo README: https://github.com/JohnZhan2023/PerpetualWonder. Semantic Scholar API: `paper/arXiv:2602.04876`.
- Not verified: presentation type (oral or highlight), and whether the promised intermediate-result download is live.
