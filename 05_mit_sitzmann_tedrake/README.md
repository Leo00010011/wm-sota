# MIT — Vincent Sitzmann & Russ Tedrake (Boyuan Chen, Kiwhan Song)

- **PIs:**
  - Vincent Sitzmann: MIT EECS/CSAIL, Scene Representation Group. A CoRL 2026 workshop listing also gives the affiliation "Rhoda AI"; not verified.
  - Russ Tedrake: MIT EECS/CSAIL, Robot Locomotion Group; also Toyota Research Institute.
- **Key students:** Boyuan Chen and Kiwhan Song (Diffusion Forcing line). Earlier Tedrake-lab model-learning work by Yunzhu Li, Lucas Manuelli, Terry Suh, and Danny Driess (visiting).
- **Lab / project pages:** https://www.vincentsitzmann.com · https://groups.csail.mit.edu/locomotion/ · https://www.boyuan.space
- **Research line:**
  - **Sitzmann:** video diffusion as world models. Diffusion Forcing (per-token noise levels) → Diffusion Forcing Transformer with history guidance → training-free stitching for camera-guided generation.
  - **Tedrake:** learned, structured dynamics models for model-based manipulation planning (graph or particle dynamics, keypoint latents, SDF/NeRF object representations, score-guided offline MBRL).
  - **Shared:** both PIs co-author the Diffusion Forcing papers.

## Included papers (top venue, acceptance confirmed)

Citation counts are from the Semantic Scholar API, checked 2026-10-13.

| Paper | Venue | Year | Citations | Group role |
|---|---|---|---|---|
| [Propagation Networks for Model-Based Control Under Partial Observation (PropNet)](propnet.md) | ICRA | 2019 | 158 | Tedrake last author |
| [Keypoints into the Future: Self-Supervised Correspondence in Model-Based RL](keypoints-into-the-future.md) | CoRL | 2020 | 113 | Tedrake last author |
| [Learning Models as Functionals of Signed-Distance Fields for Manipulation Planning](sdf-functional-models.md) | CoRL | 2021 | 74 | Tedrake last author (work mainly at TU Berlin) |
| [Learning Multi-Object Dynamics with Compositional Neural Radiance Fields](compositional-nerf-dynamics.md) | CoRL | 2022 | 103 | Tedrake co-author (last: Toussaint, not a summary PI) |
| [Fighting Uncertainty with Gradients: Offline RL via Diffusion Score Matching (SGP)](score-guided-planning.md) | CoRL | 2023 | 18 | Tedrake last author |
| [Diffusion Forcing: Next-token Prediction Meets Full-Sequence Diffusion](diffusion-forcing.md) | NeurIPS | 2024 | 779 | Sitzmann last; Tedrake co-author |
| [History-Guided Video Diffusion (DFoT)](dfot-history-guided-video-diffusion.md) | ICML (poster) | 2025 | 177 | Sitzmann last; Tedrake co-author |
| [Generative View Stitching (GVS)](generative-view-stitching.md) | ICLR (poster) | 2026 | 3 | Sitzmann last (borderline world-model scope, see Open issues) |

## Cross-group papers (not written here)

| Paper | Venue | Year | Why not here | Folder that should own it |
|---|---|---|---|---|
| Learning Particle Dynamics for Manipulating Rigid Bodies, Deformable Objects, and Fluids (DPI-Net). Li, Wu, Tedrake, Tenenbaum, Torralba | ICLR | 2019 | Tedrake is 3rd author. Last author is Torralba (not a summary PI). The summary lists it under Columbia (Yunzhu Li line) | 08 (Columbia, Yunzhu Li), per the summary |
| 3D Neural Scene Representations for Visuomotor Control. Y. Li, S. Li, Sitzmann, Agrawal, Torralba | CoRL (oral) | 2021 | Sitzmann is 3rd author. Last author is Torralba (not a summary PI). First author is Yunzhu Li | 08 (Columbia, Yunzhu Li), as part of the same line; if the Columbia folder does not take it, it is unowned |

## Excluded items

