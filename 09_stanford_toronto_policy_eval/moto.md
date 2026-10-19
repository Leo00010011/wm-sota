---
citation_count: null
citation_source: null
venue: "CoRL 2023"
date: 2024-01-06
research_group: "Stanford / Toronto (policy evaluation)"
---

# MOTO: Offline Pre-training to Online Fine-tuning for Model-based Robot Learning

- **Authors:** Rafael Rafailov*, Kyle Hatch*, Victor Kolev, John D. Martin, Mariano Phielipp, **Chelsea Finn** (*equal contribution; Stanford University, Intel AI Labs)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2023-11 (CoRL 2023, Atlanta, venue date); arXiv 2024-01-06
- **Venue:** CoRL 2023 (PMLR vol. 229)
- **Links:** arXiv https://arxiv.org/abs/2401.03306 · PMLR https://proceedings.mlr.press/v229/rafailov23a.html · project page https://sites.google.com/view/mo2o/
- **Number of citations:** 20 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
MOTO learns a DreamerV2-style recurrent variational world model (RSSM with discrete latents) from offline image data and trains an actor-critic inside it with model-based value expansion, an ensemble-disagreement reward penalty and behavior-prior regularization. The same world model and agent are then fine-tuned online. It is reported as the first method to solve Franka Kitchen from pixels.

## 1. Weights availability
Not released. The project page has no code, weight or dataset links (checked 2026-10-15); no GitHub repository found by search.

## 2. Code
No code found. Checked the project page, arXiv/PMLR pages and GitHub search. The paper promises release of environments and datasets, but no link was found.

## 3. Datasets
- MetaWorld: 10 tasks, 64x64 RGB, no proprioception, sparse rewards; 9-10 scripted demonstrations per task (collected by the authors).
- Franka Kitchen (D5RL standard version): "mixed" and "partial" tasks, central camera only.
- Release status: not found.

## 4. Evaluation and downstream tasks
- Downstream task: offline-to-online model-based RL from images (robot manipulation).
- Protocol: 10k offline gradient steps, then 500k online environment steps.
- MetaWorld (Fig. 2): MOTO matches or beats baselines on 9/10 tasks; DreamerV2 is the only competitive baseline; COMBO and LOMPO score low.
- Franka Kitchen (Fig. 3): final success 100% (mixed) and 90.5% (partial) vs DreamerV2 77.5% and 13.5%.
- Ablations: removing the uncertainty penalty or BC regularization lowers performance; gains are additive.
- Baselines: LOMPO, COMBO, DreamerV2, CQL, IQL, SAC, BC.

## 5. Architecture and techniques
- World model: RSSM recurrent VAE (deterministic + discrete stochastic latent, DreamerV2 architecture) with image and reward decoders, ELBO training (Eq. 1); ensemble of latent transition models.
- Conservatism: reward minus alpha times std of ensemble logits (Eq. 2-3).
- Policy learning: model-based value expansion with GAE(lambda) mixing over horizons (Eq. 5-9), critics trained on both model and real data; behavior-prior regularization on positive-advantage snippets (Eq. 10).
- Parameter count: not stated.

## 6. Conclusions and future work
- Authors: MOTO outperforms baselines on 9/10 MetaWorld tasks, solves Franka Kitchen from images, and empirically verifies prior offline MBRL bounds. Limits: pessimism can hurt with poor data; success-based regularization is hard to extend to dense rewards; unclear if ensemble uncertainty transfers to transformer world models. Future: CARLA, model-based imitation, multi-task and transfer.
- Own observations: all results are in simulation; data per task is tiny (about 10 demos), so the test of "large-scale models" the authors motivate is not done.

## 7. Sources and verification
- Paper: arXiv 2401.03306v1 PDF (CoRL 2023 manuscript), main text read.
- Venue: PMLR v229 and mlanthology CoRL 2023 entry (search results); PDF footer "7th Conference on Robot Learning (CoRL 2023)".
- Citations: Semantic Scholar API, 2026-10-15.
- Exact per-task MetaWorld numbers: only in Fig. 2, not given in text.
