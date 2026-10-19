---
citation_count: 177
citation_source: "Semantic Scholar, 2026-10-13 (re-check on 2026-10-15 rate-limited)"
venue: "ICML 2025"
date: 2025-02-10
research_group: "MIT (Vincent Sitzmann, Russ Tedrake)"
---

# History-Guided Video Diffusion

- **Authors:** Kiwhan Song, Boyuan Chen, Max Simchowitz, Yilun Du, **Russ Tedrake**, **Vincent Sitzmann**
- **Group role:** Senior author (Sitzmann last author; Tedrake co-author). Co-first authors Kiwhan Song and Boyuan Chen are MIT students in the group.
- **Date published:** 2025-07 (ICML 2025, Vancouver, 13 to 19 July 2025; PMLR vol. 267); arXiv v1 2025-02-10
- **Venue:** ICML 2025 (Proceedings of the 42nd International Conference on Machine Learning, PMLR 267:56242–56280), poster
- **Links:** arXiv https://arxiv.org/abs/2502.06764 · PMLR https://proceedings.mlr.press/v267/song25b.html · ICML poster https://icml.cc/virtual/2025/poster/44316 · Project https://boyuan.space/history-guidance · Code https://github.com/kwsong0113/diffusion-forcing-transformer · Weights https://huggingface.co/kiwhansong/DFoT
- **Number of citations:** 177 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
The paper introduces the Diffusion Forcing Transformer (DFoT). It is a non-causal video diffusion transformer trained with per-frame independent noise levels, so at sampling time it can condition on any subset or length of history frames, which are simply kept clean or partially noised. On top of this, "History Guidance" composes scores conditioned on different histories (vanilla, temporal, fractional/frequency). This improves quality, consistency and motion, makes the model robust to out-of-distribution history, and enables stable autoregressive rollouts of more than 800 frames. The model is used as an action- or camera-conditioned video world model (Minecraft actions, RealEstate10K camera poses) and as a robot imitation policy.

## 1. Weights availability
- Released on Hugging Face: https://huggingface.co/kiwhansong/DFoT (MIT license on the model card; about 167 GB total).
- `pretrained_models/`: `DFoT_RE10K.ckpt`, `DFoT_K600.ckpt`, `DFoT_K600_finetuned.ckpt`, `DFoT_MCRAFT.ckpt`, `ImageVAE_MCRAFT.ckpt`, `VideoVAE_K600.ckpt`, plus baselines `SD_K600`, `BD_K600`, `BD_RE10K`, `FS_K600`, `FS_RE10K`.
- `full_models/` (larger, likely full training states): `DFoT_K600.ckpt`, `DFoT_MCRAFT.ckpt`, `DFoT_RE10K.ckpt`.
- Loading: the repo README uses `load=pretrained:DFoT_RE10K.ckpt`, which downloads automatically. Interactive demo: https://huggingface.co/spaces/kiwhansong/diffusion-forcing-transformer.
- No robot (fruit-swapping) checkpoint found in the HF file listing.

## 2. Code
- https://github.com/kwsong0113/diffusion-forcing-transformer, MIT license (LICENSE file). It is built on Boyuan Chen's research template.
- It contains training (RE10K, Kinetics-600, Minecraft; 12×80 GB GPUs), validation/evaluation, and inference (image to long video, interpolation), and a wiki for reproducing paper results.
- The README does not mention robot imitation-learning code.
- Last-update date not checked (GitHub API rate-limited on 2026-10-13).

## 3. Datasets
- **Kinetics-600** (about 400K action videos, 600 classes; 128×128): main benchmark. Public.
- **RealEstate10K** (home walkthrough videos with camera poses; 256×256): camera-conditioned generation, out-of-distribution history, long rollouts. Public.
- **Minecraft** (from TECO; 200K videos × 300 frames, 3 discrete actions forward/left/right; upsampled to 256×256): action-conditioned long-context world modelling. Public.
- **Fruit Swapping** (robot imitation): 300 full-task demonstrations from a model-based planner (about 540 frames each) plus 300 short re-grasp demonstrations (about 50 frames) (App. C.1). Author-collected; no public release found.

