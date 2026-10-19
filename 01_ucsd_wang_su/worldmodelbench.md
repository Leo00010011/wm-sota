---
citation_count: 118
citation_source: "Semantic Scholar, 2026-10-13"
venue: "NeurIPS 2025 (Datasets and Benchmarks Track)"
date: 2025-02-28
research_group: "UC San Diego (Xiaolong Wang, Hao Su)"
---

# WorldModelBench: Judging Video Generation Models As World Models

- **Authors:** Dacheng Li, Yunhao Fang, Yukang Chen, Shuo Yang, Shiyi Cao, Justin Wong, Michael Luo, **Xiaolong Wang**, Hongxu Yin, Joseph E. Gonzalez, Ion Stoica, Song Han, Yao Lu (Li and Fang equal contribution)
- **Group role:** co-author (Xiaolong Wang, UCSD/NVIDIA). Co-first author Yunhao Fang is a UCSD student. The last and corresponding authors are Yao Lu (NVIDIA) and Song Han (MIT/NVIDIA); neither is a PI of any group in the summary, so the paper is kept here.
- **Date published:** 2025-12 (NeurIPS 2025 Datasets & Benchmarks Track; poster session 2025-12-03). arXiv 2025-02-28.
- **Venue:** NeurIPS 2025, Datasets and Benchmarks Track, poster ("Accept (poster)" in the NeurIPS 2025 virtual-site data)
- **Links:** arXiv https://arxiv.org/abs/2502.20694 · OpenReview https://openreview.net/forum?id=a3hafrDzuA · NeurIPS https://neurips.cc/virtual/2025/poster/121570 · Project https://worldmodelbenchteam.github.io · Code https://github.com/WorldModelBench-Team/WorldModelBench
- **Number of citations:** 118 (source: Semantic Scholar API, checked 2026-10-13)

## Summary
WorldModelBench evaluates text-to-video and image-to-video generators as world models, not only on video quality. It contains 350 text+image conditions across 7 application domains (robotics, driving, industry, human activities, gaming, animation, natural) and 56 subdomains. Each video gets a 10-point score:
- instruction following, 0–3;
- physics adherence, 0–5 (Newton's first law, mass/solid mechanics, fluids, impenetrability, gravity);
- common sense, 0–2.

The authors collected 67K human labels on 14 models and fine-tuned a 2B VLM judge. Using this judge as a differentiable reward improves a video generator.

## 1. Weights availability
- The judge model is released on Hugging Face: https://huggingface.co/Efficient-Large-Model/vila-ewm-qwen2-1.5b (linked as "the judge" in the README).
- The repo name says "qwen2-1.5b" while the paper describes a "2B" VILA-based judge; this discrepancy is not resolved.
- License of the judge weights not checked.
- The reward-fine-tuned OpenSora weights were not found.

## 2. Code
- https://github.com/WorldModelBench-Team/WorldModelBench. Contains evaluation code (`evaluate.py --model_name ... --video_dir ... --judge ...`).
- Benchmark data was moved from Hugging Face to GitHub in July 2025 (README note).
- No license detected by the GitHub API. Last push 2025-07-29.
- Training code for the judge and for the reward-gradient fine-tuning was not found in the README.

## 3. Datasets
- **Benchmark:** 350 condition pairs, 50 per domain. Reference videos come from:
  - driving: ref [11];
  - robotics: ref [39] (the paper's references include Open X-Embodiment);
  - human activities: ref [10];
  - other domains: ref [38].

  Conditions were captioned with GPT-4o and verified by humans. Released via the GitHub repo.
- **Human labels:** 8,336 complete votes from 65 voters, i.e. 67K labels (Table 2). The paper says the fine-grained labels will be open-sourced (Table 1: "Label Release? Yes").
- **WorldModelBench-Hard:** a 45-prompt subset.

## 4. Evaluation and downstream tasks
Metrics: human scores per dimension, Elo (Bradley–Terry), judge prediction error.
- **Human scores (Table 3):**
  - Best closed models: KLING 8.82 total, Minimax 8.59, Mochi-official 8.37, Runway 8.08, Luma 7.72.
  - Best open models: Mochi 7.62, OpenSoraPlan-T2V 7.61.
  - Weakest: OpenSora-I2V 5.83.
- **Findings (Section 4.1):**
  - Even KLING finishes the instructed task in only 61% of videos; 12% violate mass conservation and 11% show interpenetration.
  - I2V models are worse than their T2V counterparts.
  - Better common-sense (visual quality) scores do not imply better world modelling.
  - Driving, human activities and robotics are the hardest domains (Fig. 7).
- **Judge accuracy (Tables 4–5):** average total-score prediction error 4.1% (maximum 6.81%). Fine-tuned VILA-2B error rates are 32.3% (instruction), 16.4% (common sense) and 29.7% (physics), versus GPT-4o 29.3 / 35.0 / 36.0. The abstract claims 8.6% higher average accuracy than GPT-4o.
- **Correlation with VBench (Fig. 9):** win rates correlate at 0.69 for frame quality but only 0.28 for physics.
- **Reward fine-tuning (Fig. 8):** reward-gradient (VADER-style) fine-tuning of OpenSora-v1.2 with the judge reduces flicker and instruction errors. Results are qualitative only.

## 5. Architecture and techniques
- No world model of its own: this is an evaluation framework for video world models.
- Judge: VILA-2B vision-language model fine-tuned on vote-derived QA pairs plus filtered GPT-4o/Gemini chain-of-thought.
- Reward alignment: maximize the judge reward through differentiable token-probability gaps (Eq. 1), back-propagated into a video diffusion model.

## 6. Conclusions and future work
- **Authors:**
  - General video-quality benchmarks are insufficient for judging world models; physics adherence and instruction following must be measured.
  - Judge-based rewards improve generators.
  - Acknowledged limitations: a small prompt set (350, chosen for inference cost); I2V models need more tuning.
- **Own observations:**
  - Not action-conditioned: "actions" are text instructions, so the benchmark measures text/image-conditioned video prediction, not controllable world models for planning.
  - Physics is scored as binary per law.
  - Wang is a middle author; the group's intellectual role in this paper is limited.

## 7. Sources and verification
- arXiv PDF https://arxiv.org/pdf/2502.20694 v1 (main text, Tables 1–5, Figs. 6–9); GitHub README and API.
- NeurIPS 2025 D&B acceptance confirmed in the NeurIPS 2025 virtual-site data ("Accept (poster)", D&B track).
- Citation count: Semantic Scholar batch API, 2026-10-13.
- Not verified: judge weight license; whether the full 67K human labels are publicly downloadable.
