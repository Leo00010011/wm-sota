---
citation_count: 554
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live re-check returned HTTP 429)"
venue: "NeurIPS 2021"
date: 2021-02-16
research_group: "Stanford / Toronto (policy evaluation)"
---

# COMBO: Conservative Offline Model-Based Policy Optimization

- **Authors:** Tianhe Yu*, Aviral Kumar*, Rafael Rafailov, Aravind Rajeswaran, Sergey Levine, **Chelsea Finn** (*equal contribution; Stanford, UC Berkeley, Facebook AI Research)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2021-12 (NeurIPS 2021, venue date); arXiv 2021-02-16
- **Venue:** NeurIPS 2021 (main track)
- **Links:** arXiv https://arxiv.org/abs/2102.08363 · NeurIPS proceedings https://papers.neurips.cc/paper_files/paper/2021/file/f29a179746902e331572c483c45e5086-Paper.pdf
- **Number of citations:** 554 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
COMBO is an offline model-based RL algorithm: it learns a dynamics model from logged data, generates short model rollouts, and trains a CQL-style conservative critic that pushes down Q-values on model-generated (out-of-support) state-actions instead of relying on explicit uncertainty estimates. For image tasks it runs in the latent space of a recurrent variational world model (as in LOMPO).

Scope note: most experiments use low-dimensional state dynamics models; the image-based experiments use a latent world model. Included as world-model-based offline RL.

## 1. Weights availability
Not released. No checkpoints found (checked arXiv, NeurIPS page, GitHub search).

## 2. Code
No official code found. GitHub search for the title and a web search found no repository by the authors; arXiv lists only third-party code finders.

## 3. Datasets
- D4RL Gym (halfcheetah, hopper, walker2d; random, medium, medium-replay, medium-expert), public.
- Generalization tasks built by the authors: halfcheetah-jump, ant-angle (relabelled SAC replay buffers), sawyer-door-close.
- Image tasks: DMC walker-walk (4 datasets of 200 trajectories) and visual sawyer-door (medium-expert and expert).

## 4. Evaluation and downstream tasks
- Generalization (Table 1): halfcheetah-jump return 5308.7 vs MOPO 4016.6, MOReL 3228.7, CQL 741.1; ant-angle 2776.9 vs 2530.9 / 2660.3 / 2473.4; sawyer-door-close success 98.3% vs 65.8% / 42.9% / 36.7%.
- Image-based (Table 2): walker-walk normalized scores in line with LOMPO (e.g. M-R 69.2 vs 66.9); sawyer-door success 100% (M-E) and 96.7% (expert) where all other methods except LOMPO on M-E get 0%.
- D4RL (Table 3): best on 9/12 settings, e.g. hopper-medium 97.2, walker2d-medium 81.9, walker2d-med-expert 103.3.
- Analysis: ensemble max-variance uncertainty poorly predicts true model error (Fig. 2).
- Downstream task: offline RL (locomotion, manipulation, from states and images).

## 5. Architecture and techniques
- Dynamics model: ensemble of probabilistic neural networks (state-based, MOPO-style); for images, recurrent variational latent model following LOMPO.
- Policy learning: SAC-style actor-critic on a mix (fraction f) of real and model data; conservative Q penalty on model rollout state-actions, push-up on dataset state-actions.
- Theory: policy improvement guarantee and less conservative Q-values than CQL.

## 6. Conclusions and future work
- Authors: COMBO removes the need for uncertainty quantification, gets the best generalization on three tasks, scales to vision tasks and does well on D4RL. Open issues: offline hyperparameter selection and automatic choice of f.
- Own observations: the "world model" here is mostly a state-space dynamics model; the image-domain contribution reuses LOMPO's latent model. Results are simulation-only.

## 7. Sources and verification
- Paper: arXiv 2102.08363v2 PDF (comment "NeurIPS 2021"), main text read.
- Venue: arXiv comment, NeurIPS proceedings PDF, Semantic Scholar venue.
- Citations: Semantic Scholar API, 2026-10-15.
