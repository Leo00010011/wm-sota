# University of Geneva — François Fleuret group (Eloi Alonso, Vincent Micheli)

- **PI:** François Fleuret, Professor at the University of Geneva (Machine Learning group). He is also a Research Scientist at Meta FAIR and an External Research Fellow at Idiap.
- **Key students (world-model line):** Eloi Alonso and Vincent Micheli. Both finished PhDs in the group and are now co-founders of General Intuition (2025-).
- **Pages:** https://fleuret.org/francois/ · publications https://fleuret.org/francois/publications.html · https://eloialonso.github.io/
- **Line:** generative world models for sample-efficient RL, trained in imagination: transformer over discrete tokens (IRIS), then context-aware delta tokens (Delta-IRIS), then pixel-space diffusion (DIAMOND).

## Included papers

| Paper | Venue | Year | Citations (Google Scholar, 2026-10-13) |
|---|---|---|---|
| [Transformers are Sample-Efficient World Models (IRIS)](iris.md) | ICLR (notable top 5%, oral) | 2023 | 551 |
| [Efficient World Models with Context-Aware Tokenization (Delta-IRIS)](delta-iris.md) | ICML | 2024 | 56 |
| [Diffusion for World Modeling: Visual Details Matter in Atari (DIAMOND)](diamond.md) | NeurIPS (spotlight) | 2024 | 426 |

Fleuret is the last author on all three.

## Cross-group papers
- None found. No top-venue world-model paper with a Geneva co-author has a senior author from another group in the summary file.

## Excluded items
- **Towards Efficient World Models** (Alonso, Micheli, Fleuret, 2023): ES-FoMo workshop at ICML 2023. Workshop only.
- **Multiplayer Interactive World Models with Representation Autoencoders** (arXiv 2607.05352, July 2026; Hu et al., with Alonso, Micheli and Jelley among 27 authors): arXiv "Technical report", no venue. Industry work (General Intuition / partners), and Fleuret is not an author.
- **CWM: An Open-Weights LLM for Research on Code Generation with World Models** (arXiv 2510.02387, 2025; Meta FAIR CodeGen team, Fleuret among many authors): arXiv only, no top-venue acceptance found. It is a code LLM, not a world model in this report's scope, and the Geneva group is not the senior author.
- **MineRL Diamond 2021 Competition report** (NeurIPS 2021 Competition track, 2022): not a world-model paper.
- **Other Fleuret 2025-26 papers** (Pareto low-rank adapters, ICLR 2025; LiNeS, ICLR 2025; The Free Transformer, Thinking Slow Fast, Laminating representation autoencoders, all arXiv): not world-model papers.

## Answers to the brief
- IRIS: yes, ICLR 2023, notable top 5% with an oral presentation. An earlier version won the best paper award at the NeurIPS 2022 Deep RL Workshop (per Alonso's page).
- Delta-IRIS: yes, ICML 2024 (PMLR 235).
- 2025-2026 top-venue world-model papers from this group: none found. The summary's assessment holds: the line has moved to industry (General Intuition). The only 2026 follow-up is the Multiplayer Interactive World Models technical report, which is arXiv-only.

## Method
- Sources: Fleuret's publication page (lists up to 2024); Fleuret's Google Scholar profile sorted by date (first 60 of 73 entries, covering 2021-2026); arXiv pages and full HTML of the three papers; PMLR, NeurIPS and ICLR virtual pages; GitHub repos and Hugging Face model pages; Alonso's homepage; Micheli's alphaXiv profile; web searches (standard and extended) for 2025-2026 Alonso / Micheli / Fleuret world-model papers; a Google Scholar search for Micheli/Alonso from 2025.
- Citation counts: Google Scholar (Fleuret profile), checked 2026-10-13.
- Blocked: DBLP (bot check), Semantic Scholar API (HTTP 429), arXiv API (HTTP 429 / no response), OpenReview (bot check).

## Open issues
- Citation counts come only from Google Scholar. Semantic Scholar could not be checked.
- DBLP could not be read, so the completeness check relied on Google Scholar and the lab page. Workshop-level or non-Fleuret papers by Alonso or Micheli in 2025-26 could in principle be missing, but no candidate appeared in any search.
- Delta-IRIS presentation type (poster vs. oral/spotlight) was not checked beyond the ICML virtual "poster" page.
- Disagreements with the summary: none material. The summary did not give venues for IRIS (ICLR 2023) or Delta-IRIS (ICML 2024); both are now confirmed. The DIAMOND numbers (mean HNS 1.46, NeurIPS 2024 spotlight, CS:GO engine) match the paper.
