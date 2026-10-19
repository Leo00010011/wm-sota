---
citation_count: 9
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live re-check returned HTTP 429 on 2026-10-15)"
venue: "ICML 2026"
date: 2026-01-31
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Parallel Stochastic Gradient-Based Planning for World Models (GRASP)

- **Authors:** Michael Psenka, Michael Rabbat, Aditi S. Krishnapriyan, **Yann LeCun**, Amir Bar
- **Group role:** Co-author (LeCun, second to last). Last author is Amir Bar (Meta FAIR, first author of NWM), who is not a PI of another group in the summary, so the paper stays in this folder. Corresponding author: Michael Psenka (UC Berkeley).
- **Date published:** 2026-07 (ICML 2026 conference, PMLR vol. 306). arXiv v1: 2026-01-31; the arXiv HTML version is dated 2026-08-24.
- **Venue:** ICML 2026 (listed in the PMLR v306 index). Presentation type not verified. An earlier version was at the ICLR 2026 World Models workshop.
- **Links:** arXiv https://arxiv.org/abs/2602.00475 · Proceedings https://proceedings.mlr.press/v306/psenka26a.html · Project page https://michaelpsenka.io/grasp · Code https://github.com/michael-psenka/grasp
- **Number of citations:** 9 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
GRASP (Gradient RelAxed Stochastic Planner) is a planner for differentiable learned world models. It optimizes the actions together with "virtual" intermediate latent states, with soft one-step dynamics constraints, so all world-model calls run in parallel. It adds Gaussian noise to the states to explore, stops gradients through the world model's state inputs (only action gradients are used) to avoid adversarial state directions, and periodically runs a full-rollout gradient step. It is evaluated on DINO-WM world models and beats CEM and plain gradient descent at long horizons.

## 1. Weights availability
- No new world-model weights. The method plans with DINO-WM models; the repo points to DINO-WM's OSF bucket for data and checkpoints (https://osf.io/bmw48/?view_only=a56a296ce3b24cceaf408383a175ce28). Adapter repos for other world models are said to include their own pretrained-model links (not checked).

## 2. Code
- https://github.com/michael-psenka/grasp, license not identified by the GitHub API (NOASSERTION), last push 2026-04-06.
- A fork of DINO-WM with GRASP added (`python plan.py model_name=pusht planner=grasp ...`), plus a self-contained `grasp_pseudocode.py`. Training code is inherited from DINO-WM.

## 3. Datasets
- DINO-WM datasets and environments (simulated): PointMaze (D4RL), Wall-Single, Push-T. World models are trained with the DINO-WM framework, frameskip 5 (5 stacked actions per model step).
- Evaluation: 500 open-loop planning trials per setting; success means reaching the goal region within the horizon.

## 4. Evaluation and downstream tasks
- Metrics: success rate (%) and median wall-clock time to success (successful trials only).
- Long-horizon Push-T (Table 1), success / median time: H=40: GRASP 59.0 / 8.5 s vs CEM 61.4 / 35.3 s and GD 51.0 / 18.0 s; H=50: 43.4 / 15.2 s vs CEM 30.2 / 96.2 s, GD 37.6 / 76.3 s; H=60: 26.2 vs 7.2 (CEM) and 16.4 (GD); H=80: 10.4 vs 2.8 and 6.4. LatCo (lifted-state baseline) reaches at most 15.0.
- Short horizons (Table 3, H = 10–30): comparable success to CEM (e.g. Push-T H=30: 75.2 vs CEM 76.0, GD 69.0; PointMaze ≈ 100 for all but GD), with low times (Table 4, e.g. Push-T H=30: 9.1 s vs CEM 23.6 s, GD 25.3 s).
- Ablations (Table 2, Push-T H=40): no sync steps 1.6%; σ = 0 noise 54.8%; letting gradients flow through states 46.6%; full method 59.0%.

## 5. Architecture and techniques
- World model: DINO-WM (frozen DINOv2 patch features + ViT predictor); GRASP is model-agnostic.
- Planner: lifted (collocation / non-condensed) formulation; loss = Σ‖F(s̄_t, a_t) − s_{t+1}‖² + γ Σ‖F(s̄_t, a_t) − g‖² with stop-gradient on state inputs (Eq. 10); noisy state updates (σ = 0.5); every 100 stochastic steps, 25 steps of full-rollout GD on the terminal goal loss; states initialized as a noisy linear interpolation from start to goal, actions at zero.
- Theory: a loss whose minimizers are exactly feasible trajectories must depend on the state Jacobian ∇_s F (Theorem 1, Appendix A.4), which motivates the action-only gradient design.

## 6. Conclusions and future work
- Authors: GRASP is robust at long horizons, finds non-greedy solutions and converges faster than CEM or GD. Limitations: little gain at short horizons; extra structure and hyperparameters exist only to work around unreliable state gradients. Future work: hybrids with CEM; smoother latent geometry (adversarial training, diffusion world models, geometry regularization) that would make the stabilizers unnecessary.
- Own observations: success is measured under the learned world model in simulation, not by closed-loop execution on robots; absolute success at H ≥ 60 stays low (≤ 26%). This is a planning-algorithm paper; it does not train a new world model.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2602.00475 (Sections 3–7, Tables 1–4).
- Venue: PMLR v306 index (psenka26a) lists "Parallel Stochastic Gradient-Based Planning for World Models" with these five authors (checked 2026-10-13); OpenReview lists an ICLR 2026 World Models workshop version.
- Code: project page link and GitHub API.
- Citations: Semantic Scholar API (arXiv:2602.00475).
- Not verified: ICML presentation type; code license.