## 4. Evaluation and downstream tasks
- **Metrics:** FVD, VBench (frame-wise quality, consistency, dynamic degree), LPIPS (deterministic tasks), robot success rate.
- **Kinetics-600 video prediction (Table 1, FVD, 5 history frames → 11):**
  - DFoT from scratch 4.3±0.1; DFoT fine-tuned from a full-sequence model 4.7±0.0 (with 12.5% of the training cost).
  - Same-architecture baselines: SD 4.8, BD 6.4, FS 95.5.
  - Industry models: MAGVIT-v2 4.3, W.A.L.T 3.3, Rolling Diffusion 5.2, Video Diffusion 16.2, MAGVIT 9.9.
  - DFoT is 673M parameters, vs about 1.1 to 1.2B for Video Diffusion and Rolling Diffusion (App. C.4).
- **64-frame sliding-window rollout on Kinetics-600 (Fig. 5):** FVD 208.0 (DFoT without guidance) → 181.6 (vanilla history guidance, ω=1.5) → 170.4 (fractional history guidance). Baselines: SD 247.5, FS 1040.
- **Out-of-distribution history, RE10K (Fig. 7):** with large camera rotations between history frames, temporal history guidance keeps LPIPS low while the SD, FS and vanilla baselines degrade sharply.
- **Long-context, action-conditioned Minecraft (Sec. 6.4, App. D.3):** FVD 97.63 (full context) → 79.19 (temporal history guidance).
- **Robot imitation (Sec. 6.4 Task 3):** fruit swapping with adversarial perturbations: 83% success. Markov, 2-frame and full-history diffusion baselines "fail to perform the task completely".
- **Ultra-long generation:** an 862-frame navigation video from a single RE10K test image (Fig. 1, Sec. 6.5).
- The summary file's "preliminary zero-shot robot motion planning" is not found in the camera-ready arXiv v2. The robot result here is imitation learning.

## 5. Architecture and techniques
- **Backbones (App. C.2, Table 2):**
  - Kinetics-600: latent DiT, 28 layers, hidden size 1152, with a chunk-wise VideoVAE ({1,4}×128×128 → 16×16 latents, 16 channels).
  - Minecraft: latent DiT, 12 layers, hidden size 768, with an ImageVAE (256→32×32, 4 channels).
  - RE10K: pixel-space U-ViT ([3,3,6,20] layers).
  - Robot: attention U-Net (x0 parameterization).
- **Diffusion:** v-parameterization (except the robot model), cosine or shifted-cosine schedule, DDIM with 50 steps.
- **Training:** per-frame independent noise levels over all frames (no past/future distinction), noise-prediction loss (Eq. 4), which optimizes a reweighted ELBO (Thm 4.1). It can be obtained by fine-tuning an existing full-sequence model.
- **Conditioning:** noise level, actions (Minecraft), camera poses (RE10K) and robot actions are injected per token via AdaLN. The robot model diffuses the next 15 actions together with the next frame.
- **History Guidance:** composes scores with different history subsets (temporal, HG-t) and partially noised history (fractional, HG-f, a low-pass filter) as a CFG-like weighted sum (Eq. 5–6).
- **Compute:** 12×H100 for about 5 days per video model; robot model on 4×RTX 4090 for 4 h.

## 6. Conclusions and future work
- **Authors:** DFoT is a competitive video diffusion framework, and History Guidance significantly improves quality, consistency and motion. DFoT can be fine-tuned from existing models, which suggests future integration with foundation video models. Explicit consistency enforcement and more sophisticated (frequency-band) sampling strategies are left for future work (App. A and Sec. 5).
- **Own observations:**
  - Evaluations are at low resolution (128 to 256 px) on research datasets, with no large-scale text-to-video setting.
  - The "world model" uses are limited to Minecraft action conditioning, camera-pose conditioning and one robot task. There is no planning or RL with the model.
  - The guidance weights for the robot composition (0.2/0.45/0.45) are hand-set.
  - Robot code and weights do not appear to be released.

## 7. Sources and verification
- Paper PDF (arXiv v2, camera-ready): https://arxiv.org/pdf/2502.06764 (read Sec. 1 to 7, App. C, D)
- PMLR: https://proceedings.mlr.press/v267/song25b.html ; ICML poster page https://icml.cc/virtual/2025/poster/44316
- Code: https://github.com/kwsong0113/diffusion-forcing-transformer ; LICENSE https://raw.githubusercontent.com/kwsong0113/diffusion-forcing-transformer/main/LICENSE
- Weights: https://huggingface.co/kiwhansong/DFoT (file list via HF API)
- Citations: https://api.semanticscholar.org/graph/v1/paper/arXiv:2502.06764
- Not verified: GitHub last-update date (API rate limit), and whether the `full_models/` checkpoints include optimizer states (not documented on the model card page I read).
