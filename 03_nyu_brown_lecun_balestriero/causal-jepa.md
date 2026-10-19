---
citation_count: 19
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; 2026-10-15 re-check rate-limited, HTTP 429)"
venue: "ICML 2026"
date: 2026-02-11
research_group: "NYU / Brown (Yann LeCun, Randall Balestriero)"
---

# Causal-JEPA: Learning World Models through Object-Level Latent Masking

- **Authors:** Heejeong Nam, Quentin Le Lidec*, Lucas Maes*, **Yann LeCun**, **Randall Balestriero** (* equal contribution)
- **Group role:** Senior (last) author: Balestriero (Brown); LeCun co-author. Code hosted by Balestriero's galilai-group.
- **Date published:** 2026-07 (ICML 2026 conference, PMLR vol. 306). arXiv v1: 2026-02-11; v2: 2026-05-28. (The arXiv v1 title used "Object-Level Latent Interventions"; the ICML/PMLR title uses "Masking".)
- **Venue:** ICML 2026 (43rd International Conference on Machine Learning), regular paper (OpenReview venue "ICML 2026 regular"). Also a poster at the CompLearn 2026 workshop.
- **Links:** arXiv https://arxiv.org/abs/2602.11389 · Proceedings https://proceedings.mlr.press/v306/nam26c.html · OpenReview https://openreview.net/forum?id=VMAHQDOtjp · Code https://github.com/galilai-group/cjepa · Weights https://huggingface.co/HazelNam/CJEPA
- **Number of citations:** 19 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
C-JEPA is an object-centric JEPA world model: a frozen slot encoder (VideoSAUR on DINOv2, or SAVi) turns frames into object slots, and a bidirectional transformer predictor is trained to recover masked object slots over the history and to predict future slots, conditioned on actions and proprioception as separate tokens. Masking whole objects (keeping only an identity anchor) forces the predictor to infer each object's state from the others, which the authors argue induces interaction-aware, causal-like dependencies.

## 1. Weights availability
- Released on Hugging Face, Apache-2.0 (model card metadata): https://huggingface.co/HazelNam/CJEPA
- Files include C-JEPA predictor checkpoints (`cjepa-ckpts/clevrer_savi_{0..4}_epoch_30_object.ckpt`, `clevrer_videosaur_{0..4}_...`, `pusht_videosaur_{0..2}_...`), object-centric encoder checkpoints (`clevrer_videosaur_model.ckpt`, `clevrer_savi_model.pth`, `pusht_videosaur_model.ckpt`) and pre-extracted slots (`*_slots.pkl`) (HF API, checked 2026-10-13).
- Usage: README "Model Checkpoints" table links each checkpoint to its config (e.g. `clevrer_dinov2_hf.yml`).

## 2. Code
- https://github.com/galilai-group/cjepa, no license detected by the GitHub API, last push 2026-07-21.
- Includes object-centric encoder training (VideoSAUR, SAVi via third-party code), C-JEPA training, ALOE VQA training/evaluation and Push-T planning; built on stable-pretraining and stable-worldmodel. README notes a pending refactor to the latest stable-worldmodel.

## 3. Datasets
- CLEVRER (public synthetic video QA benchmark of colliding objects): 128-frame inputs rolled out to 160 frames; 7 slots; questions are descriptive, explanatory, predictive and counterfactual.
- Push-T (simulated contact-rich pushing, as in DINO-WM): 4 slots; data prepared per `docs/DATASET.md` (expert action/proprio/state metadata on HF).
- PHYRE (public 2D physics puzzles): qualitative rollouts only (Appendix J).

## 4. Evaluation and downstream tasks
- Visual reasoning on CLEVRER via ALOE trained on imagined slot trajectories (Table 2, VideoSAUR encoder): masking 4 objects raises average per-question accuracy 82.79 → 89.40 and counterfactual per-question accuracy 47.68 → 68.81 (+21.13) vs the unmasked OC-JEPA. With SAVi, best at 2 masked objects (83.88 avg, 60.19 counterfactual); masking 4 hurts (73.28).
- Baselines with SAVi (Table 4): SlotFormer 79.44 avg / 47.29 CF (collapses to 44.94 / 11.10 without reconstruction); OCVP-Seq 83.11 / 56.06; C-JEPA 83.88 / 60.19 without any reconstruction.
- Push-T MPC planning success (Table 6): DINO-WM 91.33% (196×384 tokens), DINO-WM with registers 88.00%, OC-DINO-WM 60.67%, OC-JEPA 76.00%, C-JEPA 88.67% with 6×128 tokens (1.02% of the latent input size).
- Planning cost: 673 s vs 5,763 s for DINO-WM to evaluate 50 trajectories on one L40S GPU (over 8× faster).
- Treating actions/proprioception as separate tokens beats concatenation (Fig. 3). Object-level masking is more stable than token- or tube-level masking (Appendix K).

## 5. Architecture and techniques
- Encoder: frozen object-centric encoder (VideoSAUR over frozen DINOv2 features; SAVi for CLEVRER baselines), N slots of dimension d.
- Predictor: ViT-style bidirectional masked transformer, 6 layers, 16 heads, head dim 64, MLP 2048 (Appendix D). Actions and proprioception are embedded with 1-D temporal convolutions and added as auxiliary entity tokens.
- Masking: selected objects are masked across the whole history except the first frame; mask token = linear projection of the identity-anchor slot + learnable temporal embedding (Eq. 3). Future tokens are always masked.
- Objective: L2 on all masked tokens = masked-history completion + future prediction (Eqs. 5–6); no reconstruction, frozen targets.
- Planner: CEM (300 samples, 30 elites, 30 iterations) on terminal latent L2 to the goal slots, MPC replanning.
- Theory (Section 6): under stated assumptions, the optimal masked-completion predictor must use the "influence neighborhood" of each object (Theorem 1).

## 6. Conclusions and future work
- Authors: first integration of JEPA with object-centric world modeling; object-level masking gives large counterfactual-reasoning gains and efficient planning with about 1% of the tokens. Stated limitations: performance is capped by the object-centric encoder; influence neighborhoods are not validated on datasets with known temporal causal graphs. Future work: jointly refining object-centric encoders on strong pretrained backbones without collapse, and richer, more complex environments.
- Own observations: control is tested on Push-T only, where C-JEPA still trails DINO-WM slightly (88.67 vs 91.33). The best number of masked objects differs by encoder (4 for VideoSAUR, 2 for SAVi), so the masking ratio needs tuning. The "causal" framing is explicitly predictive, not identifiability of causal structure.

## 7. Sources and verification
- Paper: arXiv HTML https://arxiv.org/html/2602.11389 (v2; Sections 4–7, Tables 2, 4, 6; Appendix D, G).
- Venue: PMLR v306 index (lists title and the five authors with link nam26c); OpenReview search (venue "ICML 2026 regular").
- Code/weights: GitHub API and Hugging Face API.
- Citations: Semantic Scholar API (arXiv:2602.11389).
- Not verified: ICML presentation type (poster/oral); code license (none declared); total parameter count of the predictor (layer sizes only).
