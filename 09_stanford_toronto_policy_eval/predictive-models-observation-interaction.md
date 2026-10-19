---
citation_count: null
citation_source: "unverified: 67 per earlier note (Semantic Scholar, 2026-10-15); Semantic Scholar API returned HTTP 429 on re-check"
venue: "ECCV 2020"
date: 2019-12-30
research_group: "Stanford / Toronto (policy evaluation)"
---

# Learning Predictive Models From Observation and Interaction

- **Authors:** Karl Schmeckpeper, Annie Xie, Oleh Rybkin, Stephen Tian, Kostas Daniilidis, Sergey Levine, **Chelsea Finn** (University of Pennsylvania, Stanford University, UC Berkeley)
- **Group role:** Senior (last) author (Finn); corresponding author is the first author (Schmeckpeper, UPenn), who is not a summary-group PI
- **Date published:** 2020-08 (ECCV 2020, venue date); arXiv 2019-12-30
- **Venue:** ECCV 2020
- **Links:** arXiv https://arxiv.org/abs/1912.12773 · ECVA https://www.ecva.net/papers/eccv_2020/papers_ECCV/html/3606_ECCV_2020_paper.php · project page https://sites.google.com/view/lpmfoai
- **Number of citations:** 67 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
The paper (method "POI") trains an action-conditioned video prediction model from both action-labelled robot interaction data and action-free observation data (human videos, dashcam video). Actions are modeled as latent variables inferred either from real actions or from consecutive frames, with a domain-specific prior to absorb the human-robot embodiment gap. The model is then used for visual MPC tool-use tasks on a real Sawyer.

## 1. Weights availability
Not released. Project page says the human tool-use dataset link is "coming soon"; no checkpoints (checked 2026-10-15).

## 2. Code
No code found. Checked project page, ECVA page and web/GitHub search.

## 3. Datasets
- Driving: nuScenes (Boston with actions; Singapore with actions removed) and BDD100K (action-free), public.
- Manipulation: 20,000 random Sawyer trajectories from prior open datasets; 1,000 new human tool-use videos (stated to be released; link not found); test set of 1,200 kinesthetic demonstrations.

## 4. Evaluation and downstream tasks
- Driving prediction on nuScenes Singapore (Table 1): POI PSNR 20.81 / SSIM 0.5486 / LPIPS 0.1933 vs SAVP (Boston only) 19.74 / 0.5121 / 0.1951; oracle 21.17.
- Manipulation prediction (Table 2): POI PSNR 23.79, SSIM 0.813, LPIPS 0.0722 vs SAVP 23.31 / 0.803 / 0.0757 and CLASP 22.14; oracle 24.99.
- Real-robot tool-use planning (Table 3, 30 tasks): POI 40.0% success vs SAVP 23.3% and oracle trained on kinesthetic demos 36.7%.
- Downstream tasks: action-conditioned video prediction (driving, manipulation), visual MPC planning.

## 5. Architecture and techniques
- Transition model: modified SAVP (transformation-based, convolutional recurrent) predicting x_{t+1} from frames and latent action z_t.
- Latent action: action encoder q_act (MLP) and inverse model q_inv (CNN) with a Jensen-Shannon alignment loss; action decoder p(a|z).
- Domain shift: z split into shared and domain-specific parts with separate learned priors (LSTM prior for human data), inspired by DIVA.
- Training: amortized variational inference (ELBO for action-labelled and action-free sequences, Eq. 4-8).
- Planner: sampling-based visual MPC with designated-pixel costs.

## 6. Conclusions and future work
- Authors: passive observation improves action-conditioned prediction and control despite domain shift. Limits: only mild domain shifts tested; future work on larger shifts and on representation learning and RL with observational data.
- Own observations: prediction gains over SAVP are small in absolute terms; real-robot success uses 30 tasks with wide standard errors (about 9%).

## 7. Sources and verification
- Paper: arXiv 1912.12773v1 PDF, main text read.
- Venue: ECVA ECCV 2020 page and mlanthology entry (search results).
- Citations: Semantic Scholar API, 2026-10-15.
- Ownership note: last author Finn is a group PI; corresponding author (Schmeckpeper) is not a PI of another summary group, so the paper stays here.
