# Academic Groups Working on World Models — Research Summary

*Compiled October 2026. Focus: publications and reported results, not corporate announcements.*
*Figures are taken from paper abstracts, project pages and venue listings; they have not been checked against full result tables.*

---

## Overview

| Research line | Leading academic groups | State of results |
|---|---|---|
| Latent model-based RL | UC San Diego (Wang / Su), Geneva (Fleuret) | Mature, reproducible, open checkpoints |
| JEPA / representation-space world models | NYU / Brown (LeCun, Balestriero) | Fast-moving; cheap single-GPU training; long-horizon planning still open |
| Feature-space forecasting with foundation models | Athena RC / Univ. of Crete + valeo.ai (Komodakis, Gidaris) | Steady top-venue output; strong semantic forecasting; no closed-loop control yet |
| Video world models | MIT (Sitzmann / Tedrake), Tsinghua (Long, Zhu), Stanford (Wu) | Visually strong; physics consistency weak |
| Physics-structured world models | Columbia (Yunzhu Li) | Data-efficient; narrower scope (deformables, manipulation) |
| World models for policy evaluation | Stanford (Liang, Yang, Finn), Toronto | Reliable policy rankings, unreliable absolute values |
| Driving world models | HKU / OpenDriveLab (Hongyang Li) | Large datasets; measurable planning gains |

---

## 1. UC San Diego — Xiaolong Wang & Hao Su (Nicklas Hansen)

**Line:** decoder-free latent world models + model-predictive control for continuous control.

- **TD-MPC** (ICML 2022) — local trajectory optimization in a learned, task-oriented latent space.
- **TD-MPC2** (ICLR 2024) — 104 online-RL tasks across 4 domains with a single hyperparameter set; one 317M-parameter agent handles 80 tasks.
- **Newt — Learning Massively Multitask World Models for Continuous Control** (ICLR 2026) — new 200-task, multi-embodiment benchmark; language-conditioned world model pretrained on demonstrations, then trained online on all tasks jointly. Better multitask performance and data efficiency than baselines, strong open-loop control, fast adaptation to unseen tasks. 200+ checkpoints released.

**Assessment:** the most reproducible academic line on scaling world models for control.

