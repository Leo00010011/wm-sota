---
citation_count: 32
citation_source: "Semantic Scholar, 2026-10-13 (carried over from file; live recheck rate-limited 2026-10-15)"
venue: "CVPR 2025 (oral)"
date: 2025-03-06
research_group: "Stanford (Jiajun Wu)"
---

# FluidNexus: 3D Fluid Reconstruction and Prediction from a Single Video

- **Authors:** Yue Gao*, Hong-Xing Yu*, Bo Zhu, **Jiajun Wu** (*equal contribution)
- **Group role:** senior (last) author; first authors are Wu's group members at Stanford
- **Date published:** 2025-06 (CVPR 2025 conference, June 2025; arXiv v1 2025-03-06, v2 2025-07-09)
- **Venue:** CVPR 2025, **Oral**, proceedings pp. 26091–26101 (DOI 10.1109/CVPR52734.2025.02430)
- **Links:**
  - arXiv: https://arxiv.org/abs/2503.04720
  - CVF: https://openaccess.thecvf.com/content/CVPR2025/html/Gao_FluidNexus_3D_Fluid_Reconstruction_and_Prediction_from_a_Single_Video_CVPR_2025_paper.html
  - Oral page: https://cvpr.thecvf.com/virtual/2025/oral/35372
  - Project page: https://yuegao.me/FluidNexus/
  - Code: https://github.com/ueoo/FluidNexus
- **Number of citations:** 32 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
FluidNexus is a physics-structured generative world model for fluids, such as smoke plumes.

From one fixed-view video it does three things:
- Synthesizes novel-view videos with a frame-wise Zero123 model plus a CogVideoX refiner.
- Reconstructs the 3D fluid velocity and appearance with coupled "physical" particles (differentiable Position-Based Fluids) and "visual" particles (3D Gaussians).
- Predicts future evolution through "generative fluid simulation", where simulator rollouts are refined by video diffusion. This also covers interactions such as added wind or an inserted object.

## 1. Weights availability
- **Released fine-tuned weights:** Hugging Face https://huggingface.co/yuegao/FluidNexusModels, linked from the repo README. The fine-tuned models are Zero123 for novel views and LoRA-fine-tuned CogVideoX-5B refiners, one per dataset.
- **Base models:** the README downloads the Zero123-XL base checkpoint from zero123.cs.columbia.edu, plus CogVideoX.
- **License:** the repo is MIT. The license of the HF model card was not checked.

## 2. Code
- **Repository:** https://github.com/ueoo/FluidNexus, MIT license (GitHub search API).
- **Last activity:** pushed 2026-03-22.
- **Contents:**
  - Data processing.
  - Zero123 and CogVideoX fine-tuning, so training code for the generative parts is included.
  - The FluidDynamics reconstruction and prediction code, with custom Gaussian rasterizers.

## 3. Datasets
- **ScalarFlow** (public): 104 real plume scenes, each with 5 synchronized views. 94 are used for fine-tuning and 10 are held out.
- **FluidNexus-Smoke** (new): 120 scenes of a smoke jet in front of a textured background. 5 synchronized views along a ~120° arc, 6 s at 30 fps (180 frames).
- **FluidNexus-Ball** (new): 120 scenes like FluidNexus-Smoke, with a shiny ball above the ejector to create fluid–solid interaction.
- **Splits:** 100 scenes for training and 20 held out (Sec. 4).
- **Availability:** released on Hugging Face at https://huggingface.co/datasets/yuegao/FluidNexusDatasets, as processed, "All" and raw-video versions.

## 4. Evaluation and downstream tasks
- **Protocol:**
  - The middle camera is the input; the other 4 views are ground truth.
  - The first 120 frames are reconstructed and the next 60 frames are predicted.
  - Metrics: PSNR, SSIM and LPIPS, plus mean velocity divergence ∇·V for re-simulation.
- **Baselines:** PINF and HyFluid (multi-view physics-informed fluid reconstruction), and SpacetimeGaussians (4D reconstruction).
- **PSNR results (Tables 1–3):**

  | Dataset | Novel-view synthesis | Input-view future prediction | Novel-view future prediction | Re-simulation |
  |---|---|---|---|---|
  | ScalarFlow (Table 1) | 32.45 (best baseline 22.68) | 28.51 (HyFluid 26.84) | 26.83 (best baseline 20.66) | 32.44 |
  | FluidNexus-Smoke (Table 2) | 30.62 (22.64) | 27.79 (PINF 26.48) | 25.74 (22.66) | 30.61 |
  | FluidNexus-Ball (Table 3) | 29.89 (20.71) | 27.70 (STG 25.80) | 24.82 | 29.88 |

- **Ablations (Table 4, Smoke):**
  - Novel-view synthesis PSNR falls from 30.62 to 21.88 without the novel-view synthesizer.
  - Re-simulation PSNR falls from 30.61 to 28.00 without the physics loss.
- **Interaction simulation:** adding wind or a ball after reconstruction (Figs. 7–8). Results are qualitative only, since no ground truth exists.
- **Tasks evaluated:** 3D reconstruction, future prediction and interaction simulation. There is no robotics or control task.

## 5. Architecture and techniques
- **Novel-view video synthesizer:**
  - Zero123, fine-tuned and camera-conditioned, generates per-frame novel views at 256×256.
  - CogVideoX-5B, LoRA-fine-tuned, refines them SDEdit-style into 49-frame, 720×480 clips. λ_SDEdit is 0.5 for reconstruction and 0.75 for prediction.
  - Long videos are produced with an unconditional refiner followed by a refiner conditioned on previous frames.
- **Representation:**
  - Physical particles carry velocity and density through kernel interpolation, simulated with differentiable Position-Based Fluids.
  - Visual 3D Gaussians are passively advected by the velocity field.
- **Losses:**
  - Visual: L1 + SSIM.
  - Physics: a simulation-consistency term L_sim plus incompressibility terms at t and t+1 and between visual particles. Weights are λ = 0.1.
  - A temporal regularizer on appearance.
  - Optimization is sequential and two-stage per timestep (dynamics first, then appearance).
- **Prediction ("generative fluid simulation"):**
  1. Roll out PBF from the reconstructed state.
  2. Render rough multi-view videos.
  3. Refine them with the video model.
  4. Re-run reconstruction on the refined videos.
- **Action conditioning:** external forces (wind) and inserted rigid objects, applied as PBD constraints.

## 6. Conclusions and future work
- **Authors' conclusions and limitations:**
  - Video generation gives strong support for single-video fluid prediction.
  - Limitations: reconstructions on the new datasets are blurry and miss fine details.
  - The method does not generalize to new backgrounds or objects, because the synthesizer is fine-tuned on a few scenes.
  - Future work: large-scale multi-view fluid video datasets, synthetic or real.
- **Own observations:**
  - The generative components are fine-tuned per dataset, so the "world model" is scene-family-specific rather than general.
  - The interaction results are qualitative only.
  - The table values above come from pdftotext extraction of multi-column tables. The PSNR values are consistent with the Table 4 ablations (30.62 / 30.61), but the SSIM and LPIPS values were not transcribed for that reason.

## 7. Sources and verification
- arXiv PDF v2 https://arxiv.org/pdf/2503.04720: Secs. 1–5, Tables 1–4.
- CVF and CVPR virtual pages (oral) found via web search. Venue and DOI confirmed via the Semantic Scholar API (`paper/arXiv:2503.04720`).
- Repo README: https://github.com/ueoo/FluidNexus. The license and push date come from the GitHub search API.
- **Could not open:** the project page yuegao.me, because DNS resolution failed from this environment.
