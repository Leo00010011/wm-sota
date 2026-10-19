# Athena Research Center (Archimedes) / Univ. of Crete + valeo.ai — Nikos Komodakis, Spyros Gidaris

- **PIs:** Nikos Komodakis (Archimedes/Athena RC; University of Crete; IACM-FORTH) and Spyros Gidaris (valeo.ai, Paris). Main student/researchers on this line: Efstathios Karypidis (Archimedes, NTUA), Ioannis Kakogeorgiou (Archimedes).
- **Pages:** Komodakis publications https://www.csd.uoc.gr/~komod/publications.html · Archimedes https://archimedesai.gr/ · valeo.ai https://valeoai.github.io/ · code for this line https://github.com/Sta8is
- **Research line:** forecast the future in the feature space of frozen vision foundation models (DINOv2) or in semantic label space, then decode to semantics, depth or pixels. Evaluated on driving video (Cityscapes, nuScenes, CoVLA, KITTI). No action conditioning or closed-loop control yet.

## Included papers

| Title | Venue | Year | Citations (Semantic Scholar, 2026-10-13) |
|---|---|---|---|
| [Advancing Semantic Future Prediction through Multimodal Visual Sequence Transformers (FUTURIST)](futurist.md) | CVPR (poster) | 2025 | 12 |
| [DINO-Foresight: Looking into the Future with DINO](dino-foresight.md) | NeurIPS (poster) | 2025 | 52 |
| [Representations Before Pixels: Semantics-Guided Hierarchical Video Prediction (Re2Pix)](re2pix.md) | ECCV (poster) | 2026 | 1 |

All three have Komodakis as last author and Gidaris as co-author, so they belong to this folder. All three release code (MIT) and weights on Hugging Face (Re2Pix: inference code and Cityscapes weights only, no training scripts yet).

## Cross-group papers
None found. No top-venue world-model paper was found where Komodakis or Gidaris is a non-senior co-author with a senior author from another group in the summary file.

## Excluded items

| Item | Reason |
|---|---|
| Latent-Foresight: End-to-End Learning Predictable Representations for Latent World Models (Karypidis, Gidaris, Komodakis; arXiv 2610.01942, 2026-10-01) | arXiv preprint only; no venue on arXiv, GitHub README or S2 (0 citations). In scope topically (joint tokenizer + flow-matching dynamics world model; code and weights at https://github.com/Sta8is/Latent-Foresight, HF `Sta8is/Latent-Foresight_cs`, `Sta8is/Latent-Foresight_csnscovla`). Re-check if it gets accepted. |
| ReDi — Boosting Generative Image Modeling via Joint Image-Feature Synthesis (Kouzelis, Karypidis, Kakogeorgiou, Gidaris, Komodakis; NeurIPS 2025) | Not a world model: class-conditional ImageNet image generation that jointly diffuses VAE latents and DINOv2 features. No temporal prediction, no future states, no actions. Accepted at NeurIPS 2025 (proceedings listing confirmed); "Spotlight" status from the summary not confirmed. Shares the "representations + pixels" idea that Re2Pix later applies to video. |
| CoReDi (coevolving representations for joint image generation; arXiv 2604.17492, reported as ECCV 2026 in a co-author post) | Image generation, not a world model; acceptance not verified separately. |
| EQ-VAE (ICML 2025), Multi-Token Prediction Needs Registers (NeurIPS 2025), DIP (ICCV 2025), SPOT (CVPR 2024), MOCA (TMLR 2024) and other group papers | Representation learning / image generation / LLM training, not world models. |
| Valeo4Cast: A Modular Approach to End-to-End Forecasting (Gidaris co-author) | ECCV 2024 workshop only; trajectory-forecasting pipeline, not a world model. |
| OccFeat (Gidaris co-author) | CVPR 2024 workshop; BEV pretraining, not a world model. |
| VaViM / VaVAM — Autonomous Driving through Video Generative Modeling (valeo.ai, arXiv 2502.15672) | valeo.ai driving video world model, but Gidaris was not found among its authors and no top-venue acceptance was found. Not this group's paper. |

## Method
- Sources checked: summary file section 4; arXiv full text (HTML) of all three included papers and the arXiv abstract pages of Re2Pix and Latent-Foresight; CVPR 2025 Open Access and CVPR virtual poster page; NeurIPS 2025 virtual poster page and OpenReview link; ECCV 2026 virtual poster page (eccv.ecva.net/virtual/2026/poster/5276) and the preliminary ECCV 2026 accepted-papers list; GitHub repos (README, license, push dates via GitHub API) and Hugging Face model repos (HF API) for all papers; mlanthology author page for Gidaris; IACM-FORTH preprint repository listing for Komodakis/Gidaris; web searches for 2025-2026 papers by Komodakis, Gidaris, Karypidis.
- DBLP pages and the arXiv author API were blocked (bot protection / rate limit), so the publication search relied on mlanthology, the IACM-FORTH listing and web search.
- Citation counts: Semantic Scholar Graph API, checked 2026-10-13.

## Open issues
- Re2Pix: the summary's "ECCV 2026 per co-author post" is now confirmed by the official ECCV 2026 poster page (2026-09-10 session). Numbers in re2pix.md are from arXiv v1, not the ECCV camera-ready PDF.
- DINO-Foresight summary claim "autoregressive rollout from 4 context frames to 48 future frames (about 2.9 s)" was not found in the parts of the paper read; the evaluated horizons are 0.18 s / 0.54 s (Cityscapes) and up to 1.5 s (nuScenes). Treat the 48-frame figure as unverified.
- FUTURIST: summary says "state of the art at short (0.18 s) and mid (0.54 s) horizons" — confirmed (Table 1, vs PFA).
- The summary's assessment "open code and weights" is correct for FUTURIST and DINO-Foresight; Re2Pix has weights and inference code but no training code yet.
- ReDi Spotlight status not confirmed (only acceptance as NeurIPS 2025 main-track paper).
- Google Scholar counts were not checked; Semantic Scholar counts may lag, especially for Re2Pix (S2 still lists it as arXiv).
- Gidaris publishes many valeo.ai papers with other senior authors; a full DBLP sweep was not possible, so a valeo.ai driving world-model paper with Gidaris as a middle author could have been missed. None was found by search.
