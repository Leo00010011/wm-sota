# Stanford — Jiajun Wu

- **PI:** Jiajun Wu, Assistant Professor of Computer Science (and, by courtesy, Psychology), Stanford University. He belongs to SAIL, the Stanford Vision and Learning Lab (SVL), the Graphics Lab and the Stanford Robotics Center. He did his PhD at MIT with Bill Freeman and Josh Tenenbaum.
- **Key members on the world-model papers:**
  - Hong-Xing (Koven) Yu: the "Wonder" line and FluidNexus.
  - Zizhang Li, Haoyi Duan, Yunzhi Zhang, Stephen Tian, Haochen Shi.
  - Close collaborators: Yunzhu Li (now Columbia, group 08), Li Fei-Fei, Ruohan Zhang.
- **Pages:** https://jiajunwu.com/ · https://profiles.stanford.edu/jiajun-wu · https://svl.stanford.edu/
- **Research lines on world models:**
  1. **Action-conditioned 3D/4D world generation.** Hybrid generative simulators that couple physics solvers with video diffusion. Papers: PhysDreamer → WonderPlay → PerpetualWonder. Related work: WonderWorld (world generation), FluidNexus (fluid prediction), and the WorldScore benchmark.
  2. **Learned particle and graph dynamics models for robot manipulation via MPC.** Papers: RoboCraft → RoboCook → Dynamic-Resolution → RoboPack; ActionOSF; IMPASTO.
  3. **Video prediction as a world model for control.** Papers: VP² benchmark, MaskViT, VEST, Dream2Flow.
  4. **MIT-era object-centric physical prediction.** Papers: O2P2, PSD.

## Included papers (top venue, acceptance confirmed)

Citation counts come from the Semantic Scholar API, checked 2026-10-13, except where marked otherwise. "Wu last" means Wu is the senior author. "Co-author" means the senior author is a non-summary PI; these are kept here because no other summary group owns them.

| Paper | Venue | Year | Citations | Wu's role |
|---|---|---|---|---|
| [Reasoning About Physical Interactions with Object-Oriented Prediction and Planning (O2P2)](o2p2.md) | ICLR | 2019 | 137 (checked 2026-10-15) | Wu last (MIT era) |
| [Unsupervised Discovery of Parts, Structure, and Dynamics (PSD)](psd-parts-structure-dynamics.md) | ICLR | 2019 | 63 (checked 2026-10-15) | Wu last and corresponding (MIT era) |
| [RoboCraft](robocraft.md) | RSS 2022; extended in IJRR 43(4) 2024 | 2022 / 2024 | 141 | Wu last |
| [Video Extrapolation in Space and Time (VEST)](vest-video-extrapolation.md) | ECCV | 2022 | 8 (OpenAlex, checked 2026-10-15) | Wu last |
| [A Control-Centric Benchmark for Video Prediction (VP²)](vp2-control-centric-video-prediction.md) | ICLR | 2023 | 34 | Wu last |
| [MaskViT](maskvit.md) | ICLR | 2023 | 160 | Co-author (last author: Fei-Fei Li) |
| [Multi-Object Manipulation via Object-Centric Neural Scattering Functions (ActionOSF)](action-osf.md) | CVPR | 2023 | 14 | Wu last |
| [Dynamic-Resolution Model Learning for Object Pile Manipulation](dynamic-resolution-pile-manipulation.md) | RSS | 2023 | 47 | Wu last |
| [RoboCook](robocook.md) | CoRL | 2023 | 131 | Wu last |
| [RoboPack](robopack.md) | RSS | 2024 | 46 | Wu last |
| [PhysDreamer](physdreamer.md) | ECCV (oral) | 2024 | 241 | Co-author (last author: W. T. Freeman, MIT) |
| [WonderWorld](wonderworld.md) | CVPR | 2025 | 235 | Wu last |
| [FluidNexus](fluidnexus.md) | CVPR (oral) | 2025 | 32 | Wu last |
| [WonderPlay](wonderplay.md) | ICCV (highlight) | 2025 | 56 | Wu last |
| [WorldScore](worldscore.md) | ICCV | 2025 | 154 | Wu last |
| [PerpetualWonder](perpetualwonder.md) | CVPR | 2026 | 11 | Wu last |
| [Dream2Flow](dream2flow.md) | ICRA | 2026 | 42 | Co-author (equal advising: Fei-Fei Li and Ruohan Zhang) |
| [IMPASTO](impasto.md) | ICRA | 2026 | 0 | Co-author (last author: Ruohan Zhang) |