Sources: [arXiv 2310.16828](https://arxiv.org/abs/2310.16828v2) · [ICML 2022](https://proceedings.mlr.press/v162/hansen22a.html) · [arXiv 2511.19584](https://arxiv.org/pdf/2511.19584v2) · [ICLR 2026](https://www.iclr.cc/virtual/2026/poster/10009948)

---

## 2. University of Geneva — François Fleuret (Eloi Alonso, Vincent Micheli)

**Line:** generative world models (transformer, then diffusion) for sample-efficient RL.

- **IRIS → Δ-IRIS** — transformer world models; Δ-IRIS adds context-aware tokenization for efficiency.
- **DIAMOND** (NeurIPS 2024 Spotlight) — diffusion world model; mean human-normalized score of 1.46 on Atari 100k, best at the time for agents trained entirely in a world model. Also shown as a neural game engine trained on Counter-Strike gameplay.

**Assessment:** influential, but no academic follow-up found for 2025–26. Alonso co-founded General Intuition, so the line may have moved to industry.

Sources: [arXiv 2405.12399](https://arxiv.org/abs/2405.12399v2)

---

## 3. NYU / Brown / Mila network — Yann LeCun, Randall Balestriero

**Line:** JEPA world models — predict in representation space, not pixels, and plan in that space.

- **DINO-WM** (NYU) — world model on frozen DINOv2 features; beat DreamerV3 and TD-MPC2 on goal-conditioned planning when no reward labels were available.
- **LeWorldModel (LeWM)** (arXiv, March 2026; Maes, Le Lidec, Scieur, LeCun, Balestriero) — first JEPA trained stably end-to-end from pixels with only two loss terms (prediction + Gaussian regularizer); loss hyperparameters cut from 6 to 1. About 15M parameters, trains on one GPU in a few hours, plans up to 48× faster than foundation-model-based world models, about 200× fewer tokens per observation than DINO-WM. Latents encode physical quantities; the model flags physically implausible events.

**Follow-up preprints built on this line (2026, affiliations not verified):**
- *Latent Geometry Beyond Search* — goal-conditioned inverse-dynamics model on frozen LeWM latents matches or beats CEM in 7 of 8 settings at 100–130× lower cost per decision ([2605.08732](https://arxiv.org/abs/2605.08732)).
- FF-JEPA ([2606.09311](https://arxiv.org/html/2606.09311v1)), Variable-Length Latent World Models ([2606.21775](https://arxiv.org/html/2606.21775v1)), Dual-WM ([2609.37644](https://arxiv.org/abs/2609.37644)) — hierarchical / long-horizon planning.

**Assessment:** fast-moving and cheap to reproduce; long-horizon planning is the main open problem.

Sources: [arXiv 2603.19312](https://arxiv.org/pdf/2603.19312) · [LeWM code](https://github.com/lucas-maes/le-wm)

---

## 4. Athena Research Center (Archimedes) / Univ. of Crete + valeo.ai — Nikos Komodakis, Spyros Gidaris (Efstathios Karypidis, Ioannis Kakogeorgiou)

**Line:** forecast the future in the feature space of frozen vision foundation models (e.g. DINOv2), then decode to semantics, depth or pixels.

| Paper | Venue | Result |
|---|---|---|
| **FUTURIST** — Advancing Semantic Future Prediction through Multimodal Visual Sequence Transformers | CVPR 2025 | Multimodal masked transformer jointly forecasting segmentation and depth; VAE-free hierarchical tokenization allows end-to-end high-resolution training; state of the art for Cityscapes future segmentation at short (0.18 s) and mid (0.54 s) horizons. |
| **DINO-Foresight** — Looking into the Future with DINO | NeurIPS 2025 | Self-supervised masked feature transformer forecasting how foundation-model features evolve; plug-in task heads without retraining; autoregressive rollout from 4 context frames to 48 future frames (about 2.9 s). |
| **Re2Pix** — Representations Before Pixels: Semantics-Guided Hierarchical Video Prediction | ECCV 2026 (per co-author post) | Two stages: forecast DINOv2 features, then condition latent diffusion on them; better temporal semantic consistency, perceptual quality and convergence speed than baselines such as REPA. |
| **Latent-Foresight** — End-to-End Learning Predictable Representations for Latent World Models | arXiv, Oct 2026 (venue not confirmed) | Trains the feature tokenizer and a flow-matching dynamics model together, claimed first for foundation-model-feature world models; the generative model avoids averaged, blurry futures. No benchmark numbers found in the abstract. |

Related: **ReDi** (NeurIPS 2025 Spotlight) — joint image–feature synthesis for generative modeling, same "representations + pixels" philosophy.

**Assessment:** at the forefront of feature-space forecasting, with roughly one top-venue paper every six months and open code and weights. The trajectory goes from deterministic feature forecasting, to pixels guided by features, to a generative latent world model with a learned tokenizer, which converges with the JEPA line (§3). Limitation: evaluated mainly on driving and video datasets, no closed-loop control yet.

Sources: [arXiv 2501.08303](https://arxiv.org/abs/2501.08303v1) · [CVPR 2025](https://openaccess.thecvf.com/content/CVPR2025/html/Karypidis_Advancing_Semantic_Future_Prediction_through_Multimodal_Visual_Sequence_Transformers_CVPR_2025_paper.html) · [arXiv 2412.11673](https://arxiv.org/abs/2412.11673v1) · [NeurIPS 2025](https://neurips.cc/virtual/2025/poster/116713) · [arXiv 2604.11707](https://arxiv.org/pdf/2604.11707) · [arXiv 2610.01942](https://arxiv.org/html/2610.01942) · [ReDi code](https://github.com/zelaki/ReDi)

---

## 5. MIT — Vincent Sitzmann & Russ Tedrake (Boyuan Chen, Kiwhan Song)

**Line:** video diffusion as world models and planners for robots.

- **Diffusion Forcing** (NeurIPS 2024) — unifies next-token prediction with full-sequence diffusion.
- **History-Guided Video Diffusion / DFoT** (ICML 2025) — conditions on a variable number of history frames; "history guidance" improves quality and consistency and allows stable rollouts far beyond the training horizon; preliminary zero-shot robot motion planning.

**Assessment:** diffusion forcing became a standard building block for interactive video world models.

Sources: [ICML 2025](https://proceedings.mlr.press/v267/song25b.html) · [arXiv 2502.06764](https://arxiv.org/abs/2502.06764v1)

---

## 6. Tsinghua University — Mingsheng Long (THUML); Jun Zhu & Chongxuan Li

**Line:** scalable, interactive video world models.

- **iVideoGPT** (NeurIPS 2024, Long) — autoregressive transformer over observation, action and reward tokens; pretrained on millions of manipulation trajectories; used for action-conditioned video prediction, visual planning and model-based RL.
- **minWM** (arXiv, May 2026, last author Jun Zhu) — full-stack open-source framework for real-time interactive video world models, positioned as a reproducible baseline.

Sources: [arXiv 2405.15223](https://arxiv.org/html/2405.15223v3) · [arXiv 2605.30263](https://arxiv.org/abs/2605.30263)

---

## 7. Stanford — Jiajun Wu

**Line:** controllable, action-conditioned 3D/4D world generation; physical scene understanding.

- **WonderWorld** (CVPR 2025) — interactive 3D scene generation from a single image.
- **WonderPlay** — action-conditioned 4D world generation and interaction (method details not verified).

Sources: [Stanford profile](https://profiles.stanford.edu/jiajun-wu)

---

## 8. Columbia University — Yunzhu Li

**Line:** structured, physics-informed, particle-based world models for manipulation.

- **DPI-Net** (ICLR 2019, with Wu, Tedrake, Tenenbaum, Torralba) — learned particle-based simulators; the origin of the line.
- **PhysTwin** (ICCV 2025) — physics-informed reconstruction and simulation of deformable objects from videos.
- **PhysWorld** ([2510.21447](https://arxiv.org/pdf/2510.21447v1)) — reports being 47× faster than PhysTwin (authorship not verified).

**Assessment:** the main academic alternative to scaling video models: data-efficient and physically grounded, but narrower in scope.

---

## 9. Stanford (Percy Liang, Sherry Yang, Chelsea Finn) & University of Toronto — World models as policy evaluators

- **WPE — Evaluating Robot Policies in a World Model** ([2506.00613](https://arxiv.org/html/2506.00613v1)) — underestimates in-distribution and overestimates out-of-distribution policy values, but keeps relative rankings.
- **WorldGym** (ICLR 2026) — policy success inside the world model correlates strongly with real-world success.
- **Ctrl-World** (Stanford + Tsinghua, [2510.10125](https://arxiv.org/html/2510.10125v3)) — ranks policies without real robot rollouts.
- **dWorldEval** (Toronto + Current Robotics, [2604.22152](https://arxiv.org/pdf/2604.22152)) — discrete-diffusion evaluator; reports beating WorldEval, Ctrl-World and WorldGym.
- **WMBench** ([2607.02642](https://papers.cool/arxiv/2607.02642)) — 7 video world models, more than 324k simulated rollouts paired with real executions.

---

## 10. HKU / OpenDriveLab — Hongyang Li (with HKUST, CUHK)

**Line:** generative world models for autonomous driving.

- **GenAD** (CVPR 2024 Highlight) — latent-diffusion driving video prediction; OpenDV dataset with more than 1,700 hours.
- **Vista** (NeurIPS 2024) — +55% FID and +27% FVD over the previous best driving world model; multiple action modalities (steering, speed, trajectories, goals).
- **ReSim** (NeurIPS 2025) — adds hazardous and non-expert behavior via simulator data; up to +44% visual fidelity, +2% NAVSIM planning, +25% policy selection.

Sources: [Vista arXiv 2405.17398](https://arxiv.org/html/2405.17398v4) · [ReSim](https://opendrivelab.com/ReSim)

---

## 11. Shanghai AI Laboratory — Physical Intelligence Team

- **A Definition and Roadmap for World Models** ([2607.06401](https://arxiv.org/abs/2607.06401), July 2026) — 58-page technical report proposing a definition and roadmap for the field.

---

## Industry reference points (context only)

Academic papers usually compare against these:

- **Dreamer 4** (Google DeepMind; Hafner, Yan, Lillicrap) — first agent to obtain diamonds in Minecraft from offline data only (about 0.7% success); 100× less data than VPT; 9.6 s context; real-time. [arXiv 2509.24527](https://arxiv.org/pdf/2509.24527)
- **V-JEPA 2-AC** (Meta FAIR) — more than 1M hours of video pretraining, under 62 h of robot post-training, zero-shot pick-and-place.
- **JEPA-WM ablation study** (Meta FAIR, Dec 2025) — systematic study of architecture, objective and planner; beats DINO-WM and V-JEPA-2-AC. [arXiv 2512.24497](https://arxiv.org/abs/2512.24497v1)

---

## Benchmarks & surveys

**Physical consistency remains the main weakness:**
- **WorldBench** ([2601.21282](https://arxiv.org/html/2601.21282v1)) — no tested model was physically consistent enough for reliable real-world interaction.
- **WoW-World-Eval** ([2601.04137](https://arxiv.org/html/2601.04137v1)) — best physical consistency score: 68.02.
- **World Models' Last Exam in Physics** (Oct 2026, [2610.08791](https://www.alphaxiv.org/abs/2610.08791)) — best of 8 models: 57.76/100.
- **WorldArena** ([2602.08971](https://arxiv.org/pdf/2602.08971)) — evaluates perceptual quality and usefulness for embodied tasks.

**Surveys:**
- A Comprehensive Survey on World Models for Embodied AI ([2510.16732](https://arxiv.org/pdf/2510.16732))
- World Model for Robot Learning: A Comprehensive Survey ([2605.00080](https://www.emergentmind.com/papers/2605.00080))
- A Survey: Learning Embodied Intelligence from Physical Simulators and World Models ([2507.00917](https://arxiv.org/html/2507.00917v1))

---

## Known gaps

- Several 2026 preprints in the JEPA planning and policy-evaluation sections could not be tied to specific groups.
- No specific 2025–26 world-model papers were found for Berkeley (Levine, Abbeel), CMU, Oxford or Mila. This reflects search coverage, not absence of work.
- Latent-Foresight's venue (possibly NeurIPS 2026) and Re2Pix's ECCV 2026 acceptance come from secondary sources.
