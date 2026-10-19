# Tsinghua University: Mingsheng Long (THUML); Jun Zhu and Chongxuan Li (TSAIL)

- **PIs:**
  - Mingsheng Long, School of Software, Tsinghua University; THUML lab: https://ise.thss.tsinghua.edu.cn/~mlong/ (GitHub: https://github.com/thuml)
  - Jun Zhu, Dept. of Computer Science and Technology, Tsinghua University; TSAIL group (GitHub: https://github.com/thu-ml); also chief scientist of Shengshu Technology (Vidu)
  - Chongxuan Li, Gaoling School of AI, Renmin University of China (a frequent co-author with Jun Zhu)
- **Research line:** Long's group works on scalable, interactive world models: Dreamer-style MBRL, autoregressive video world models (iVideoGPT), trajectory world models, RL post-training of world models, and video diffusion to interactive world models. Zhu's group works on video foundation models (Vidu) and embodied unified world models (Motus).

## Included papers (top venue, accepted, group PI senior author)

Citation counts are from Semantic Scholar, checked 2026-10-13.

| Paper | Venue | Year | Senior PI | Citations |
|---|---|---|---|---|
| [Pre-training Contextualized World Models with In-the-wild Videos for RL (ContextWM)](contextwm.md) | NeurIPS | 2023 | Long | 55 |
| [HarmonyDream: Task Harmonization Inside World Models](harmonydream.md) | ICML | 2024 | Long | 28 |
| [iVideoGPT: Interactive VideoGPTs are Scalable World Models](ivideogpt.md) | NeurIPS | 2024 | Long | 180 |
| [Trajectory World Models for Heterogeneous Environments (TrajWorld)](trajworld.md) | ICML | 2025 | Long | 5 |
| [RLVR-World: Training World Models with Reinforcement Learning](rlvr-world.md) | NeurIPS | 2025 | Long | 52 |
| [Vid2World: Crafting Video Diffusion Models to Interactive World Models](vid2world.md) | ICLR | 2026 | Long | 73 |
| [Foresight Diffusion (ForeDiff), borderline](foresight-diffusion.md) | ICLR | 2026 | Long | 1 |
| [Motus: A Unified Latent Action World Model](motus.md) | CVPR | 2026 | Jun Zhu | 303 |

No included paper has Chongxuan Li as senior author. He is a co-author of minWM and Causal Forcing, both excluded below.

## Cross-group papers
None found. No top-venue world-model paper in this search had a senior or corresponding author who is a PI of another group in the summary file.

## Excluded items

| Item | Venue / status | Reason |
|---|---|---|
| minWM: A Full-Stack Open-Source Framework for Real-Time Interactive Video World Models (arXiv 2605.30263; last author Jun Zhu, co-author Chongxuan Li) | arXiv only (v1 2026-05-28, v2 2026-09-22) | No venue acceptance found: the arXiv comments field is empty and no proceedings listing exists. Code and weights: https://github.com/shengshu-ai/minWM |
| Causal Forcing (arXiv 2602.02214; Zhu, Li et al.) | ICML 2026 (arXiv comment) | Real-time autoregressive text-to-video distillation. No action or camera conditioning, and "world model" appears only as a motivating application. Not a world model. |
| Causal Forcing++ (arXiv 2605.15141) | arXiv only | Preprint; text-to-video distillation. |
| Vidar: Embodied Video Diffusion Model for Generalist Manipulation (arXiv 2507.12898; Su, Zhu) | arXiv only (v4 2025-12-20) | No venue found. |
| Vidu S1 / Vidu S2 (arXiv 2607.03118, 2609.11638; Zhu) | arXiv only | Preprints / technical reports. |
| Visual Generation Unlocks Human-Like Reasoning through Multimodal World Models (arXiv 2601.19834; Long) | ICLR 2026 workshop (World Models workshop poster) | Workshop only. Long's lab page lists it as an arXiv preprint. |
| JEPA-VLA: Video Predictive Embedding is Needed for VLA Models (Long) | arXiv preprint (lab page) | Preprint. |
| CompilerDream: Learning a Compiler World Model for General Code Optimization (Long) | KDD 2025 | KDD is not in the top-venue list. |
| Dynamical Diffusion (DyDiff), arXiv 2503.00951 (Long) | ICLR 2025 | Borderline. A general temporal predictive diffusion framework (scientific forecasting, video prediction including action-conditioned BAIR and RoboNet, and time series), with no world-model framing. Excluded; it could be added if the orchestrator wants a broader "video prediction" scope. |
| PredRNN (TPAMI 2023), ModeRNN (TPAMI 2023), MotionRNN (CVPR 2021) (Long) | Top venues | Spatiotemporal or video predictive learning, not framed as world models. Whether PredRNN-TPAMI has action-conditioned experiments was not checked. |
| Continual Predictive Learning from Videos (CVPR 2022) | Top venue | Video prediction. Senior author is Xiaokang Yang (SJTU), not a group PI. |
| Out-of-Dynamics Imitation Learning from Multimodal Demonstrations (CoRL 2022; Long) | Top venue | Imitation learning; no world model. |
| NowcastNet (Nature 2023; Long, Jordan, Wang) | Nature | Not in the venue list. Precipitation nowcasting. |
| Contrastive Energy Prediction for Offline RL (ICML 2023; Zhu) | Top venue | Diffusion policy / energy guidance; no world model. |
| RIFLEx (ICML 2025; Zhu, Li) | Top venue | Video length extrapolation; not a world model. |

## Method
- **Sources:** the summary file (section 6); Long's lab homepage and full publication list (ise.thss.tsinghua.edu.cn/~mlong); Semantic Scholar author pages (Long ID 35776445 and related IDs, Jun Zhu ID 2259945495, Chongxuan Li ID 2399563; these records are fragmented); web searches for Jun Zhu, Hang Su, Hengkai Tan and Chongxuan Li world-model papers; arXiv abstract pages and full PDFs (read via pdftotext); NeurIPS, ICML, ICLR and CVPR virtual and proceedings pages; GitHub READMEs; Hugging Face model and dataset pages.
- **Not reachable:** DBLP was blocked by bot protection, and OpenReview by a verification page.
- **Citation counts:** Semantic Scholar Graph API batch endpoint, checked 2026-10-13.

## Open issues
- The summary lists only iVideoGPT and minWM. minWM has no top-venue acceptance as of 2026-10-13, so it is excluded. Seven further Long papers and one Zhu paper (Motus, CVPR 2026) were added.
- The summary says iVideoGPT was "pretrained on millions of manipulation trajectories". The paper reports about 1.42M trajectories (OXE plus an SSv2 subset), so "over one million" is more precise.
- Jun Zhu's and Chongxuan Li's records are hard to search: Semantic Scholar splits their profiles and DBLP was blocked. Other top-venue world-model papers from TSAIL may exist, for example robot video-diffusion work under Hang Su as last author. Motus was the only one found with Zhu as last author.
- ForeDiff and DyDiff are borderline scope calls. ForeDiff is included because its arXiv v1 was titled "Consistent World Models via Foresight Diffusion" and it targets action-conditioned prediction. DyDiff is excluded.
- Presentation types (oral or spotlight vs poster) were confirmed only from virtual-site "poster" pages. The HarmonyDream PMLR URL and the Motus CVF open-access URL were not found.
- TrajWorld weights are not yet released, and ForeDiff has no public code or weights.
- ForeDiff's citation count (1) is probably undercounted, because the Semantic Scholar record may be split by the title change.
