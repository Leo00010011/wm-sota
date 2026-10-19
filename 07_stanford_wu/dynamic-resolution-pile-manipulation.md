---
citation_count: 47
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live re-check returned HTTP 429 on 2026-10-15)"
venue: "RSS 2023"
date: 2023-06-29
research_group: "Stanford (Jiajun Wu)"
---

# Dynamic-Resolution Model Learning for Object Pile Manipulation

- **Authors:** Yixuan Wang*, Yunzhu Li*, Katherine Driggs-Campbell, Li Fei-Fei, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author. Yunzhu Li, PI of summary group 8 and then at Stanford/UIUC, is co-first author.
- **Date published:** 2023-07 (RSS 2023, Daegu, July 2023; arXiv 2023-06-29)
- **Venue:** Robotics: Science and Systems (RSS) 2023, DOI 10.15607/RSS.2023.XIX.047. Won a Best Paper Award at the IROS 2023 Workshop on Learning Meets Model-Based Methods (per Wu's homepage).
- **Links:**
  - arXiv: https://arxiv.org/abs/2306.16700
  - Project: https://robopil.github.io/dyn-res-pile-manip/
  - Code: https://github.com/WangYixuan12/dyn-res-pile-manip
- **Number of citations:** 47 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper learns one GNN particle-dynamics world model that works at any particle resolution ω. At each MPC step, a self-supervised resolution regressor chooses how many particles to use for the current observation and goal. The regressor is trained on labels produced by Bayesian optimization of task progress under a fixed compute budget. Planning uses sampling plus gradient descent through the learned model.

The method is applied to pushing granular piles (coffee beans, almonds, corn, M&Ms, etc.) in three tasks: gather, redistribute and sort.

## 1. Weights availability
- **Released.** `scripts/download_model.sh` downloads two checkpoints from Google Drive with `gdown`:
  - a GNN dynamics model (`gnn_dyn_model/2023-01-28-...`)
  - a resolution regressor (`res_rgr_model/2023-01-30-...`)
- **License:** none declared.

## 2. Code
- **Repository:** https://github.com/WangYixuan12/dyn-res-pile-manip. Last commit 2024-01-29. No LICENSE file (raw `LICENSE` returns 404).
- **Contents:**
  - Modified PyFleX simulator (built in Docker)
  - Data generation (`data_gen/`)
  - Training (`train/`)
  - MPC planners (`planners.py`) and MPC visualization
  - Custom robot models
- **Downloads:** `scripts/download_data.sh` (data) and `scripts/download_model.sh` (models). Training and evaluation code are both included.

## 3. Datasets
- **Training data:** generated in an NVIDIA FleX (PyFleX) simulation of object piles pushed by a pusher. It is downloadable via `download_data.sh`. Dataset size is not stated in the arXiv main text; the supplementary material is not part of the arXiv PDF.
- **Resolution-regressor data:** randomly generated (observation, goal) pairs, labelled by Bayesian optimization (Sec. III-C).
- **Real-world evaluation:** a top-down RealSense D455 camera and a pusher on the robot. Piles of M&Ms, almonds, granola, candy, carrots, rice, corn and coffee beans.

## 4. Evaluation and downstream tasks
- **Efficiency vs. effectiveness trade-off (Fig. 4):** the optimal resolution changes with both the goal and the initial configuration. Complex goal shapes, or piles already close to the goal, need more particles.
- **Simulation MPC (Fig. 5):** for Gather and Redistribute, the adaptive model "almost always" has the highest task score (the fraction of trials below a distance threshold) across thresholds. It is compared against fixed resolutions and against a CNN dynamics model. Results are shown in plots only; no table is given.
- **Real world (Fig. 6):**
  - Gather corn. The adaptive model has the lowest task loss at every MPC step. Fixed resolutions of 75 or 100 particles do not converge within the time budget. A fixed 25 particles reaches a similar final loss but more slowly.
  - Gather almond, granola and M&M.
  - Redistribute into the letters J, T and U.
  - Sort two piles, using a high-level waypoint planner.
- **Downstream tasks:** model-based planning / MPC for gathering, redistribution and sorting.
- **Caveat:** quantitative results are reported only as plots, so I give no exact numbers.

## 5. Architecture and techniques
- **Perception h:** RGB-D → point cloud → colour and depth segmentation → farthest-point sampling down to ω particles.
- **Dynamics f:**
  - A GNN with node and edge encoders conditioned on the action u_t and the resolution ω.
  - Multi-step message passing, following DPI-Net.
  - Edges connect particles within a radius, with a cap on neighbours per node.
  - Trained with multi-step MSE on particle positions.
- **Resolution regressor g(y0, yg):**
  - Supervised on ω* labels.
  - Labels come from Gaussian-process Bayesian optimization of c*(ω) + R(ω), where R is a size regularizer and the time budget is fixed.
- **Planner:** at each MPC step, sample M action sequences, refine each with N gradient steps through the learned dynamics, and execute the first action.

## 6. Conclusions and future work
- **Authors' conclusions:** choosing the representation resolution adaptively online, with a single resolution-agnostic GNN dynamics model, gives better control than fixed-resolution models across three pile-manipulation tasks. No explicit limitations or future-work section appears in the main text.
- **Own observations:**
  - The dynamics model is trained in simulation (FleX). Sim-to-real transfer is shown qualitatively, not measured.
  - Real-world quantitative evaluation is limited to loss curves.
  - The action space is a 2D push with a flat pusher.

## 7. Sources and verification
- **Paper:** arXiv PDF v2 https://arxiv.org/pdf/2306.16700. Read Secs. I–V, Figs. 1–6 and Algorithm 1.
- **Code:** repo README and `scripts/download_model.sh` from https://github.com/WangYixuan12/dyn-res-pile-manip.
- **Venue:** Semantic Scholar API (`paper/arXiv:2306.16700`) confirms RSS. The README BibTeX gives the RSS DOI.
- **Not verified:**
  - Training dataset size: the supplementary material was not available.
  - The IROS workshop award: taken from Wu's homepage only.
