# HKU / OpenDriveLab: Hongyang Li (with HKUST, CUHK)

- **PI:** Hongyang Li, The University of Hong Kong. He leads OpenDriveLab (https://opendrivelab.com), which was previously part of Shanghai AI Laboratory; papers up to 2024 list "OpenDriveLab and Shanghai AI Lab". Homepage: https://lihongyang.info/. GitHub: https://github.com/OpenDriveLab.
- **Frequent co-advisors:**
  - Li Chen (HKU / OpenDriveLab)
  - Jun Zhang (HKUST)
  - Xiangyu Yue (CUHK)
  - Andreas Geiger and Kashyap Chitta (Tübingen / NVIDIA)
  - Ping Luo (HKU)
- **Research line:** generative world models for autonomous driving:
  - large-scale driving video prediction (GenAD/OpenDV, then Vista);
  - action-conditioned 4D forecasting (ViDAR);
  - layout-level traffic world models and simulators (Nexus).

  Since 2024 the same team has carried the "world model as simulator plus reward" recipe into robot manipulation (CLOVER, RISE).

## Included papers (top venue, accepted; Hongyang Li last or corresponding author, or an OpenDriveLab co-authored paper with no summary-PI senior author)

Citation counts are from Semantic Scholar, checked 2026-10-15.

| Paper | Venue | Year | Senior PI | Citations |
|---|---|---|---|---|
| [ViDAR: Visual Point Cloud Forecasting enables Scalable Autonomous Driving (borderline)](vidar.md) | CVPR (Highlight) | 2024 | Hongyang Li (last) | 142 |
| [GenAD: Generalized Predictive Model for Autonomous Driving](genad.md) | CVPR (Highlight) | 2024 | Hongyang Li (last, project lead) | 197 |
| [Vista: A Generalizable Driving World Model with High Fidelity and Versatile Controllability](vista.md) | NeurIPS | 2024 | Hongyang Li (last; † with Geiger and Zhang) | 414 |
| [CLOVER: Closed-Loop Visuomotor Control with Generative Expectation (borderline)](clover.md) | NeurIPS | 2024 | Hongyang Li (last, co-project lead) | 82 |
| [Nexus: Decoupled Diffusion Sparks Adaptive Scene Generation (borderline)](nexus.md) | ICCV | 2025 | Hongyang Li (last) | 16 |
| [ReSim: Reliable World Simulation for Autonomous Driving](resim.md) | NeurIPS (Spotlight) | 2025 | Co-author (7th of 10); corresponding authors Li Chen and Xiangyu Yue, not summary PIs | 54 |
| [RISE: Self-Improving Robot Policy with Compositional World Model](rise.md) | RSS | 2026 | Hongyang Li (last; equal advising with Xiangyu Yue) | 48 |

ReSim is kept under the 07_stanford_wu precedent (as with MaskViT and PhysDreamer there): a co-authored paper from the group whose senior author is not a PI in the summary. It is an OpenDriveLab project (opendrivelab.com/ReSim, github.com/OpenDriveLab/ReSim).

## Cross-group papers
None found. No top-venue world-model paper was found where Hongyang Li is a non-senior co-author and the senior or corresponding author is a PI of another group in the summary file.

## Excluded items

| Item | Venue / status | Reason |
|---|---|---|
| SimScale: Learning to Drive via Real-World Simulation at Scale (arXiv 2511.23369; Hongyang Li last) | CVPR 2026 Oral | Not a learned world model. It builds a sim-and-real co-training pipeline: 3DGS-style neural rendering of perturbed real scenes, a pseudo-expert for labels, and reactive agents. "World model" appears only in related work. |
| World Engine: Towards the Era of Post-Training for Autonomous Driving (arXiv 2606.19836; Hongyang Li last) | Technical report (arXiv comment) | No venue acceptance. |
| OMEGA: Optimization-Guided Diffusion for Interactive Scene Generation (arXiv 2512.07661; Hongyang Li last) | Preprint; NVIDIA lists it as "Submitted" | No venue found. |
| SparseVideoNav: Sparse Video Generation Propels Real-World Beyond-the-View VLN (arXiv 2602.05827; Hongyang Li last) | Preprint | No venue found (arXiv comment empty). Video generation for navigation. |
| FLARE: Learning Future-Aware Latent Representations from VLMs for Autonomous Driving (arXiv 2601.05611; Hongyang Li last) | Preprint | No venue found. Future-feature prediction as an auxiliary objective, borderline even if accepted. Do not confuse with NVIDIA's FLARE (CoRL 2025). |
| Learning Manipulation by Predicting Interaction (MPI; arXiv 2406.00439; Hongyang Li last) | RSS 2024 | Representation pre-training: it predicts interaction frames and detects objects. Not framed as or used as a world model. Borderline, excluded. |
| UniVLA: Learning to Act Anywhere with Task-centric Latent Actions (arXiv 2505.06111; Hongyang Li last) | RSS 2025 | Latent-action VLA; no predictive world model is used for control. |
| SimVTA: Simulation-Grounded Video-Trajectory Alignment for Driving World Action Models | Preprint (OpenDriveLab page) | Hongyang Li is not an author on arXiv; last author is Zhenjie Yang. |
| DriveZero (arXiv 2609.06055), On the Identifiability of Controlled World Models (2607.22430), Orca: The World is in Your Mind (2606.30534) | Preprints | A "Hongyang Li" is a middle author. The last authors are Naiyan Wang, Shengbo Eben Li and Pengwei Wang, none of them summary PIs. All are preprints. |
| DriveWorld (CVPR 2024, Peking University), Doe-1 (Tsinghua, Jiwen Lu / Wenzhao Zheng) | n/a | Named in the task as possible leads. Neither has Hongyang Li as an author; not OpenDriveLab papers. |
| OpenDV | n/a | Not a separate paper. It is the dataset introduced in GenAD (covered in genad.md). |
| NAVSIM (NeurIPS 2024 D&B), Pseudo-Simulation (CoRL 2025), Centaur | Top venues | Hongyang Li is not last author, and these are benchmarks or planners, not world models. |

## Method
- **Sources:**
  - the summary file (section 10);
  - arXiv API author query `au:"Hongyang Li"` (147 entries, filtered for last authorship);
  - the OpenDriveLab publications page;
  - arXiv PDFs read in full via pdftotext (GenAD, Vista, ReSim, ViDAR, Nexus, CLOVER and RISE; SimScale skimmed for scope);
  - GitHub READMEs and the GitHub REST API (stars, licenses);
  - Semantic Scholar Graph API batch endpoint (citations, checked 2026-10-15);
  - web searches for venue confirmation (ICCV 2025 CVF listing, RSS 2026 program page).
- **Not reachable:** CVF open-access pages returned 403, and the `gh` CLI was not installed (the GitHub REST API was used through curl). The shared group-instructions file named in the task did not exist at the given path, so the format follows the existing folders (for example 06_tsinghua_long_zhu).

## Open issues
- **ReSim ownership (resolved).** ReSim's corresponding authors are Li Chen and Xiangyu Yue, not Hongyang Li. At the coordinator's request it is included under the 07_stanford_wu co-author precedent; see resim.md.
- **Summary figure checks:**
  - **Vista "+55% FID, +27% FVD":** correct as relative reductions, but against different baselines. FID is 6.9 vs GenAD's 15.4; FVD is 89.4 vs Drive-WM's 122.7, on nuScenes. Baseline numbers were copied from the original papers. The multiple action modalities are confirmed (angle and speed, trajectory, command, goal point).
  - **ReSim "up to +44% visual fidelity, +2% NAVSIM planning, +25% policy selection":** matches the abstract.
    - The 44% is FVD 50.4 vs Vista's 89.4 on nuScenes; FID improves 6.9 → 5.2, about 25%.
    - The +2% is absolute PDMS: 86.6 vs LAW's 84.6 on NAVSIM navtest.
    - The +25% is absolute PDMS for reward-steered selection: 74.1 vs about 47.5 for the candidate policies. The Table 3 caption says +26%.
    - The abstract also claims more than 50% better controllability, which the summary omits.
  - **GenAD "OpenDV with more than 1,700 hours":** OpenDV-YouTube is 1,747 h; the full OpenDV-2K (with 7 public datasets) is 2,059 h. The paper abstract says "over 2000 hours". GenAD is image-and-text-to-video (latent diffusion from SDXL); action conditioning is only a nuScenes fine-tune (GenAD-act).
- **RISE abstract inconsistency.** Its "+45% backpack packing" is measured against RECAP (40%); the strongest baseline is π0.5 + DAgger (50%), giving +35. The main text says "40% margin".
- **Borderline scope calls:**
  - ViDAR: action-conditioned point-cloud forecasting, used as pre-training.
  - Nexus: layout-level generative traffic world model.
  - CLOVER: text-conditioned video planner, not action-conditioned.

  Drop these three if the folder should hold only sensor-level action-conditioned world models.
- **Presentation types** (Highlight, Spotlight) rest on arXiv comments, READMEs and the lab page; CVF and NeurIPS pages were not opened. Vista's NeurIPS presentation type is unknown.
- **Weights:**
  - GenAD weights were never released.
  - Nexus's HF checkpoint URL was not opened.
  - RISE releases only the pre-trained dynamics model, not the task-specific value or policy checkpoints.
  - ReSim's CARLA-trained (non-expert) weights are still a TODO item.
