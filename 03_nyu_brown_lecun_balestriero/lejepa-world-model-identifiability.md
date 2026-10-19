---
citation_count: 30
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; 2026-10-15 re-check rate-limited)"
venue: "NeurIPS 2026 (poster); arXiv preprint"
date: 2026-05-25
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# When Does LeJEPA Learn a World Model?

- **Authors:** David Klindt, **Yann LeCun**, **Randall Balestriero**
- **Group role:** Senior (last) author: Balestriero (Brown); LeCun co-author (NYU). First author David Klindt is at Cold Spring Harbor Laboratory.
- **Date published:** 2026-09 (NeurIPS 2026 acceptance; the NeurIPS 2026 author-notification date in the call for papers is 2026-09-24; conference in December 2026). arXiv v1: 2026-05-25.
- **Venue:** NeurIPS 2026, main conference, poster ("Accept (poster)", Sydney Poster Session 5, per the NeurIPS 2026 accepted-papers data).
- **Links:** arXiv https://arxiv.org/abs/2605.26379 · NeurIPS page https://neurips.cc/virtual/2026/poster/152207 · OpenReview https://openreview.net/forum?id=UmYyYHEj9P · Code https://github.com/klindtlab/lejepa-identifiability
- **Number of citations:** 30 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
A theory paper: it proves that LeJEPA (an alignment loss between positive pairs plus a Gaussian regularizer on embeddings, SIGReg) recovers the true latent state of the world up to a rotation (linear identifiability) when latents are Gaussian and evolve by stationary additive-noise (Ornstein–Uhlenbeck) transitions, and that the Gaussian is the only latent distribution for which this holds. It also shows the guarantee degrades gracefully when objectives are only approximately met, and that orthogonal identifiability makes latent-space planning exactly as good as planning in the true state space.

## 1. Weights availability
Not applicable / not released. The paper trains small MLP and CNN encoders for validation experiments; no pretrained weights are linked (paper and repo checked 2026-10-13).

## 2. Code
- https://github.com/klindtlab/lejepa-identifiability ("Simulations and identifiability proof for LeJEPA"), MIT license, last push 2026-05-27 (GitHub API).
- Contains the simulation experiments; the paper states all proofs were checked in the Lean 4 theorem prover, modulo axiomatized background lemmas (Appendix G).

## 3. Datasets
- Synthetic: Gaussian latents (2-D to 1,024-D) with nonlinear mixings (spiral rotation, sinusoidal shear, parabolic shear, RealNVP coupling); generalized-normal latents with shape α for the converse.
- Pixel-based: DeepMind Control Reacher renderings, with (i) OU-sampled joint angles or (ii) joint-angle pairs from 10k RL episodes from the LeWorldModel data (Section 6.2).

## 4. Evaluation and downstream tasks
- Metric: linear identifiability R² (linear regression from learned embedding h to true latent z, and back); recovery error vs the bound of Theorem 3; control cost of latent straight-line plans (ideal 1).
- Scaling (Table 1, RealNVP mixing, 5 seeds): SIGReg R² 0.999998 at N = 2 and 0.999561 at N = 1,024; VICReg similar (0.999582 at 1,024); InfoNCE degrades at scale (0.720 at 1,024) with a fixed kernel width; raw observations give R² ≈ 0.73–0.78.
- Converse (Fig. 4b): linear recovery peaks sharply at α = 2 (Gaussian) for SIGReg, VICReg and InfoNCE.
- Reacher (Table 2): OU-sampled pairs reach R² 0.95 at ρ = 0.99; real RL-trajectory pairs never exceed total R² 0.5, with anisotropic per-joint recovery.
- Bound check (Fig. 4a): the Theorem 3 bound holds across runs, except two near-zero outliers attributed to finite-sample noise.
- Planning (Fig. 4c–d, Fig. 5): over 30 random start–goal pairs, straight-line plans in the Gaussian-trained encoder's latent match the oracle joint-space cost; the trajectory-trained encoder inflates the cost; cost decreases monotonically with R².

## 5. Architecture and techniques
- Learner: LeJEPA objective = alignment E‖h(z′) − h(z)‖² subject to h(z) ~ N(0, I) (in practice λ·SIGReg + (1 − λ)·invariance).
- Analysis tools: spectral decomposition of the transition operator; Hermite polynomials (eigenvalue ρ^d for degree d) for the forward result; Sturm–Liouville theory for the converse; an alternative proof via Dirichlet energy and Mazur–Ulam (Appendix E).
- Theorems: 5.1 linear identifiability (h = Qz, Q orthogonal, at the optimum); 5.2 Gaussian uniqueness; 5.3 approximate identifiability E‖h(z) − Qz‖² ≤ D + (ε + D)²; 5.4 equal optimal value and plan for O(n)-invariant costs.
- Experimental encoders: 4-layer MLP (2-D), RealNVP-matched encoder (scaling), CNN (Reacher pixels).

## 6. Conclusions and future work
- Authors: LeJEPA provably learns the world's latent structure in Gaussian worlds, which turns a recipe into a guarantee and explains why linear probes and simple latent planners work. Limitations: real latents may not be Gaussian (they argue for it via maximum entropy and the central limit theorem); the result assumes encoder dimension equals latent dimension; it is a population-level result that does not cover finite samples or training dynamics. Outlook: extend identifiability to the action-conditioned transition (interventional causal representation learning); exploration data close to an isotropic random walk keeps pretraining inside the theory; compare SIGReg, VICReg and InfoNCE estimators in practice.
- Own observations: the result covers the encoder (state) side only, not the action-conditioned predictor that makes a world model. The Reacher experiment shows that typical policy-collected data violates the assumptions (R² ≤ 0.5), so practical relevance for LeWM-style training on agent trajectories is limited without isotropic exploration data.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2605.26379 and PDF header (Sections 3–8, Tables 1–2, Fig. 4).
- Venue: NeurIPS 2026 accepted-papers JSON (https://neurips.cc/static/virtual/data/neurips-2026-orals-posters.json) lists the paper with decision "Accept (poster)"; NeurIPS 2026 call for papers for the notification date.
- Code: GitHub API.
- Citations: Semantic Scholar API (arXiv:2605.26379).
- Not verified: whether the camera-ready title or content differs from arXiv v1; a precise publication date (the conference has not yet taken place on 2026-10-13).
