---
citation_count: null
citation_source: null
venue: "ICLR 2026"
date: 2025-10-11
research_group: "Stanford / Toronto (policy evaluation)"
---

# Ctrl-World: A Controllable Generative World Model for Robot Manipulation

- **Authors:** Yanjiang Guo*, Lucy Xiaoyang Shi*, Jianyu Chen, **Chelsea Finn** (*equal contribution; Stanford University, Tsinghua University)
- **Group role:** Senior (last) author (Finn); first author is a Stanford/Tsinghua student
- **Date published:** 2026-04 (ICLR 2026 conference, venue date); first arXiv version 2025-10-11, v3 2026-03-01
- **Venue:** ICLR 2026, main conference poster
- **Links:** arXiv https://arxiv.org/abs/2510.10125 · ICLR poster https://iclr.cc/virtual/2026/poster/10011332 · mlanthology https://mlanthology.org/iclr/2026/guo2026iclr-ctrlworld/ · project page https://ctrl-world.github.io · code https://github.com/Robert-gyj/Ctrl-World · weights https://huggingface.co/yjguo/Ctrl-World
- **Number of citations:** 196 (source: Semantic Scholar API, checked 2026-10-15)

## Summary
Ctrl-World turns the pretrained Stable Video Diffusion model into an action-conditioned, multi-view world model for the DROID Franka platform. A generalist policy (pi0, pi0-FAST, pi0.5) interacts with it in closed loop for 20+ seconds. It is used to rank policies without real rollouts and to generate successful synthetic trajectories for fine-tuning pi0.5.

## 1. Weights availability
- Released: Hugging Face `yjguo/Ctrl-World` (about 8 GB, "Ctrl-World model trained on DROID dataset"), https://huggingface.co/yjguo/Ctrl-World
- Also needs `stabilityai/stable-video-diffusion-img2vid` and `openai/clip-vit-base-patch32`; pi0.5-DROID comes from the openpi repo.
- Loading: through the repo scripts with `--svd_model_path`, `--clip_model_path`, `--ckpt_path` (e.g. `python scripts/rollout_replay_traj.py`). No Python API is documented.
- License of the checkpoint: not stated on the model card as summarized; the code is MIT.

## 2. Code
- https://github.com/Robert-gyj/Ctrl-World, MIT license, 569 stars, last push 2026-04-08 (GitHub API, checked 2026-10-15).
- Contains training (`scripts/train_wm.py`, latent extraction, metadata creation) and inference / policy evaluation (trajectory replay, keyboard control, pi0.5 interaction and evaluation scripts), plus a small DROID subset.

## 3. Datasets
- Training: DROID, 95,599 trajectories from 564 scenes (about 76k successes and 19k failures), public (Hugging Face `cadene/droid_1.0.1`, about 370 GB per the README). 2% held out for validation.
- Evaluation: 256 validation clips of 10 s (Table 1); a new in-house DROID setup with novel camera placements for policy evaluation; 400 synthetic trajectories per task for policy improvement.

## 4. Evaluation and downstream tasks
- Video quality, 10-s autoregressive rollouts, third-view camera (Table 1): Ctrl-World PSNR 23.56, SSIM 0.828, LPIPS 0.091, FID 25.00, FVD 97.4, vs WPE (WorldGym) single-view FVD 156.4 and IRASim single-view FVD 138.1.
- Ablations (Table 2): removing memory raises third-view FVD to 105.5; removing frame-level conditioning to 122.7; wrist-view FVD without joint multi-view prediction 158.1 vs 127.1.
- Policy evaluation: pi0, pi0-FAST and pi0.5 on 7 tasks (pick-place, towel folding, drawer, wipe table, close laptop, pull tissue, stack). Linear fit between world-model and real rates: instruction following y = 0.87x - 0.04; success rate y = 0.81x - 0.11 (Fig. 7). The model tracks instruction following well but underestimates execution success.
- Policy improvement: fine-tuning pi0.5 on 25-50 human-selected successful imagined trajectories per task raises success on novel instructions/objects from 38.7% to 83.4% (+44.7%, Fig. 9; tasks: spatial, shape, towel direction, novel object).
- Baselines: WPE/WorldGym and IRASim (re-trained on DROID).

## 5. Architecture and techniques
- Backbone: Stable Video Diffusion 1.5B (spatial-temporal transformer / latent video diffusion), initialized from pretrained weights; only an action-projection MLP is new.
- Paradigm: latent diffusion, x0-prediction loss (Eq. 3).
- Multi-view joint prediction: three cameras (two third-person, one wrist), 192x320 each, tokens concatenated.
- Pose-conditioned memory retrieval: 7 sparse history frames (1-2 s stride) with robot poses injected by frame-level cross-attention.
- Action conditioning: 15-step (1 s) action chunks converted to Cartesian poses, applied by frame-wise cross-attention in the spatial transformer; CLIP semantic tokens for language.
- Success labels: human judgment (no VLM reward).
- Training: 2x8 H100, batch 64, 2-3 days.

## 6. Conclusions and future work
- Authors: policies evaluated in Ctrl-World mirror real instruction-following behavior; synthetic post-training gives large gains on novel instructions. Limits: failures on precise interactions and long-horizon reasoning; sensitivity to initial observations; not accurate enough to improve low-level success on seen instructions. Future: iterative policy rollout and model fine-tuning, VLM reward models, stronger video backbones.
- Own observations: success judgment and trajectory filtering are manual, so the pipeline is not fully automatic. Ranking evidence covers three closely related policies from one company (pi0 family). The world model systematically underestimates success, so absolute values need calibration.

## 7. Sources and verification
- Paper: arXiv 2510.10125v3 PDF ("Published as a conference paper at ICLR 2026"), main text read in full.
- Venue: ICLR 2026 poster page and mlanthology entry.
- Code/weights: GitHub API and README (via WebFetch), checked 2026-10-15.
- Citations: Semantic Scholar API, 2026-10-15.
- Ownership decision: Finn (Stanford) is last author; Jianyu Chen (Tsinghua IIIS) is not a PI of group 06 (Long / Zhu / Li), so the paper belongs to this folder.
