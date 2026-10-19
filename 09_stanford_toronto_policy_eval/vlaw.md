---
citation_count: 46
citation_source: "Semantic Scholar, 2026-10-15"
venue: "ICML 2026"
date: 2026-02-12
research_group: "Stanford / Toronto (policy evaluation)"
---

# VLAW: Iterative Co-Improvement of Vision-Language-Action Policy and World Model

- **Authors:** Yanjiang Guo*, Tony Lee*, Lucy Xiaoyang Shi*, Jianyu Chen, **Percy Liang**, **Chelsea Finn** (*core contributors; Stanford University, Tsinghua University)
- **Group role:** Senior authors (Finn last, Liang second-to-last); corresponding author Yanjiang Guo (Stanford)
- **Date published:** 2026-07 (ICML 2026, venue date); arXiv 2026-02-12
- **Venue:** ICML 2026, poster; PMLR vol. 306, pp. 37998-38011
- **Links:** arXiv https://arxiv.org/abs/2602.12063 · ICML poster https://icml.cc/virtual/2026/poster/66169 · PMLR https://proceedings.mlr.press/v306/guo26i.html · project page https://sites.google.com/view/vlaw-arxiv
- **Number of citations:** 46 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
VLAW is a follow-up to Ctrl-World. It fine-tunes the Ctrl-World action-conditioned video world model on real policy rollouts (including failures) so that it predicts contact-rich outcomes faithfully, then rolls the pi0.5 policy out inside the model, filters successful imagined trajectories with a fine-tuned VLM reward model, and fine-tunes the policy on real plus synthetic successes. World model and policy are improved in alternating rounds.

## 1. Weights availability
Not released. The project page links only to the Ctrl-World repository (labeled "Code (Post-train WM)"); no VLAW checkpoints, fine-tuned reward model or policy weights are linked. The base Ctrl-World weights are public (https://huggingface.co/yjguo/Ctrl-World).

## 2. Code
No VLAW-specific repository found. The project page points to https://github.com/Robert-gyj/Ctrl-World (MIT) for world-model post-training. Policy training uses openpi (pi0.5) per the paper; no VLAW pipeline code was found (checked project page and GitHub).

## 3. Datasets
- World-model pretraining: DROID (public; via Ctrl-World).
- New data (not reported as released): 25 expert demonstrations per task category; 50 real policy rollouts per task category per iteration; 500 synthetic trajectories per task per iteration.
- Tasks (real DROID Franka setup): stack blocks, open book, erase marks, scooping, drawing (Sec. 5.1).

## 4. Evaluation and downstream tasks
- World-model fidelity (Table 1, wrist view, 256 replayed 5-s clips): pretrained Ctrl-World PSNR 16.32 / FVD 225.13; + expert fine-tuning PSNR 19.87 / FVD 99.98; + online rollouts PSNR 21.77 / FVD 64.12.
- Interaction-outcome confusion matrix on 50 contact clips (Table 1): expert-only model TP 28, FN 2, TN 9, FP 11; with online rollouts TP 26, FN 4, TN 19, FP 1 (the over-optimism of demo-trained models largely disappears).
- Policy improvement (Fig. 7): two rounds of VLAW beat filtered BC and DSRL on all five tasks; abstract reports +39.2% absolute success over the base policy and +11.6% over training without synthetic rollouts.
- Ablations (Fig. 9, drawing task): fewer synthetic trajectories or removing real rollouts lowers success.
- Baselines: filtered BC on real rollouts, DSRL (noise-space RL).

## 5. Architecture and techniques
- World model: Ctrl-World (Stable Video Diffusion 1.5B backbone, multi-view, frame-level action conditioning, pose-conditioned memory), fine-tuned with the diffusion x0 loss on real rollouts, co-trained with DROID as regularizer (Eq. 1-2), 50K steps per iteration.
- Reward model: Qwen3-VL-4B-Instruct fine-tuned on real success labels; success if P("yes") > threshold (Eq. 3).
- Policy: pi0.5 (flow-matching VLA) updated with success-weighted flow-matching loss (Eq. 4), interpreted as advantage-weighted regression under KL-regularized RL (Sec. 4.3); 2k steps, batch 256 per round.

## 6. Conclusions and future work
- Authors: online rollout data grounds the world model in the target tasks; the grounded model produces useful synthetic data, and policy and world model can be co-improved. Limits: only five task categories; future work is scaling online data to broader tasks, and better base video models.
- Own observations: the world model is specialized per task set through real rollouts, so the "real data savings" claim depends on still collecting 50 real rollouts per task per iteration. Evaluation is on one platform (DROID) with one policy family (pi0.5). The paper's numbers are mostly in figures, so exact per-task values are not given in the text.

## 7. Sources and verification
- Paper: arXiv 2602.12063v2 PDF, main text read.
- Venue: ICML 2026 poster page and PMLR v306 entry (from search results); an ICLR 2026 virtual-site listing with the same title also exists, probably a workshop entry (not verified).
- Lab listing: Stanford IRIS publications page lists it as ICML 2026.
- Citations: Semantic Scholar API, 2026-10-15.
- Ownership: Finn (last) and Liang are group PIs; Jianyu Chen (Tsinghua) is not a group 06 PI.
