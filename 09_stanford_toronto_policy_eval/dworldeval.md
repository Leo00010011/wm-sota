---
citation_count: 11
citation_source: "Semantic Scholar, 2026-10-15 (value from existing note; live re-check returned HTTP 429)"
venue: "ICML 2026"
date: 2026-04-24
research_group: "Stanford / Toronto (policy evaluation)"
---

# dWorldEval: Scalable Robotic Policy Evaluation via Discrete Diffusion World Model

ICML 2026 title: "Scaling Real-World Robot Policy Evaluation via Discrete Diffusion World Model"

- **Authors:** arXiv v1: Yaxuan Li, Zhongyi Zhou, Yefei Chen, Yaokai Xue, **Yichen Zhu** (Current Robotics; University of Toronto). ICML 2026 listing: Yaxuan Li, Junjie Wen, Zhongyi Zhou, Yefei Chen, Chaomin Shen, Yaxin Peng, **Yichen Zhu**
- **Group role:** Senior (last) author (Yichen Zhu, University of Toronto affiliation)
- **Date published:** 2026-07 (ICML 2026, poster session 2026-07-08); arXiv 2026-04-24
- **Venue:** ICML 2026. The ICML poster page lists a poster; the ICML virtual site also has a spotlight page and the project page says "Spotlight".
- **Links:** arXiv https://arxiv.org/abs/2604.22152 · ICML poster https://icml.cc/virtual/2026/poster/65898 · ICML spotlight https://icml.cc/virtual/2026/spotlight/84893 · OpenReview https://openreview.net/forum?id=93uNlQ1Qp0 · project page https://dworldeval.github.io/
- **Number of citations:** 11 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
dWorldEval is a masked discrete-diffusion world model in which images, language and action chunks are tokens in one sequence handled by a single bidirectional transformer. It adds a sparse keyframe memory for long-horizon consistency and a discrete "progress" token, so the model itself predicts task completion and success is read off when progress reaches 1. It is used as a proxy evaluator to rank robot policies on LIBERO, RoboTwin and a real bimanual robot.

## 1. Weights availability
Not released. The project page lists only the arXiv paper; no Hugging Face or GitHub weight links found (GitHub search for "dWorldEval" returned only the project-page repo dworldeval/dworldeval.github.io; checked 2026-10-15).

## 2. Code
No code found. Checked the project page, GitHub repository search ("dWorldEval") and the ICML page (no code link).

## 3. Datasets
- LIBERO (Object, Spatial, Goal, LIBERO-100): 5.5k official expert demos plus 1k failed rollouts from suboptimal policies. Public benchmark; the failure data is not reported as released.
- RoboTwin (ARX arm): 5.5k trajectories across 10 tasks.
- Real world: bimanual AgileX, three RealSense cameras, 5.2k trajectories (including 1k human-collected failures) across 5 tasks (bussing table, place cup, handover block, strike block, place bottles). Proprietary/not released.
- Progress labels: produced by prompting SEED-1.5VL with task milestones (Sec. 3.2.3, Appendix C).

## 4. Evaluation and downstream tasks
- Action controllability on LIBERO (Table 1): new metric Delta-LPIPS (LPIPS on frame differences). Ours 0.315 (expert) / 0.352 (failure) vs Ctrl-World 0.334/0.416, WorldGym 0.347/0.650, WorldEval 0.423/0.701. Plain LPIPS: ours 0.215 vs WorldGym 0.218, Ctrl-World 0.220, WorldEval 0.262.
- Long-horizon round-trip consistency (Tables 2 and 4), LPIPS at H = 20: ours 0.243 vs Ctrl-World 0.370, WorldGym 0.482, WorldEval 0.531; without memory 0.411.
- Policy evaluation (Fig. 7): Pearson r with real success: LIBERO multi-view 0.910, RoboTwin 0.927 (pi0, DexVLA, Diffusion Policy), real world 0.918. On LIBERO single-view, rank-violation MMRV 0.013 vs up to 0.039 for baselines.
- Automatic success detection: progress-token success tracks real and human-graded success across pi0 checkpoints (Fig. 6).
- Multi-view fidelity across platforms (Table 3): Delta-LPIPS about 0.30-0.37.
- Baselines retrained on the same splits: WorldEval, WorldGym, Ctrl-World.

## 5. Architecture and techniques
- Backbone: initialized from MMaDA-VLA-8B, bidirectional transformer, 32 layers, 32 heads, hidden 4096 (Appendix A.2), so about 8B parameters.
- Paradigm: masked discrete diffusion (MDD) with weighted cross-entropy on masked tokens (Eq. 1); 16-step iterative parallel decoding.
- Tokenizers: MAGVIT-v2 for images, LLaDA tokenizer for text, FAST for continuous action chunks.
- Memory: K = 4 low-resolution (128x128) top-down keyframes with absolute frame indices as text tokens; current observation at 256x256, all views.
- Progress token: text token in [0, 1], loss weight 2 vs 1 for visual tokens.
- Prediction horizon equals action chunk length (2-8).
- Training: 15 epochs, 8x H800, AdamW lr 5e-5, global batch 128. Evaluation of one trajectory takes 30-90 s on one H800.

## 6. Conclusions and future work
- Authors: action tokens as first-class sequence elements plus sparse memory give consistent long rollouts and much better controllability; predicted success closely tracks real execution and enables policy ranking across architectures. They present it as a new architectural paradigm for world simulators for evaluation at scale.
- Own observations: the world model is trained per platform on in-domain data that already include failure trajectories, which favors controllability on failure cases; it is unclear how it transfers to new scenes. The progress labels come from a closed VLM (SEED-1.5VL). The arXiv abstract repeats its first sentences (an editing slip). No code or weights, so results cannot be reproduced. Author lists differ between arXiv and ICML.

## 7. Sources and verification
- Paper: arXiv 2604.22152v1 PDF, main text and Appendix A.2 read.
- Venue: ICML 2026 poster page (author list and OpenReview link), ICML spotlight page (from search results), project page.
- Citations: Semantic Scholar API, 2026-10-15.
- Not verified: whether the ICML presentation is spotlight or poster (sources disagree); the ICML camera-ready version (not read; numbers above come from arXiv v1); the Toronto PI's exact role (Yichen Zhu is listed with a University of Toronto affiliation; his position there was not verified).
