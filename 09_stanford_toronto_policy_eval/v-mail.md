---
citation_count: 61
citation_source: "Semantic Scholar, 2026-10-15"
venue: "NeurIPS 2021"
date: 2021-07-16
research_group: "Stanford / Toronto (policy evaluation)"
---

# Visual Adversarial Imitation Learning using Variational Models (V-MAIL)

- **Authors:** Rafael Rafailov, Tianhe Yu, Aravind Rajeswaran, **Chelsea Finn** (Stanford University, University of Washington, Facebook AI Research)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2021-12 (NeurIPS 2021, venue date); arXiv 2021-07-16
- **Venue:** NeurIPS 2021 (main track)
- **Links:** arXiv https://arxiv.org/abs/2107.08829 · NeurIPS https://papers.neurips.cc/paper_files/paper/2021/hash/1796a48fa1968edd5c5d10d42c7b1813-Abstract.html · project page https://sites.google.com/view/variational-mail · code https://github.com/rmrafailov/VMAIL
- **Number of citations:** 61 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
V-MAIL learns a variational latent dynamics model (world model) from images and runs on-policy adversarial imitation learning entirely inside it: a discriminator separates expert and model-rollout latent states and serves as reward, and the policy is trained by back-propagating through model rollouts. The shared model also enables zero-shot transfer to new imitation tasks without extra environment samples.

## 1. Weights availability
Not released. The repository README links expert demonstration data but no trained checkpoints.

## 2. Code
https://github.com/rmrafailov/VMAIL, 25 stars, last push 2021-12-10, no license reported by GitHub API (checked 2026-10-15). Single-commit research snapshot; contains training script `vmail.py`, conda environment, bundled robel package. Training code yes; separate evaluation tooling not documented.

## 3. Datasets
- Environments: DMC Cheetah Run, Walker Walk; OpenAI Gym Car Racing; D'Claw (ROBEL) screw; Shadow Hand Baoding Balls (vision-based).
- Expert demonstrations: 10 per task (Car Racing: 10); public via Google Drive link in the README: https://drive.google.com/drive/folders/1JZmOVmlCqScqu0DDmn7857D5FtHZr6Un

## 4. Evaluation and downstream tasks
- Single-task visual imitation (Fig. 3): V-MAIL reaches near-expert return on Cheetah, Walker, Car Racing and D'Claw; it is the only method to make progress on Baoding Balls; baselines DA-DAC and DA-SQIL are unstable.
- Ablations (Fig. 4): works with 5 demos (minor drop) and 1 demo (drop on Walker); representation-only variant (VDAC) is about 30% less sample efficient.
- Zero-shot transfer (Table 1, % of expert return): Walker Walk 92.7% and Claw Rotate 97.9% vs offline DAC 8.8% / -0.7%, BC 26.8% / 8.3%, policy transfer 21.3% / 5.6%; oracle trained on target 98.2% / 102.3%.
- Downstream task: imitation learning from visual demonstrations.

## 5. Architecture and techniques
- World model: variational recurrent latent model (belief encoder, latent forward model, image decoder) trained with an ELBO (Eq. 7), Dreamer/SLAC-style.
- Discriminator on latent state-actions (GAN objective, Eq. 6); policy optimized by back-propagating K-step model returns plus value (Eq. 8).
- Theory: latent-space divergence upper-bounds state/observation divergence (Theorem 1); sub-optimality bound with model error (Lemma 1).
- Compute: about 1000 GPU hours on single Titan RTX GPUs.

## 6. Conclusions and future work
- Authors: V-MAIL gives better asymptotic performance and stability with similar sample efficiency, and zero-shot transfer. Limits: compact single-level latent may not handle cluttered or deformable scenes; possible vulnerability to visual perturbations. Future: imitation from observations only, other on-policy algorithms, offline imitation.
- Own observations: all experiments are simulated; transfer experiments assume shared dynamics across tasks.

## 7. Sources and verification
- Paper: arXiv 2107.08829v2 PDF (footer "NeurIPS 2021"), main text read.
- Code: GitHub API; README content via web search summary and raw README.
- Citations: Semantic Scholar API, 2026-10-15.
- Exact learning-curve values: only in figures.
