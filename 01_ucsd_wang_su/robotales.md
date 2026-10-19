---
citation_count: 0
citation_source: "Semantic Scholar, 2026-10-13 (carried over; live re-check rate-limited on 2026-10-15)"
venue: "ECCV 2026"
date: 2026-07-07
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# RoboTALES: Learning Reasoning-Guided Robot Policies via Task-Aligned Simulated Futures

- **Authors:** Hanan Gani, Tejal Kulkarni, Madhoolika Chodavarapu, Nicklas Hansen, Manmohan Chandraker
- **Group role:** co-author (Nicklas Hansen, UCSD, Wang/Su group). Neither Wang nor Su is an author. The senior author is Manmohan Chandraker (UCSD), who is not a PI of any group in the summary, so the paper is kept here as a group-member paper.
- **Date published:** 2026-09 (ECCV 2026, Malmö, 8–12 Sep 2026). arXiv v1 2026-07-07.
- **Venue:** ECCV 2026, poster ("Accept Poster" in the ECCV 2026 virtual-site data, poster ID 5587)
- **Links:** arXiv https://arxiv.org/abs/2607.06018 · ECCV https://eccv.ecva.net/virtual/2026/poster/5587 · Code https://github.com/hananshafi/RoboTALES · Weights https://huggingface.co/hanangani/robotales-ckpts
- **Number of citations:** 0 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
RoboTALES uses a pretrained video diffusion model (Stable Video Diffusion) as a goal-conditioned "simulated futures" world model and jointly trains a diffusion action policy on its decoder features. Three ingredients shape the video model:
- an LLM planner (Gemini-2.5-Pro) splits the instruction into subgoals that condition the video model;
- a frozen VLM critic scores imagined rollouts, and that score is fed back through DDPO (RL fine-tuning of the diffusion model);
- action-loss gradients flow back into the video decoder ("imagine for acting").

It improves long-horizon manipulation on RoboCasa and LIBERO-10.

## 1. Weights availability
- Released: trained RoboTALES checkpoints for RoboCasa and LIBERO-10 at https://huggingface.co/hanangani/robotales-ckpts.
- Download: `huggingface-cli download hanangani/robotales-ckpts --local-dir checkpoints`, then set `model.params.ckpt_path=checkpoints/robocasa_ckpt/robotales-trained-robocasa.ckpt`.
- Also needs the OpenCLIP ViT-H-14 weights (laion/CLIP-ViT-H-14-laion2B-s32B-b79K).
- License: GitHub API reports "NOASSERTION"; the Hugging Face license was not checked.

## 2. Code
- https://github.com/hananshafi/RoboTALES. Contains training and closed-loop RoboCasa evaluation (`robocasa_experiment.py`). Built on the Video Policy code.
- License not asserted (GitHub API "NOASSERTION"). Last push 2026-07-13.

## 3. Datasets
- **RoboCasa:** 24 tasks, 50 human demonstrations per task, 256×256 multi-view frames; demo zip from videopolicy.cs.columbia.edu.
- **LIBERO-10:** 10 tasks, 50 demonstrations per task, resized to 256×256.
- Action space: 7-D (6-DoF gripper pose plus open/close).
- All public simulation benchmarks.

## 4. Evaluation and downstream tasks
Metric: success rate (RoboCasa: 50 rollouts over 5 scenes per task).
- **RoboCasa (Table 1):** RoboTALES has the highest average success (0.64 in the extracted table; baselines range from 0.06 up to about 0.575).
  - Baselines: 3D Diffuser Actor, DP3, Diffusion Policy variants, GR00T, UVA, Video Policy.
  - Text-reported results: pick-and-place mean 48%, turning 64%, pressing 96%.
  - Largest gains on doors and drawers.
  - It beats GR00T and another baseline that use 300 demos, while using only 50.
- **LIBERO-10 (Table 2):** 0.97 average success, vs Video Policy 0.94, UVA 0.90, π0 0.85, π0-FAST 0.60, DP-T 0.58, OpenVLA 0.54, DP-C 0.53.
- **Ablations (Figs. 4–6):**
  - The planner helps only when the video model is trained with it; the critic adds further gains.
  - More robust to observation noise and smoother latents (lower jerk).
  - Better with fewer demos.
  - Inference overhead is reported in Fig. 6b.
  - Joint vs decoupled training is ablated in the appendix.
- **Video Policy baseline caveat:** its numbers were reproduced with the released code; per correspondence with the Video Policy authors there is a discrepancy from their published results.

## 5. Architecture and techniques
- **Video world model:** Stable Video Diffusion UNet, initialized from Video Policy stage-2 weights. Only the text cross-attention and selected decoder layers are trained. Conditioned on CLIP embeddings of the augmented plan C* = [τ; c¹…c^K] with K ∈ [2, 5].
- **Policy:** 1D diffusion UNet conditioned on video-decoder hidden states.
- **Loss:** L_video (noise prediction) + β·L_DDPO (REINFORCE over denoising steps with VLM-critic reward minus a per-instruction baseline) + γ·L_action. No stop-gradient between policy and video model.
- **Note:** the "world model" is not conditioned on robot actions. It predicts plan-conditioned future frames, and actions are decoded from its features.
- **Compute:** 2×A100 80 GB for 6–7 days.

## 6. Conclusions and future work
- **Authors:** aligning imagination with hierarchical task logic gives more reliable rollouts and more coherent actions, especially on long-horizon tasks. Code and models are released.
- **Own observations:**
  - Not an action-conditioned world model; it is closer to video-prediction-as-policy-representation, which makes it borderline for the world-model scope.
  - Depends on a proprietary LLM planner (Gemini-2.5-Pro).
  - Simulation only.
  - The baseline reproduction discrepancy makes the comparison with Video Policy harder to interpret.
  - Hansen is a middle author, so the group's contribution is limited.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2607.06018 v1 (Sections 1–4, Tables 1–2, Figs. 1–6); GitHub README and API.
- ECCV 2026 acceptance confirmed in the ECCV 2026 virtual-site paper JSON ("Accept Poster").
- Citation count: Semantic Scholar batch API, 2026-10-13. Zero citations is plausible for a paper published one month ago.
- Unverified:
  - Exact per-baseline column mapping of the Table 1 average row (garbled in text extraction).
  - Licenses.
  - Appendix ablation numbers.
