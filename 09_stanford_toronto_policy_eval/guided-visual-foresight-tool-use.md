---
citation_count: 98
citation_source: "Semantic Scholar, 2026-10-15 (carried over from existing note; live API check returned HTTP 429)"
venue: "RSS 2019"
date: 2019-04-11
research_group: "Stanford / Toronto (policy evaluation)"
---

# Improvisation through Physical Understanding: Using Novel Objects as Tools with Visual Foresight (GVF)

- **Authors:** Annie Xie, Frederik Ebert, Sergey Levine, **Chelsea Finn** (UC Berkeley)
- **Group role:** Senior (last) author (Finn)
- **Date published:** 2019-06 (RSS 2019, venue date); arXiv 2019-04-11
- **Venue:** RSS 2019
- **Links:** arXiv https://arxiv.org/abs/1904.05538 · project page https://sites.google.com/view/gvf-tool
- **Number of citations:** 98 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
Guided visual foresight (GVF) combines an action-conditioned video prediction model trained on random, demonstration and proposal-policy data with an LSTM mixture-density "action proposal" model learned from diverse tool-use demonstrations. The proposal model seeds the first CEM iteration of visual MPC, so the planner can find multi-stage tool-use plans (grasp a tool, then sweep or wipe), including with never-seen objects used as tools.

## 1. Weights availability
Not released. The project page has only the paper link and videos (checked 2026-10-15).

## 2. Code
No code found. Checked project page and GitHub search. The video model reuses the open-source SNA architecture from Ebert et al. (visual foresight).

## 3. Datasets
- Video-model training: 16,000 random trajectories from an open dataset (Ebert et al.), 5,052 random trajectories with tools, 1,754 action-proposal rollouts, 1,200 kinesthetic demonstrations (20 training tools). New data not reported as released.
- Evaluation: 10 tasks with seen tools and 10 with novel tools on a Sawyer.

## 4. Evaluation and downstream tasks
- Metric: final distance of objects to goal (cm), per-task tables in Appendix (Figs. 16-17).
- Seen tools, mean over 10 tasks (computed here from the per-task table): GVF 6.4 cm, GVF without demo data in the video model 12.8 cm, visual MPC 15.5 cm, imitation learning 17.0 cm.
- Novel tools, mean (computed here): GVF 5.6 cm, ablation 11.4 cm, visual MPC 13.0 cm, imitation 14.1 cm.
- Qualitative: uses a sponge, a knife, a water bottle as improvised tools; chooses not to use a tool when one object suffices (Fig. 13).
- Downstream task: real-robot tool-use manipulation by visual MPC.

## 5. Architecture and techniques
- World model: transformation-based recurrent convolutional video prediction (SNA, Finn et al. / Ebert et al.), predicts flow fields to warp previous frames, L2 loss; also propagates designated-pixel distributions.
- Action proposal: LSTM with mixture of 10 Gaussians, conditioned on initial image (CNN + spatial soft-argmax, VGG-initialized) and end-effector state.
- Planner: CEM (100 samples, horizon 21, 3 iterations), first iteration sampled from the proposal model; expected pixel-distance cost.
- Grasp reflex during data collection.

## 6. Conclusions and future work
- Authors: combining demonstrations with visual MPC outperforms either alone and enables improvised tool use. Limits: mainly sweeping/wiping/hooking; vision only (no force); future: cutting, screwing, tactile sensing, other long-horizon skills.
- Own observations: the means above are my own averages of the paper's per-task tables (the paper reports them only as a bar chart); each task is run a small number of times.

## 7. Sources and verification
- Paper: arXiv 1904.05538v1 PDF, main text and appendix tables read.
- Venue: Semantic Scholar venue ("Robotics: Science and Systems") and Finn CV ([37] RSS 2019).
- Citations: Semantic Scholar API, 2026-10-15.