**Inclusion notes**
- Five Wu-last papers have Yunzhu Li (summary group 08) as a co-author: RoboCraft, RoboCook, Dynamic-Resolution, ActionOSF and RoboPack. Under the ownership rule they belong here. The 08 README already lists them as cross-group papers owned by 07.
- IMPASTO has Yunzhu Li as second-to-last author, but he is not the senior author, so it stays here.
- Borderline scope:
  - WonderWorld is camera- and text-controlled *static* 3D world generation, with no dynamics. It is kept because the summary lists it.
  - WorldScore is a benchmark, not a model.
  - VEST and PSD are passive future-frame predictors (not action-conditioned).
  - O2P2 predicts only the final steady state.
- O2P2 and PSD predate Wu's Stanford appointment. They are included because Wu is the last author.

## Cross-group papers (Wu co-author; another summary PI is senior)

| Paper | Venue | Year | Senior author | Owning folder |
|---|---|---|---|---|
| Model-Based Control with Sparse Neural Dynamics | NeurIPS | 2023 | Yunzhu Li | 08_columbia_yunzhu_li (file `sparse-neural-dynamics.md` exists) |
| Reconstruction and Simulation of Elastic Objects with Spring-Mass 3D Gaussians (Spring-Gaus) | ECCV | 2024 | Yunzhu Li | 08_columbia_yunzhu_li (`spring-gaus.md`) |
| Propagation Networks for Model-Based Control Under Partial Observation (PropNet) | ICRA | 2019 | Russ Tedrake | 05_mit_sitzmann_tedrake (`propnet.md`) |
| Efficient Imitation Learning with Conservative World Models | L4DC | 2024 | Chelsea Finn | 09 would own it, but L4DC is not a top venue, so it is out of scope anyway |

Not owned by any summary PI and handled by other folders:
- DPI-Net (ICLR 2019; Wu 2nd author, last author Torralba) and Compositional Koopman (ICLR 2020; last author Torralba). Folder 08 includes both following the summary.
- Visual Grounding of Learned Physical Models (`vgpl.md` in 08).

## Excluded items

| Item | Reason |
|---|---|
| RealWonder: Real-Time Physical Action-Conditioned Video Generation (arXiv 2603.05449; Wu last) | arXiv only; no venue found (arXiv comment, project page, web search) |
| Choreographing a World of Dynamic Objects (CHORD; CVPR 2026; Wu last) | Accepted (CVPR 2026 poster page), but it generates text-driven 4D object motion distilled from video models. It is not action-conditioned prediction, so it is out of scope (borderline; see Open issues) |
| NeuROK: Generative 4D Neural Object Kinematics (CVPR 2026; Wu last) | Learned prior over object deformation states (inverse kinematics / 4D shape generation), not a predictive world model (borderline) |
| WonderJourney (CVPR 2024; last author Herrmann, Google) and WonderZoom (CVPR 2026; Wu last) | Static 3D scene generation with no dynamics. Same family as WonderWorld, but not listed in the summary and outside the "predict future states" scope |
| Ctrl-VI (arXiv 2510.07670), RoboRender (arXiv 2610.09254), T²Mem (arXiv 2609.36720), APIVOT, AnyLift | arXiv only, or not world models |
| Masked Visual Actions for Unified World Modeling (arXiv 2607.19343), Cross-Embodiment Robot Foundation World Models (arXiv 2610.10846), OpenWAM (arXiv 2610.07922) | arXiv only. Listed under Wu's Semantic Scholar profile, but that profile merges several people, so Wu's authorship was not verified |
| VAGEN (NeurIPS 2025; last author Manling Li) | World-model *reasoning* in VLM agents (language-level state prediction), not a learned dynamics or observation model; the senior author is outside the summary |
| ENACT (ICLR 2026; last author Manling Li) | Benchmark of VLM embodied cognition and world modelling; not a world-model method; the senior author is outside the summary |
| Model-Based Policy Adaptation for Closed-Loop End-to-End Driving (NeurIPS 2025; last author Ding Zhao) | Uses a 3DGS simulator plus a policy adapter and Q-model, not a learned world model |
| Learning Vortex Dynamics for Fluid Inference and Prediction (ICLR 2023; last author Bo Zhu); Inferring Hybrid Neural Fluid Fields from Videos (NeurIPS 2023) | Physics-based fluid reconstruction and inference; not learned world models (FluidNexus is included because it predicts with generative refinement) |
| Probabilistic Video Prediction from Noisy Data with a Posterior Confidence (CVPR 2020; Wu 2nd author, last author Tenenbaum) | Wu is not senior and the senior author is not a summary PI; MIT-era co-authorship. Could be added if the scope widens |
| Entity Abstraction in Visual Model-Based RL (CoRL 2019; last author Levine); Visual Dynamics / cross-convolutional future generation (NeurIPS 2016, TPAMI 2019; last author Freeman) | Wu is not senior; MIT/Berkeley-era co-authorship |
| Evaluating Real-World Robot Manipulation Policies in Simulation (SIMPLER, CoRL 2024; last author Ted Xiao) | Hand-built simulators, not a learned world model |
| Differentiable Physics Simulation of Dynamics-Augmented Neural Objects (RA-L 2023) | Journal is not in the top-venue list |
| A Review of Learning-Based Dynamics Models for Robotic Manipulation (Science Robotics 2025); "World Model for Robot Learning" survey (IJRR 2026, arXiv 2605.00080, Wu co-author) | Surveys, not method papers. The IJRR survey's acceptance was not checked |
| Discovering Hybrid World Representations with Co-Evolving Foundation Models (AAAI 2026 Emerging Trends track; Wu first author) | Perspective / emerging-trends track paper, not a world-model method (contents not verified) |
| ZeroHSI (3DV), 60-1 Physics-Grounded Visual World Generation (SID Digest 2026) | Venues outside the top-venue list |

