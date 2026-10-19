---
citation_count: 42
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live re-check rate-limited)"
venue: "ICML 2026"
date: 2026-03-12
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Temporal Straightening for Latent Planning

- **Authors:** Ying Wang, Oumayma Bounou, Gaoyue Zhou, **Randall Balestriero**, Tim G. J. Rudner, **Yann LeCun**\*, Mengye Ren\* (* equal advising)
- **Group role:** Co-senior author (LeCun, equal advising with the last author); Balestriero co-author. Last author is Mengye Ren (NYU, Agentic Learning AI Lab), who is not a PI of another group in the summary, so the paper stays in this folder. Affiliations: NYU, Brown, University of Toronto.
- **Date published:** 2026-07 (ICML 2026 conference, PMLR vol. 306). arXiv v1: 2026-03-12.
- **Venue:** ICML 2026, poster (icml.cc/virtual/2026/poster/64904; OpenReview venue "ICML 2026 regular").
- **Links:** arXiv https://arxiv.org/abs/2603.12231 · Proceedings https://proceedings.mlr.press/v306/wang26n.html · ICML page https://icml.cc/virtual/2026/poster/64904 · Project page https://agenticlearning.ai/temporal-straightening/ · Code https://github.com/agentic-learning-ai-lab/temporal-straightening
- **Number of citations:** 42 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper trains a JEPA world model (encoder + action-conditioned ViT predictor) with an added curvature regularizer that maximizes the cosine similarity between consecutive latent velocity vectors, so that latent trajectories become locally straight. Straighter latents make Euclidean distance a better proxy for geodesic distance and make gradient-based planning better conditioned, which raises goal-reaching success with a simple gradient-descent planner.

## 1. Weights availability
Not released as far as found. The README and project page link only code; no checkpoint links were found (GitHub README and project page checked 2026-10-13).

## 2. Code
- https://github.com/agentic-learning-ai-lab/temporal-straightening, MIT license, last push 2026-09-10 (GitHub API).
- Training and planning code (encoders `dino_global`, spatial projector, ResNet; GD/CEM/MPC planners). The README lists post-publication bug fixes: predictor sizing for the global projector (the affected Table 1 row improves), PushT success check angle normalization, and a train/val split leak; the authors say they retrained all models and "fully reproduce the reported results" (see UPDATES.md in the repo).

## 3. Datasets
Simulated, generated as in DINO-WM (Appendix A):
- Wall: 1,920 trajectories × 50 steps.
- PointMaze UMaze: 2,000 trajectories; Medium maze: 4,000 trajectories; 100 steps each (D4RL MuJoCo).
- PushT: DINO-WM training data, 18,500 trajectories of length 100–300.
- Teleported-PointMaze: new variant where touching the right wall teleports the agent left (Appendix F).
- Test: 50 episodes, start/goal from test trajectories reachable within 25 steps (frameskip 5, H = 5); long-horizon setting with goals 50 steps away.

## 4. Evaluation and downstream tasks
- Metric: goal-reaching success rate (%) over 50 test episodes, 3 seeds; open-loop and MPC with a gradient-descent planner.
- Table 1, best configuration (DINOv2 patch features + trainable projector to 14×14×8, with straightening) vs DINO-WM (frozen DINOv2 patch, 14×14×384), open-loop / MPC:
  - Wall: 90.67 / 100.00 vs 52.67 / 76.67
  - UMaze: 94.00 / 100.00 vs 35.33 / 80.67
  - Medium maze: 82.67 / 98.67 vs 40.83 / 76.67
  - PushT: 77.33 / 85.33 vs 56.00 / 66.00
- ResNet trained from scratch (14×14×8) with straightening: PushT MPC 91.33 (best PushT number); without straightening this encoder degrades badly on mazes (e.g. Wall MPC 6.67).
- Long horizon (Table 2, goals 50 steps away): PushT MPC 33.33–36.00 vs DINO-WM 27.33; Medium maze MPC 98.67 (ResNet + straightening) vs 65.33.
- CEM vs GD (Appendix B.3): straightening helps both; CEM gets higher success but is much slower; GD + straightening gives the better success–latency trade-off.
- Straightening beats smoothness and temporal-contrastive regularizers (Appendix B.5).

## 5. Architecture and techniques
- Encoder options: frozen DINOv2 + trainable light CNN projector (spatial 14×14×8 or global 1×384), or ResNet trained from scratch.
- Predictor: ViT with frame-level causal mask, history K; action and proprioception embeddings concatenated to visual tokens.
- Losses: L_pred = MSE(ẑ_{t+1}, stop-grad(z_{t+1})) + λ·(1 − cos(v_t, v_{t+1})), λ typically 0.1; stop-gradient is the only anti-collapse mechanism. For spatial features, curvature is computed on a learnable aggregation head.
- Theory: for linear latent dynamics, ε-straight transitions (‖A − I‖ ≤ ε) bound the planning Hessian's condition number by κ(B)²((1+ε)/(1−ε))^{2(K−1)} (Theorem 4.4).
- Planner: gradient descent on terminal (and weighted intermediate) latent MSE; MPC replans every step. A combined spatial + 0.1 × aggregated cost helps long-horizon planning.
- Parameter count: not stated in the main text.

## 6. Conclusions and future work
- Authors: representation geometry matters for latent planning; straightening gives consistent gains on 2D goal-reaching tasks and makes gradient planning viable. Limitations and directions: symmetric Euclidean goal cost may be unsuitable for asymmetric or irreversible dynamics (quasimetrics suggested); planning and regularization may be better done in a projected space separate from the prediction space; nonlinear-dynamics guarantees are open; more challenging environments.
- Own observations: all tasks are 2D, low-visual-complexity simulations with short horizons; long-horizon PushT success stays low (≤ 36%). Results depend on λ chosen per environment by MPC on validation seeds. The repo's disclosed train/val leak and re-run numbers mean the arXiv/PMLR tables may differ slightly from the code's current outputs.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2603.12231 (Sections 3–6, Tables 1–2, Appendix A, B).
- Venue: PMLR v306 index (wang26n, seven authors), ICML 2026 poster page, NYU CDS blog post stating ICML 2026 acceptance.
- Code: GitHub API (license, push date, README).
- Citations: Semantic Scholar API (arXiv:2603.12231).
- Not verified: parameter counts; whether released numbers in UPDATES.md change any headline result beyond the one row the README mentions.
