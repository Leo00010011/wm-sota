# Columbia University — Yunzhu Li (RoboPIL)

- **PI:** Yunzhu Li, Assistant Professor of Computer Science at Columbia University. Earlier: PhD at MIT CSAIL, then Stanford (postdoc) and UIUC (faculty). Pronoun "they" is used here because no source read states otherwise.
- **Lab:** Robotic Perception, Interaction, and Learning Lab (RoboPIL), https://robopil.github.io
- **PI page:** https://yunzhuli.github.io
- **Research line:** structured, physics-informed world models for manipulation. The line runs:
  - Particle and graph neural dynamics: DPI-Net, then AdaptiGraph, GS-Dynamics and PGND.
  - Koopman and latent 3D models.
  - Physics-based digital twins from video: Spring-Gaus, then PhysTwin and Real2Sim-Eval.
  - Planners over learned dynamics: sparse MIP, then BaB-ND.
  - In 2026, a 2D action-conditioned video world model (Interactive World Simulator) and a 2D-vs-3D world-model benchmark (Deform360).

## Included papers (top venue, acceptance confirmed)

Citation counts are from the Semantic Scholar batch API, checked 2026-10-13.

| Paper                                                                                                                       | Venue            | Year | Citations | Yunzhu Li's role                               |
| --------------------------------------------------------------------------------------------------------------------------- | ---------------- | ---- | --------- | ---------------------------------------------- |
| [Learning Particle Dynamics for Manipulating Rigid Bodies, Deformable Objects, and Fluids (DPI-Net)](dpi-net.md)            | ICLR             | 2019 | 497       | First author (Torralba last)                   |
| [Learning Compositional Koopman Operators for Model-Based Control](compositional-koopman.md)                                | ICLR (spotlight) | 2020 | 139       | Co-first author (Torralba last)                |
| [Visual Grounding of Learned Physical Models (VGPL)](vgpl.md)                                                               | ICML             | 2020 | 91        | First and corresponding author (Torralba last) |
| [Causal Discovery in Physical Systems from Videos (V-CDN)](v-cdn.md)                                                        | NeurIPS          | 2020 | 135       | First author (Garg last)                       |
| [3D Neural Scene Representations for Visuomotor Control](3d-neural-scene-representations.md)                                | CoRL (oral)      | 2021 | 159       | Co-first author (Torralba last)                |
| [Model-Based Control with Sparse Neural Dynamics](sparse-neural-dynamics.md)                                                | NeurIPS          | 2023 | 28        | Last author                                    |
| [Reconstruction and Simulation of Elastic Objects with Spring-Mass 3D Gaussians (Spring-Gaus)](spring-gaus.md)              | ECCV             | 2024 | 127       | Last author                                    |
| [AdaptiGraph: Material-Adaptive Graph-Based Neural Dynamics](adaptigraph.md)                                                | RSS              | 2024 | 90        | Last author                                    |
| [Dynamic 3D Gaussian Tracking for Graph-Based Neural Dynamics Modeling (GS-Dynamics)](gs-dynamics.md)                       | CoRL             | 2024 | 60        | Last author                                    |
| [BaB-ND: Long-Horizon Motion Planning with Branch-and-Bound and Neural Dynamics](bab-nd.md)                                 | ICLR             | 2025 | 5         | Last author                                    |
| [KUDA: Keypoints to Unify Dynamics Learning and Visual Prompting](kuda.md)                                                  | ICRA             | 2025 | 7         | Last author                                    |
| [PhysTwin: Physics-Informed Reconstruction and Simulation of Deformable Objects from Videos](phystwin.md)                   | ICCV             | 2025 | 127       | Last author                                    |
| [Particle-Grid Neural Dynamics (PGND)](pgnd.md)                                                                             | RSS              | 2025 | 42        | Last author                                    |
| [Real-to-Sim Robot Policy Evaluation with Gaussian Splatting Simulation of Soft-Body Interactions](real2sim-policy-eval.md) | ICRA             | 2026 | 44        | Last author                                    |
| [Interactive World Simulator for Robot Policy Training and Evaluation](interactive-world-simulator.md)                      | RSS              | 2026 | 41        | Last author                                    |
| [Deform360: A Massive Multi-view Visuotactile Dataset for Deformable World Models](deform360.md)                            | ECCV             | 2026 | 1         | Last author                                    |

