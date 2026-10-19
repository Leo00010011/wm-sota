---
citation_count: 65
citation_source: "Semantic Scholar, 2026-10-15 (value from existing note; live re-check rate-limited)"
venue: "ICLR 2026"
date: 2025-05-31
research_group: "Stanford / Toronto (policy evaluation)"
---

# WorldGym: World Model as An Environment for Policy Evaluation

- **Authors:** Julian Quevedo, Ansh Kumar Sharma, Yixiang Sun, Varad Suryavanshi, **Percy Liang**, **Sherry Yang** (Stanford, NYU, Google DeepMind)
- **Group role:** Senior authors (Liang second-to-last, Yang last and corresponding author)
- **Date published:** 2026-04 (ICLR 2026 conference, venue date); first arXiv version 2025-05-31
- **Venue:** ICLR 2026, main conference poster
- **Links:** arXiv https://arxiv.org/abs/2506.00613 · ICLR poster https://iclr.cc/virtual/2026/poster/10008029 · mlanthology https://mlanthology.org/iclr/2026/quevedo2026iclr-worldgym/ · project page https://world-model-eval.github.io · code https://github.com/world-model-eval/world-model-eval
- **Number of citations:** 65 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
WorldGym is an autoregressive, action-conditioned video world model (latent Diffusion Transformer trained with Diffusion Forcing) that stands in for the real world when evaluating robot policies. A policy is rolled out in closed loop from a single real initial frame, and GPT-4o judges success on the generated video. Success rates in the world model correlate with real-world success rates and keep policy rankings. The first arXiv version was titled "Evaluating Robot Policies in a World Model" (WPE); later papers (e.g. Ctrl-World) cite it as WPE.

## 1. Weights availability
- Released. The repo README points to a world-model checkpoint `mixed_openx_9robots_20frames_0p1actiondropout_580ksteps.pt` (about 9 GB) on Google Drive: https://drive.google.com/file/d/1uiRP2BuavapMsyP9Cbr25mi_ymk9SEJb/view
- Loading: pass the file to the CLI runners (`world-model-eval-openvla --checkpoint-path ...`, also `-octo`, `-spatialvla`, `-rt1`).
- License: no license file reported by the GitHub API (license field empty), so terms of use are not stated.
- No Hugging Face release found.

## 2. Code
- https://github.com/world-model-eval/world-model-eval (114 stars, last push 2025-11-06 per GitHub API, checked 2026-10-15).
- Contains the evaluation harness (policy runners for OpenVLA, Octo, SpatialVLA, RT-1-X), training script (`torchrun train.py`) and Open X-Embodiment download utilities. So both training and evaluation code are included.
- License: not stated in the repository metadata.

## 3. Datasets
- Training: 9 Open X-Embodiment robot datasets whose action spaces can be unified, including Bridge V2 and RT-1 (Appendix A). Public.
- Evaluation: initial frames from the OpenVLA Bridge evaluation trials (17 tasks x 10 trials, Kim et al.), RT-1 / Google Robot tasks (Appendix E.2), OOD images edited with Nano Banana, and OOD language variants (Table 1).
- VLM reward validation: RT-1 validation split with ground-truth success labels (Table 3).

## 4. Evaluation and downstream tasks
- World-model fidelity: qualitative agreement with Open-X validation videos (Fig. 2); end-effector control sweeps (Fig. 3). Dataset-size ablation (Table 8): Bridge V2 vs V1 training gives MSE 0.010 vs 0.015, LPIPS 0.073 vs 0.131, SSIM 0.827 vs 0.735.
- Policy evaluation (main task): Pearson r = 0.78 between per-task success in WorldGym and the real world for RT-1-X, Octo and OpenVLA (Fig. 4a). Mean success real vs world model: RT-1-X 18.5% vs 15.6%, Octo 20.0% vs 23.8%, OpenVLA 70.6% vs 67.4%; average gap 3.3% (Fig. 4b, Sec. 4.1).
- Policy ranking: keeps rankings across policy families, versions/sizes (Octo-Small vs Octo-Base 1.5; OpenVLA v0.1 vs OpenVLA 7B; Fig. 6), and training checkpoints of a UniPi-style video policy and a DexVLA-style diffusion policy (Fig. 7).
- OOD testing: unseen objects, distractors, color/shape classification, OOD instructions (Table 1, Figs. 8-13). Adding distractors drops success by 51% (RT-1-X), 83% (Octo), 41.5% (OpenVLA) (Fig. 13).
- VLM reward (GPT-4o) on RT-1 videos: TP 0.81, FP 0.03, TN 0.97 (Table 3).
- Efficiency: 40-frame rollout on one A100 takes 93 s with horizon 1 vs 33 s with horizon 4 (Table 9); all reported rollouts run in under an hour on one GPU.
- Baselines: real-world results from the OpenVLA paper; no other world model is compared directly.

## 5. Architecture and techniques
- Latent Diffusion Transformer (DiT), 16 layers, hidden size 1024, 16 heads, patch size 2, 609M parameters (Table 2).
- Tokenizer: Stable Diffusion 3 VAE, 256x256 frames, 16 latent channels.
- Generative paradigm: diffusion with Diffusion Forcing for autoregressive frame generation; v-prediction, sigmoid noise schedule, 10 sampling steps; causal temporal attention interleaved with spatial attention; 20-frame context with sliding window.
- Action conditioning: 7-D action (6-D end-effector + gripper) linearly projected and added to the timestep embedding, injected through AdaLN-Zero. Whole-clip action dropout plus classifier-free guidance for stronger action adherence. Action spaces normalized to RT-1 percentiles.
- Inference: diffusion horizon set equal to the policy's action-chunk size.
- Reward: GPT-4o with a task rubric, optionally partial credit.
- Training: 300k steps, batch 16, AdamW, lr 8e-5, 2x A100 80GB (Table 2).

## 6. Conclusions and future work
- Authors: WorldGym emulates realistic robot motion, its success rates correlate strongly with real outcomes, and it makes OOD language and image tests easy. Not all interactions are realistic; generating highly realistic object interaction remains hard. It is presented as a starting point for safe and reproducible pre-deployment evaluation.
- Own observations: the correlation (r = 0.78) rests on three policies on one robot (WidowX/Bridge) with 10 trials per task, so the statistical support for "ranking preservation" is thin. The reward model is a closed API model (GPT-4o), which limits reproducibility. The checkpoint is on Google Drive with no stated license.

## 7. Sources and verification
- Paper: arXiv 2506.00613v3 (PDF read in full, main text and Appendices A, B, E, F).
- Venue: ICLR 2026 virtual poster page and mlanthology ICLR 2026 entry. The arXiv v3 PDF header still says "Preprint. Under review"; acceptance is taken from the ICLR site.
- Code and weights: GitHub API and repository README (checked 2026-10-15).
- Citations: Semantic Scholar API batch endpoint, 2026-10-15.
- Exact ICLR camera-ready date: not verified; ICLR 2026 conference month used.
- The summary file's claim that the model "underestimates in-distribution and overestimates out-of-distribution policy values" is not in v3 of the paper; it probably comes from v1 (WPE). Not verified against v1.
