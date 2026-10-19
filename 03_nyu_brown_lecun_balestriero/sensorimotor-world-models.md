---
citation_count: 14
citation_source: "Semantic Scholar, 2026-10-13 (carried over from existing note; live recheck returned HTTP 429 on 2026-10-15)"
venue: "NeurIPS 2026 (poster)"
date: 2026-06-18
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Sensorimotor World Models: Perception for Action via Inverse Dynamics

- **Authors:** Petr Ivashkov, **Randall Balestriero**, Bernhard Schölkopf
- **Group role:** Co-author (Balestriero, second author). Last author is Bernhard Schölkopf (MPI for Intelligent Systems / ELLIS Tübingen / ETH Zürich), who is not a PI of another group in the summary, so the paper stays in this folder. Affiliations: MPI-IS Tübingen, Brown University, ELLIS Institute Tübingen, ETH Zürich.
- **Date published:** 2026-09 (NeurIPS 2026 acceptance; author notification 2026-09-24; conference in December 2026). arXiv v1: 2026-06-18.
- **Venue:** NeurIPS 2026, main conference, poster ("Accept (poster)", Paris Poster Session 4). The NeurIPS listing uses the title "Perception for Action in Latent World Models"; the arXiv title is "Sensorimotor World Models: Perception for Action via Inverse Dynamics" (same authors).
- **Links:** arXiv https://arxiv.org/abs/2606.20104 · NeurIPS page https://neurips.cc/virtual/2026/poster/153770 · OpenReview https://openreview.net/forum?id=InDOasTozM · Code https://github.com/petr-ivashkov/sensorimotor-world-model
- **Number of citations:** 14 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
SMWM is a JEPA-style latent world model (encoder + action-conditioned forward predictor) trained end-to-end from pixels, where the only anti-collapse mechanism is an inverse-dynamics head that must predict the action from consecutive embeddings. This single extra loss (one hyperparameter λ) prevents collapse and pushes the latent space to keep exactly the controllable degrees of freedom while ignoring uncontrollable distractors. Planning follows LeWM (CEM + MPC).

## 1. Weights availability
Not released as far as found. The README describes training and reproduction scripts; no checkpoint links were found (README checked via GitHub API, 2026-10-13).

## 2. Code
- https://github.com/petr-ivashkov/sensorimotor-world-model, MIT license, last push 2026-07-29 (GitHub API).
- `toy/` (dot-world and sprite experiments, runs on CPU/macOS) and `planning/` (TwoRoom, Reacher, Push-T, OGBench-Cube; "Reproduces the paper's results", needs one CUDA GPU). The `planning/` part derives from the LeWorldModel code and runs through stable-worldmodel.

## 3. Datasets
Same four offline datasets as LeWorldModel (Appendix B), episode-level 90/10 train/validation split:
- TwoRoom: 10,000 noisy scripted rollouts, mean 92 steps.
- Push-T: DINO-WM expert demonstrations, 20,000 episodes, mean length 196.
- OGBench-Cube (single cube): 10,000 scripted trajectories × 200 steps.
- DMC Reacher (DINO-WM variant): 10,000 SAC-generated episodes × 200 steps.
- Toy: dot world (64×64 images, 2-D to 6-D controllable state, optional random distractor dot) and a triangular sprite with pose (x, y, θ) (Appendix F).

## 4. Evaluation and downstream tasks
- Goal-conditioned planning (Fig. 5, 5 seeds, 50-step budget, goal 25 steps ahead): SMWM roughly matches the SIGReg (LeWM-style) baseline on TwoRoom, Reacher and Push-T and beats it on OGBench-Cube (84% vs 59%). Both beat Forward-only (λ = 0) and Random. Exact numbers for the 2D tasks are only shown in the figure.
- Longer goal offsets (Appendix C, Fig. 9): evaluated with a 2× step budget; not summarized numerically in the text.
- Physical-state probing (Table 1, held-out R², linear / MLP): TwoRoom agent position 1.000 / 1.000 (SIGReg 0.996 / 1.000; Forward-only 0.832 / 0.991); Push-T block orientation 0.664 / 0.882 (SIGReg 0.931 / 0.987); Cube gripper opening 0.989 / 0.992 (Forward-only 0.269 / 0.427).
- Latent geometry (Figs. 2, 4, 6): PCA rank matches the controllable dimension (2, 4, 2, 6 in dot-world variants); a random distractor dot is ignored; Reacher angles appear as a torus (four principal components); the TwoRoom wall shows up as an empty band.

## 5. Architecture and techniques
- Encoder: ViT-Tiny (about 5M parameters), CLS token projected to d = 192 (LeWM encoder); small CNN with d = 64 for toy worlds.
- Forward model g_φ: small transformer (about 10M parameters) predicting z_{t+1} from (z_t, a_t), action injected with AdaLN-zero (LeWM predictor); 2-layer MLPs in toy worlds.
- Inverse model h_ψ: 2-layer MLP (width 256) mapping [z_t; z_{t+1}] to the action.
- Objective: L = L_fwd (MSE in latent space) + λ·L_inv (MSE on actions); no frozen encoder, EMA target, stop-gradient or distributional regularizer.
- Planner: CEM on terminal latent distance to the goal embedding, receding-horizon MPC.
- Theory: if encoder and forward model are exactly equivariant, action composition maps to composition of latent transitions (Theorem 1); empirically, actions act as near-translations z + ρ(a).

## 6. Conclusions and future work
- Authors: a single inverse-dynamics head is enough to train a JEPA world model end-to-end and gives action-aligned, low-dimensional latents with planning on par with or better than SIGReg. Limitations: actions must be recoverable from consecutive frames; single-frame encoding misses velocities; action-aligned bias may drop information needed by other tasks; biased behavior policies can create action-correlated distractors; usual offline-world-model limits (coverage, compounding error, moderate-scale simulation only). Future work: multi-frame and multi-step inverse objectives, more robust data regimes, using the inverse model inside latent or hierarchical planners.
- Own observations: the only clear planning gain is on OGBench-Cube; on the three 2D tasks results are a tie with SIGReg. SIGReg gives better linear probes on Push-T block pose, which the inverse loss does not directly encourage because the block is not directly actuated.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2606.20104 and PDF (Sections 3–6, Table 1, Fig. 5, Appendix B).
- Venue: NeurIPS 2026 accepted-papers JSON lists "Perception for Action in Latent World Models" by Ivashkov, Balestriero, Schölkopf with decision "Accept (poster)"; the matching arXiv paper was identified by authors and topic.
- Code: GitHub API (README, license, push date).
- Citations: Semantic Scholar API (arXiv:2606.20104).
- Not verified: exact success rates for TwoRoom, Reacher and Push-T (figure only); whether the camera-ready content differs from arXiv v1 beyond the title.