**Note on the first five rows (2019–2021, MIT era).** Yunzhu Li is first author and the senior author is not a summary PI. The senior authors are Torralba for four papers and Animesh Garg for V-CDN. The orchestrator assigned DPI-Net and the CoRL 2021 paper to this folder. I applied the same rule to Koopman, VGPL (where Li is also the stated corresponding author) and V-CDN. The orchestrator may move these papers to an "unowned" list if a strict last-author rule is preferred.

## Cross-group papers (not written here)

| Paper | Venue | Year | Last author | Should be owned by |
|---|---|---|---|---|
| Propagation Networks for Model-Based Control Under Partial Observation (PropNet) | ICRA | 2019 | Tedrake | 05_mit_sitzmann_tedrake (already written there) |
| Keypoints into the Future: Self-Supervised Correspondence in Model-Based RL | CoRL | 2020 | Tedrake | 05 (already written there) |
| Learning Multi-Object Dynamics with Compositional Neural Radiance Fields | CoRL | 2022 | Toussaint (Tedrake co-author) | 05 (already written there) |
| RoboCraft: Learning to See, Simulate, and Shape Elasto-Plastic Objects with Graph Networks | RSS 2022, and IJRR 2023 journal version | 2022/2023 | Jiajun Wu | 07_stanford_wu |
| RoboCook: Long-Horizon Elasto-Plastic Object Manipulation with Diverse Tools | CoRL (Best Systems Paper) | 2023 | Jiajun Wu | 07_stanford_wu |
| Dynamic-Resolution Model Learning for Object Pile Manipulation | RSS | 2023 | Jiajun Wu (Yunzhu Li co-first) | 07_stanford_wu |
| Multi-Object Manipulation via Object-Centric Neural Scattering Functions | CVPR | 2023 | Jiajun Wu | 07_stanford_wu |
| RoboPack: Learning Tactile-Informed Dynamics Models for Dense Packing | RSS | 2024 | Jiajun Wu | 07_stanford_wu |

Folder 07 had not produced a README when I checked, so I could not confirm whether it includes these papers.

## Excluded items

| Item | Reason |
|---|---|
| PhysWorld (arXiv 2510.21447) | Not this group. The authors are Yu Yang, Zhilu Zhang, Xiang Zhang, Yihan Zeng, Hui Li and Wangmeng Zuo (Harbin Institute of Technology line); Yunzhu Li is not an author. It is an arXiv preprint with no venue found. The summary's "47× faster than PhysTwin" claim concerns another group's paper. |
| Learning Physics-Guided Residual Dynamics for Deformable Object Simulation (PGRD, arXiv 2607.13451; Patel, …, Lazebnik, Y. Li last) | Acceptance not confirmed. The project page says "CoRL Spotlight 2026", but the arXiv comment has no venue, OpenReview was blocked by a challenge, and no public CoRL 2026 accepted list was found (checked 2026-10-15). It is in scope if confirmed: a spring-mass simulator plus a learned residual transformer, with MPC. |
| Robotic Manipulation by Imitating Generated Videos Without Physical Demonstrations (RIGVid, ICLR 2026; Jain, Lazebnik and Y. Li co-senior) | Uses off-the-shelf video generation to produce demonstrations. It is not an action-conditioned world model or world-model planning. Borderline; excluded. |
| Localized Graph-Based Neural Dynamics Models for Terrain Manipulation (ICRA 2026) | Graph neural dynamics world model, but the last author is Kris Hauser (not a summary PI) and Y. Li is a middle author. Unowned. |
| IMPASTO: Model-Based Planning with Learned Dynamics for Robotic Oil Painting (ICRA 2026) | Learned dynamics plus planning, but the last author is Ruohan Zhang (not a summary PI); Y. Li is second-to-last. Unowned. |
| PhysGen3D: Crafting a Miniature Interactive World from a Single Image (CVPR 2025) | The last author is Shenlong Wang; Y. Li is a middle author. Unowned. |
| 3D-IntPhys: 3D-grounded Visual Intuitive Physics (NeurIPS 2023) | The last author is Hsiao-Yu Tung; Y. Li is second-to-last. Unowned. |
| Boba: Batched Simulation for Physics-Based Gaussian Digital Twins (ECCV 2026) | A web poster lists Shenlong Wang as last author, with no Y. Li authorship found. Not this group. |
| BoxTwin (arXiv 2607.17132) and Agentic Real2Sim (arXiv 2607.19190) | The last author is Peter Yichen Chen, and BoxTwin is workshop or arXiv only. Not owned here. |
| A Review of Learning-Based Dynamics Models for Robotic Manipulation (Science Robotics 2025) | Journal not in the venue list, and it is a review. |
| D³Fields (CoRL 2024), GenDP (CoRL 2024), RoboEXP (CoRL 2024), CodeDiffuser (RSS 2025), 3D-ViTac (CoRL 2024), VT-Refine (CoRL 2025), Touch in the Wild (NeurIPS 2025), IKER (ICRA 2025), CuriousBot (RA-L) | Y. Li is last author, but these are policies, scene representations or tactile systems, not predictive world models. RoboEXP's "action-conditioned scene graph" is a memory structure, not a dynamics predictor. |
| Dynamic Modeling of Hand-Object Interactions via Tactile Sensing (IROS 2021) | IROS is not in the venue list. |