| Item | Reason |
|---|---|
| Large Video Planner Enables Generalizable Robot Control (Chen, ..., Tedrake, Sitzmann, Du), arXiv 2512.15840 | Preprint only. No venue acceptance found on arXiv, the project page or the web (checked 2026-10-13). Last author is Yilun Du (Harvard) in any case. 14B video model, 73 citations |
| Turning Video Models into Generalist Robot Policies (VERA) (Li, ..., Simchowitz, Sitzmann), arXiv 2605.27817 | Preprint only (May 2026). No acceptance found. Sitzmann is last author; re-check after CoRL/NeurIPS 2026 decisions |
| Controlling diverse robots by inferring Jacobian fields with deep networks (Neural Jacobian Fields) (Sitzmann last) | Published in Nature (2025), which is not in the venue list. Arguably a self-model rather than a world model |
| The Surprising Effectiveness of Linear Models for Visual Foresight in Object Pile Manipulation (Tedrake last) | WAFR 2020, not a top venue in scope |
| DittoGym: Learning to Control Soft Shape-Shifting Robots (ICLR 2024, Sitzmann last) | RL benchmark and policy for soft robots, not a world-model paper |
| Lyapunov-stable neural-network control (RSS 2021, Tedrake last) | Controller and Lyapunov synthesis focus; learned dynamics is incidental. Judged out of scope |
| Smoothed Online Learning for Prediction in Piecewise Affine Systems (NeurIPS 2023, Tedrake last) | System-identification and online-learning theory, not a world model for planning or prediction from observations |
| Learning Geometric Reasoning and Control for Long-Horizon Tasks from Visual Input (ICRA 2021, Driess, Ha, Tedrake, Toussaint) | Learns feasibility classifiers and a policy, not a predictive dynamics model; last author Toussaint |
| Locality in Image Diffusion Models (NeurIPS 2025), Selective Underfitting in Diffusion Models, view-synthesis papers (pixelSplat CVPR 2024, etc.) (Sitzmann last) | Not world models (image diffusion theory, 3D reconstruction) |
| SceneSmith; Steerable Scene Generation (Tedrake last) | Simulation scene generation, not dynamics/world models; arXiv only in Semantic Scholar |

## Method

- **Sources checked:**
  - Semantic Scholar author API for all of Sitzmann's split author IDs and both of Tedrake's IDs. The Tedrake main profile list appeared truncated to 2019–2024 items.
  - arXiv API metadata (comments for acceptance), PMLR volume indices (v155, v164, v205, v229, v267), NeurIPS proceedings, and the ICML and ICLR virtual sites.
  - Project pages and GitHub READMEs; the Hugging Face API for the DFoT checkpoints.
  - Web searches for 2025–2026 Sitzmann and Tedrake world-model papers.
  - DBLP was blocked by bot protection. Boyuan Chen's publications page is JS-rendered and was not readable.
- **Papers:** each paper was read from the arXiv PDF (main text plus relevant appendix). Numbers are taken from the cited tables or figures.
- **Citation counts:** Semantic Scholar batch API, checked 2026-10-13.

## Open issues

- **The summary's DFoT description:** "preliminary zero-shot robot motion planning" is not in the ICML camera-ready (arXiv v2). The robot experiment there is long-horizon imitation learning (83% success with temporal history guidance). The summary's other DFoT claims (variable history, history guidance, stable rollouts far beyond training horizon) are confirmed.
- **The summary lists only 2 papers.** This folder adds 6 more top-venue world-model papers, mostly Tedrake's model-based manipulation line (2019–2023) and Sitzmann's GVS (ICLR 2026).
- **GVS is borderline.** It is a sampling method for a camera-pose-conditioned video model. There are no agent actions or control, and the paper does not call itself a world model. It is included because camera trajectories act as action conditioning and it extends the DF/DFoT world-model line. The orchestrator may drop it.
- **Ownership edge cases:**
  - PropNet and Keypoints into the Future have Yunzhu Li (Columbia PI) as first or second author, but Tedrake is last, so they are kept here.
  - Compositional NeRF dynamics and SDF functionals are joint work with Toussaint (TU Berlin). Tedrake is last only on the SDF paper.
  - DPI-Net and "3D Neural Scene Representations" have Torralba as last author and are flagged for the Columbia folder.
- **Not verified:**
  - Code licenses for PropNet, GVS and score_po (no license shown).
  - Last-commit dates for most repos (GitHub API rate-limited).
  - Oral or poster status for the CoRL papers.
  - The PMLR supplementary ZIP contents for the SDF paper.
- **Coverage risk:** Tedrake's Semantic Scholar list looked truncated, and DBLP was inaccessible, so a 2024–2026 Tedrake world-model paper at a top venue could have been missed. Web searches found none.
- **To re-check:** acceptance of LVP and VERA after CoRL 2026 and NeurIPS 2026 decisions.
