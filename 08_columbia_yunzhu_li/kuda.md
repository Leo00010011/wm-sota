---
citation_count: 7
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live re-check rate-limited on 2026-10-15)"
venue: "ICRA 2025"
date: 2025-03-13
research_group: "Columbia (Yunzhu Li)"
---

# KUDA: Keypoints to Unify Dynamics Learning and Visual Prompting for Open-Vocabulary Robotic Manipulation

- **Authors:** Zixian Liu\*, Mingtong Zhang\*, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author, affiliated with Columbia.
- **Date published:** 2025-05 (ICRA 2025, Atlanta, 19 to 23 May 2025). arXiv v1 2025-03-13.
- **Venue:** IEEE International Conference on Robotics and Automation (ICRA) 2025. IEEE DOI 10.1109/ICRA55743.2025.11128038 (from Semantic Scholar). The project page also lists it as a spotlight at the CoRL 2024 LangRob workshop and a Best Paper Finalist there (workshop only).
- **Links:** arXiv https://arxiv.org/abs/2503.10546 · IEEE https://doi.org/10.1109/ICRA55743.2025.11128038 · Project http://kuda-dynamics.github.io · Code https://github.com/StoreBlank/KUDA
- **Number of citations:** 7 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
KUDA connects a vision-language model to learned neural-dynamics world models through keypoints:
1. SAM segments the RGB image, and farthest-point sampling places labelled keypoints and reference points on it.
2. GPT-4o, given few-shot examples retrieved with CLIP, writes code that sets target positions for chosen keypoints, as reference-point offsets.
3. These targets become a 3D cost.
4. MPPI optimises robot actions through a pre-trained world model: graph-based neural dynamics for rope, cubes and granular piles, and an MLP for a T-block.

There are two loops: a low-level MPC loop and a high-level loop that re-prompts the VLM.

## 1. Weights availability
- Released. The README links dynamics-model checkpoints on Google Drive (https://drive.google.com/drive/folders/1szjoTHBZH1kYE_V_1-bv8uVcsd9lxzVT).
- Third-party perception checkpoints (GroundingDINO, SAM-HQ, SpaTracker) are downloaded from their own sources.
- License: MIT (repository LICENSE, copyright 2025 by the three authors).

## 2. Code
- https://github.com/StoreBlank/KUDA, MIT license. It contains the full system: perception, VLM prompting, the dynamics models and planning. The README also links 3D-printable end-effectors.
- Whether dynamics training code is included was not verified; the README describes downloading the dynamics checkpoints.
- Last-commit date not verified.

## 3. Datasets
- The dynamics models are trained in simulation (Sec. V limitations). The paper does not give dataset sizes; the main text says only that models were trained for 4 object categories. The appendix on the project page was not read.
- Evaluation is 6 real-world tasks × 10 trials (Table II instructions). No public dataset release was found.

## 4. Evaluation and downstream tasks
- **Success rate on a real robot** (Chamfer distance threshold to the target after a fixed number of actions; Table I). All language models are GPT-4o.

  | Task | KUDA | MOKA | VoxPoser |
  |---|---|---|---|
  | Rope straightening | 8/10 | 2/10 | 0/10 |
  | Cube collection | 6/10 | 0/10 | 3/10 |
  | Cube movement | 10/10 | 6/10 | 3/10 |
  | Granular collection | 10/10 | 0/10 | 1/10 |
  | Granular movement | 6/10 | 0/10 | 1/10 |
  | T movement | 8/10 | 0/10 | 0/10 |
  | **Total** | **80.0%** | **13.3%** | **13.3%** |

  - The per-cell assignment for the cube and granular rows was reconstructed from garbled PDF text. It is consistent with the stated totals and with the paper's statement that KUDA's failures were in cube collection and granular movement.
- **Error breakdown over 60 trials (Fig. 4):** 58 keypoint-specification successes, 51 grounding, 49 tracking and 48 manipulation. Perception causes half of all failures (10% of trials); the dynamics model caused 1 failure.
- **Top-K prompt ablation on rope straightening (Table III):** human-picked "category" 10/10, Top-0 2/10, Top-1 3/10, Top-3 10/10, Top-5 7/10.
- **Downstream tasks:** open-vocabulary, language-conditioned manipulation of ropes, cubes, granular piles and a T-block via model-based planning.

## 5. Architecture and techniques
- **World models:**
  - Graph-based neural dynamics (particle GNN, from the group's earlier GBND work) for rope, cubes and granular piles.
  - A state-based MLP for the T-block.
  - All trained in simulation.
- **Target specification:** GPT-4o writes code of the form p = r + [dx, dy, dz] over labelled keypoints. A CLIP-based top-K retriever over a prompt library scores examples as image similarity + 0.6 × text similarity (Eq. 3).
- **Cost:** the sum of L2 distances between tracked keypoints and their targets (Eq. 4).
- **Planner:** MPPI-based MPC. The VLM is re-queried every few actions.
- **Perception:** GroundingDINO, SAM or SAM-HQ, and SpaTracker for tracking.
- **Size:** parameter counts not stated.

## 6. Conclusions and future work
- **Authors:** keypoints unify VLM visual prompting and dynamics-model planning, which enables open-vocabulary manipulation of deformable and granular objects.
- **Authors' limitations:**
  - A single top-down camera limits complex 3D spatial tasks.
  - Simulation-trained dynamics give a sim-to-real gap and limited cross-category generalisation.
- **My own observations:**
  - The world model is a component here; the contribution is the VLM-to-cost interface.
  - Evaluation is small (10 trials per task) and depends on a proprietary VLM (GPT-4o).
  - Dataset details for the dynamics models are not in the main paper.

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2503.10546 (v1, full main text read).
- README and LICENSE: raw.githubusercontent.com (StoreBlank/KUDA).
- Project page: https://kuda-dynamics.github.io
- Citations and DOI: Semantic Scholar batch API.
- **Not verified:**
  - The project-page appendix (dynamics training data).
  - The exact Table I cell layout, as reconstructed above.
  - Repository last update.