## Method

- **Sources checked:**
  - The PI's homepage publication list (full "by date" list parsed from the HTML).
  - arXiv API author search "Yunzhu Li" (all entries, with comments for acceptance).
  - Semantic Scholar author API (main ID 3422021 plus split IDs) and batch paper API.
  - Project pages and GitHub READMEs and LICENSE files (via raw.githubusercontent.com).
  - Hugging Face API, the RSS proceedings indexes (rss20, rss21), the RSS 2026 program and proceedings, and the ECCV 2026 accepted-papers listing.
  - Web search for 2026 acceptances (NeurIPS 2026, CoRL 2026).
- **Not accessible:**
  - DBLP and Google Scholar (not tried, because of known bot blocking).
  - OpenReview API (challenge-blocked).
  - GitHub REST API (rate-limited), so last-commit dates are not recorded.
- **Papers:** each paper was read from its arXiv PDF (main text, plus appendix where relevant). Numbers are taken from the cited tables. Where pdftotext scrambled table layout, I reconstructed the values and say so in the file.
- **Citation counts:** Semantic Scholar batch API, checked 2026-10-13.

## Open issues

- **Disagreements with the summary file:**
  - PhysWorld (2510.21447) is not by Yunzhu Li or this lab.
  - The summary lists only 3 items. This folder adds 13 more top-venue papers, including several 2023–2026 last-author papers.
  - The research line moved in 2026 toward a 2D video world model (Interactive World Simulator, RSS 2026) and 2D-vs-3D benchmarking. The summary's "narrower in scope" framing does not capture this.
- **Ownership decisions for the orchestrator:**
  - The five MIT-era first-author papers (see the note above).
  - V-CDN's last author is Animesh Garg. Section 9 of the summary mentions "University of Toronto" without naming a PI; if Garg is meant, V-CDN should move there.
- **Borderline world-model scope:**
  - Real2Sim-Eval: a physics simulator plus GS rendering for policy evaluation.
  - BaB-ND and Sparse Neural Dynamics: planning contributions over learned dynamics.
  - KUDA: VLM plus dynamics planning.
  - Deform360: a dataset and benchmark.
- **To re-check:**
  - PGRD's CoRL 2026 acceptance, once the accepted list or OpenReview is reachable.
  - Any NeurIPS 2026 acceptances from the lab; none were found as of 2026-10-15.
- **Not verified:**
  - Presentation types (oral, spotlight, poster) except where stated.
  - Repository last-commit dates.
  - Hugging Face asset licenses.
  - The PGND LICENSE file, whose copyright line reads "GitHub Inc.", which looks like a template.
  - ICRA 2026 and ECCV 2026 conference dates.