## Method
- **Sources checked:**
  - Wu's homepage publication list (https://jiajunwu.com/, fetched 2026-10-13).
  - Semantic Scholar author record (id 2326444500; it merges several people and is incomplete, so it was cross-checked against the homepage).
  - arXiv API (titles, authors, comments).
  - CVF Open Access and the CVPR/ICCV virtual pages, ECVA, Crossref (IJRR DOI), OpenAlex (VEST).
  - Web search for venues, and every project page and GitHub repo (README, LICENSE, commits Atom feed).
- **What was not available:** DBLP returned a bot-check page, and the GitHub REST API was rate-limited, so repo metadata was scraped from HTML, raw files and Atom feeds.
- **Selection rule:**
  - Top-venue papers on learned or generative models that predict future states or observations, are action-conditioned generators, or evaluate such models, where Wu is last author.
  - Plus Wu co-authored papers whose senior author is not a PI of another summary group.
- **Reading:** every included paper was read from its arXiv PDF (main text; appendices where relevant). Numbers are taken from the paper tables cited in each file.
- **Citation dates:** Semantic Scholar counts checked 2026-10-13 (O2P2 and PSD on 2026-10-15). VEST count from OpenAlex on 2026-10-15, because Semantic Scholar does not resolve that paper.

## Open issues
- **Summary file vs. findings:**
  - The summary gives WonderPlay with "method and venue not verified". It is ICCV 2025 (Highlight): a hybrid physics-solver plus video-diffusion generative simulator.
  - The summary lists only WonderWorld and WonderPlay. This folder adds 16 more top-venue papers.
  - The summary assigns the particle-dynamics line (DPI-Net, PhysTwin, etc.) to Yunzhu Li. Several of its papers (RoboCraft, RoboCook, Dynamic-Resolution, RoboPack, ActionOSF) have Wu as the senior author and are owned here.
- **Scope calls to confirm with the orchestrator:**
  - CHORD and NeuROK (both CVPR 2026, Wu last) were excluded as borderline. Add them if text-driven 4D motion generation counts as world modelling.
  - WonderZoom and WonderJourney were excluded, but WonderWorld was kept. This is inconsistent by construction, because only WonderWorld is in the summary.
  - Co-authored papers without a summary-PI senior author (MaskViT, PhysDreamer, Dream2Flow, IMPASTO) were kept, following the precedent in 01_ucsd_wang_su. Drop them if the folder should hold only Wu-senior papers.
- **Unverified details:**
  - Presentation types: ICLR, RSS and CoRL tracks; the RoboCook award (finalist confirmed, win not confirmed).
  - Exact proceedings pages for ActionOSF, PhysDreamer and VP².
  - ICRA 2026 acceptance for Dream2Flow and IMPASTO rests on IEEE ICRA DOIs reported by Semantic Scholar; the program pages were not opened.
  - The IJRR RoboCraft journal version was not read.
- **Table values from multi-column PDF text:** some values were extracted from multi-column PDF text (RoboPack Table I, FluidNexus SSIM/LPIPS, VEST KITTI, PSD IoU). Only values that could be aligned confidently are reported; the files flag this.
- **Release status:**
  - No code found for ActionOSF, MaskViT or IMPASTO.
  - O2P2's repo has data-generation code only.
  - Several repos have no LICENSE file.
- **Coverage gap:** Wu has more than 200 publications. Pre-2019 MIT-era co-authored physics-prediction papers (Galileo, Visual De-animation, etc.) were not reviewed in detail, because Wu was not the senior author on them.
