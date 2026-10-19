---
citation_count: 5
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICLR 2025"
date: 2024-12-12
research_group: "Columbia (Yunzhu Li)"
---

# BaB-ND: Long-Horizon Motion Planning with Branch-and-Bound and Neural Dynamics

- **Authors:** Keyi Shen\*, Jiangwei Yu\*, Jose Barreiros, Huan Zhang, **Yunzhu Li** (\* equal contribution)
- **Group role:** Senior (last) author, affiliated with Columbia.
- **Date published:** 2025-04 (ICLR 2025, Singapore, 24 to 28 April 2025). arXiv v1 2024-12-12 (v3 2025-03-16).
- **Venue:** International Conference on Learning Representations (ICLR) 2025. The PDF header reads "Published as a conference paper at ICLR 2025". OpenReview forum https://openreview.net/forum?id=JXKFPJe0NU, from the repo BibTeX. Poster or spotlight status not verified.
- **Links:** arXiv https://arxiv.org/abs/2412.09584 · OpenReview https://openreview.net/forum?id=JXKFPJe0NU · Project https://robopil.github.io/bab-nd/ · Code https://github.com/keyis2/BaB-ND
- **Number of citations:** 5 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
BaB-ND is a planner for learned neural dynamics world models (MLPs over keypoints, GNNs over object sets). It is a GPU-accelerated branch-and-bound method adapted from the α,β-CROWN neural-network verifier:
- **Branching:** splits the action-sequence space using heuristics based on the top samples.
- **Bounding:** estimates objective lower bounds with a modified CROWN, using propagation early-stop and empirical pre-activation bounds taken from search samples. Subdomains whose bound cannot beat the best solution are pruned.
- **Searching:** runs CEM inside the promising subdomains.

It finds better long-horizon, contact-rich plans than gradient descent, MPPI and CEM, and it scales far beyond MIP-based planning.

## 1. Weights availability
- The README provides scripts that reproduce the qualitative and quantitative results for pushing with obstacles and object sorting. Their outputs are compared with `output_sample/`.
- The README does not list separately downloadable dynamics-model checkpoints. Whether the trained models used by these scripts are committed to the repo was not verified.
- License: MIT (repository LICENSE, copyright Keyi Shen and Jiangwei Yu).

## 2. Code
- https://github.com/keyis2/BaB-ND, MIT license. The README says it is an "initial release for reproducibility" (December 2024), and that a more accessible version would be released with α,β-CROWN in 2025.
- Contains:
  - The synthetic-function test.
  - Pushing-with-obstacles and object-sorting experiments.
  - The planner, built on α,β-CROWN in `Verifier_Development`.
- Tested on Ubuntu 22.04 with PyTorch 2.1.0, CUDA 12.1 and an RTX 4090.
- The README does not list dynamics-model training scripts. Last-commit date not verified.

## 3. Datasets
All training data is simulated (App. D.2):
- **Pushing with obstacles:** Pymunk, 32,000 episodes × 30 pushes.
- **Object merging:** Pymunk, 64,000 episodes × 40 pushes.
- **Rope routing:** NVIDIA FleX, 15,000 episodes × 6 random 3D actions, split into 2 cm segments.
- **Object sorting:** Pymunk, 32,000 episodes × 12 long pushes.

No public dataset release was found.

## 4. Evaluation and downstream tasks
- **Synthetic non-convex function** (Σ 5u_i + cos 50u_i, d up to 100; Fig. 4): BaB-ND reaches optimality on 98 to 100 of 100 dimensions. GD, MPPI and CEM converge to sub-optima.
- **Open-loop planning in simulation (Fig. 6a):** BaB-ND reaches lower objectives than GD, MPPI and CEM on pushing with obstacles and object merging. It is comparable to MPPI and CEM on rope routing, and on object sorting it has a similar median to CEM but lower variance. Values are box plots only.
- **Closed-loop on a real robot (Fig. 6b),** compared with MPPI and CEM: lower final cost on pushing with obstacles, merging and sorting, and higher success rate on rope routing (plots only).
- **Scalability (Fig. 7):**
  - Against MIP planning (Liu et al. 2023, the sparse-neural-dynamics paper), MIP gave optimal solutions in only 6 of 36 settings, suboptimal in 3, and failed in the rest within 300 s.
  - BaB-ND scales to horizon H = 20 and models with more than 500K parameters. Branching and bounding time grows slowly as model size grows more than 50-fold.
- **Downstream tasks:**
  - Long-horizon manipulation planning: non-prehensile T-pushing with obstacles, merging of L-shaped objects, rope routing into a slot, and multi-object sorting.
  - Execution with MPC trajectory tracking.

## 5. Architecture and techniques
- **World models used:**
  - MLPs, e.g. [128, 256, 256, 128] for pushing. The input is 4 object keypoints relative to the pusher plus the action, and the model is trained with a 6-step rollout MSE (App. D.3).
  - A GNN for object sorting.
- **Planner:**
  - Branch-and-bound over box-constrained action sequences.
  - Subdomain selection mixes the n1 subdomains with the best upper bounds and a softmax sample over lower bounds (Eq. 2).
  - The split dimension is ranked by (range × imbalance of top samples).
  - Bounds come from a CROWN variant with early stop and empirical intermediate bounds. Search uses decentralised CEM.
- **Paradigm:** deterministic neural dynamics with no generative modelling. The paper's contribution is the planning algorithm.

## 6. Conclusions and future work
- **Authors:** BaB with specialised branching and adapted bound propagation gives better long-horizon planning over neural dynamics, and it scales across architectures.
- **Authors' limitations (App. A.4):**
  - Performance depends on the accuracy of the dynamics model, so plans can overshoot in reality. They suggest robust formulations that account for model error.
  - Optimality is limited by the underlying sampling search; better samplers and parameter schedules could help.
  - Branching heuristics need improvement and could be learned, possibly with RL.
- **My own observations:**
  - The world models here are small, low-dimensional and trained in simulation. The paper is mainly a planning contribution that uses a world model, not a new world model.
  - Results are mostly plots.
  - The citation count is low for an ICLR paper (5).

## 7. Sources and verification
- Paper: https://arxiv.org/pdf/2412.09584 (v3, main text, App. A.4 and App. D read).
- README and LICENSE: raw.githubusercontent.com (keyis2/BaB-ND).
- Project page: https://robopil.github.io/bab-nd/
- Citations: Semantic Scholar batch API.
- **Not verified:**
  - OpenReview decision page and presentation type (forum ID taken from the repo BibTeX).
  - Whether checkpoints are committed.
  - Repository last update.
